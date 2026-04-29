# GE MCP OAuth Proxy — Developer Guide

**Project:** Gemini Enterprise × Third-Party MCP Server Integration  
**Author:** Zorro (for Tommy)  
**Stack:** Cloudflare Workers · TypeScript · KV Storage  
**Target MCP Servers:** Alpha Vantage · SEC EDGAR

---

## 1. Background & Problem Statement

Google Gemini Enterprise (GE) Data Store supports MCP servers as a data source, but **requires OAuth 2.0** (Authorization URL + Token URL + Client ID + Client Secret).

Both target MCP servers use simple API key authentication:

| MCP Server | Auth Method | Repo |
|---|---|---|
| Alpha Vantage MCP | `ALPHAVANTAGE_API_KEY` env var | [alphavantage/alpha_vantage_mcp](https://github.com/alphavantage/alpha_vantage_mcp) |
| SEC EDGAR MCP | No auth required (public data) | [stefanoamorelli/sec-edgar-mcp](https://github.com/stefanoamorelli/sec-edgar-mcp) |

**Solution:** Build an OAuth 2.0 Client Credentials proxy on Cloudflare Workers that:
1. Accepts OAuth 2.0 token requests from GE
2. Issues short-lived JWTs stored in KV
3. Proxies validated requests to the target MCP server
4. Injects the API key server-side (never exposed to GE)

```
GE Platform
    │  POST /oauth/token (client_id + client_secret)
    ▼
Cloudflare Worker (this project)
    │  Validates credentials → issues JWT
    │  On MCP tool calls: verifies JWT → injects API key
    ▼
Alpha Vantage MCP / SEC EDGAR MCP
```

---

## 2. Prerequisites

Before starting, ensure you have:

- [ ] Node.js 18+ and npm installed
- [ ] Cloudflare account (free tier is fine)
- [ ] Wrangler CLI: `npm install -g wrangler`
- [ ] Logged in: `wrangler login`
- [ ] Alpha Vantage API key (provided by Zorro)
- [ ] Git access to this project repo

---

## 3. Project Structure

```
ge-mcp-oauth-proxy/
├── src/
│   ├── index.ts          # Main Worker entry point (routing)
│   ├── oauth.ts          # /oauth/authorize + /oauth/token handlers
│   ├── proxy.ts          # MCP proxy logic (forwards + injects API key)
│   ├── jwt.ts            # JWT sign/verify using Web Crypto API
│   └── types.ts          # Shared TypeScript types
├── wrangler.toml         # Cloudflare Worker config
├── package.json
└── tsconfig.json
```

---

## 4. Setup

### 4.1 Init the project

```bash
npm create cloudflare@latest ge-mcp-oauth-proxy -- --type worker-typescript
cd ge-mcp-oauth-proxy
```

### 4.2 Create KV Namespaces

KV is used to store issued tokens and client credentials.

```bash
# Production namespace
wrangler kv:namespace create "TOKEN_STORE"

# Preview namespace (for local dev)
wrangler kv:namespace create "TOKEN_STORE" --preview
```

Copy the returned `id` values into `wrangler.toml`.

### 4.3 Configure `wrangler.toml`

```toml
name = "ge-mcp-oauth-proxy"
main = "src/index.ts"
compatibility_date = "2024-01-01"

[[kv_namespaces]]
binding = "TOKEN_STORE"
id = "REPLACE_WITH_YOUR_KV_ID"
preview_id = "REPLACE_WITH_YOUR_PREVIEW_KV_ID"

[vars]
# Target MCP server URL (change per deployment)
TARGET_MCP_URL = "https://your-alphavantage-mcp.example.com"
TOKEN_TTL_SECONDS = "3600"

# Secrets — set via `wrangler secret put`, NOT here
# ALPHAVANTAGE_API_KEY
# OAUTH_CLIENT_ID
# OAUTH_CLIENT_SECRET
# JWT_SECRET
```

### 4.4 Set Secrets

```bash
wrangler secret put ALPHAVANTAGE_API_KEY
# Paste the API key when prompted

wrangler secret put OAUTH_CLIENT_ID
# e.g.: ge-mcp-client

wrangler secret put OAUTH_CLIENT_SECRET
# Generate a strong random string: openssl rand -hex 32

wrangler secret put JWT_SECRET
# Generate: openssl rand -hex 32
```

---

## 5. Implementation

### 5.1 `src/types.ts`

```typescript
export interface Env {
  TOKEN_STORE: KVNamespace;
  TARGET_MCP_URL: string;
  TOKEN_TTL_SECONDS: string;
  ALPHAVANTAGE_API_KEY: string;
  OAUTH_CLIENT_ID: string;
  OAUTH_CLIENT_SECRET: string;
  JWT_SECRET: string;
}

export interface TokenPayload {
  sub: string;        // client_id
  iat: number;        // issued at (Unix timestamp)
  exp: number;        // expiry (Unix timestamp)
  jti: string;        // unique token ID (for revocation)
}
```

### 5.2 `src/jwt.ts` — JWT using Web Crypto (no external deps)

```typescript
import { TokenPayload } from "./types";

function base64url(buffer: ArrayBuffer): string {
  return btoa(String.fromCharCode(...new Uint8Array(buffer)))
    .replace(/\+/g, "-").replace(/\//g, "_").replace(/=+$/, "");
}

async function getKey(secret: string): Promise<CryptoKey> {
  return crypto.subtle.importKey(
    "raw",
    new TextEncoder().encode(secret),
    { name: "HMAC", hash: "SHA-256" },
    false,
    ["sign", "verify"]
  );
}

export async function signJWT(payload: TokenPayload, secret: string): Promise<string> {
  const header = base64url(new TextEncoder().encode(JSON.stringify({ alg: "HS256", typ: "JWT" })));
  const body = base64url(new TextEncoder().encode(JSON.stringify(payload)));
  const key = await getKey(secret);
  const sig = await crypto.subtle.sign("HMAC", key, new TextEncoder().encode(`${header}.${body}`));
  return `${header}.${body}.${base64url(sig)}`;
}

export async function verifyJWT(token: string, secret: string): Promise<TokenPayload | null> {
  try {
    const [header, body, sig] = token.split(".");
    const key = await getKey(secret);
    const valid = await crypto.subtle.verify(
      "HMAC", key,
      Uint8Array.from(atob(sig.replace(/-/g, "+").replace(/_/g, "/")), c => c.charCodeAt(0)),
      new TextEncoder().encode(`${header}.${body}`)
    );
    if (!valid) return null;
    const payload: TokenPayload = JSON.parse(atob(body.replace(/-/g, "+").replace(/_/g, "/")));
    if (payload.exp < Math.floor(Date.now() / 1000)) return null;  // expired
    return payload;
  } catch {
    return null;
  }
}
```

### 5.3 `src/oauth.ts` — Token Endpoint

```typescript
import { Env, TokenPayload } from "./types";
import { signJWT } from "./jwt";

export async function handleAuthorize(request: Request, env: Env): Promise<Response> {
  const url = new URL(request.url);
  const redirectUri = url.searchParams.get("redirect_uri") ?? "";
  const state = url.searchParams.get("state") ?? "";
  const redirect = new URL(redirectUri);
  redirect.searchParams.set("code", "client_credentials_flow");
  redirect.searchParams.set("state", state);
  return Response.redirect(redirect.toString(), 302);
}

export async function handleToken(request: Request, env: Env): Promise<Response> {
  let body: Record<string, string> = {};
  const contentType = request.headers.get("content-type") ?? "";
  if (contentType.includes("application/x-www-form-urlencoded")) {
    const text = await request.text();
    body = Object.fromEntries(new URLSearchParams(text));
  } else {
    body = await request.json();
  }

  const { grant_type, client_id, client_secret } = body;

  if (grant_type !== "client_credentials" && grant_type !== "authorization_code") {
    return errorResponse("unsupported_grant_type", 400);
  }
  if (client_id !== env.OAUTH_CLIENT_ID || client_secret !== env.OAUTH_CLIENT_SECRET) {
    return errorResponse("invalid_client", 401);
  }

  const ttl = parseInt(env.TOKEN_TTL_SECONDS, 10);
  const now = Math.floor(Date.now() / 1000);
  const jti = crypto.randomUUID();
  const payload: TokenPayload = { sub: client_id, iat: now, exp: now + ttl, jti };
  const accessToken = await signJWT(payload, env.JWT_SECRET);
  await env.TOKEN_STORE.put(`token:${jti}`, "valid", { expirationTtl: ttl });

  return new Response(JSON.stringify({
    access_token: accessToken,
    token_type: "Bearer",
    expires_in: ttl,
  }), { headers: { "Content-Type": "application/json" } });
}

function errorResponse(error: string, status: number): Response {
  return new Response(JSON.stringify({ error }), {
    status,
    headers: { "Content-Type": "application/json" },
  });
}
```

### 5.4 `src/proxy.ts` — MCP Proxy with API Key Injection

```typescript
import { Env } from "./types";
import { verifyJWT } from "./jwt";

export async function handleMCPProxy(request: Request, env: Env): Promise<Response> {
  const authHeader = request.headers.get("Authorization") ?? "";
  const token = authHeader.startsWith("Bearer ") ? authHeader.slice(7) : null;
  if (!token) return new Response(JSON.stringify({ error: "missing_token" }), { status: 401 });

  const payload = await verifyJWT(token, env.JWT_SECRET);
  if (!payload) return new Response(JSON.stringify({ error: "invalid_token" }), { status: 401 });

  const kvEntry = await env.TOKEN_STORE.get(`token:${payload.jti}`);
  if (!kvEntry) return new Response(JSON.stringify({ error: "token_revoked" }), { status: 401 });

  const targetUrl = new URL(request.url);
  const upstream = new URL(env.TARGET_MCP_URL);
  targetUrl.hostname = upstream.hostname;
  targetUrl.port = upstream.port;
  targetUrl.protocol = upstream.protocol;

  const needsApiKey = env.TARGET_MCP_URL.includes("alphavantage");
  const upstreamRequest = new Request(targetUrl.toString(), {
    method: request.method,
    headers: {
      ...Object.fromEntries(request.headers),
      "X-API-Key": needsApiKey ? env.ALPHAVANTAGE_API_KEY : "",
      "Authorization": needsApiKey ? `ApiKey ${env.ALPHAVANTAGE_API_KEY}` : "",
    },
    body: request.method !== "GET" ? request.body : undefined,
  });

  const response = await fetch(upstreamRequest);
  return new Response(response.body, { status: response.status, headers: response.headers });
}
```

### 5.5 `src/index.ts` — Router

```typescript
import { Env } from "./types";
import { handleAuthorize, handleToken } from "./oauth";
import { handleMCPProxy } from "./proxy";

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    if (request.method === "OPTIONS") {
      return new Response(null, {
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Methods": "GET, POST, OPTIONS",
          "Access-Control-Allow-Headers": "Authorization, Content-Type",
        },
      });
    }

    if (url.pathname === "/oauth/authorize") return handleAuthorize(request, env);
    if (url.pathname === "/oauth/token" && request.method === "POST") return handleToken(request, env);
    if (url.pathname.startsWith("/mcp")) return handleMCPProxy(request, env);
    if (url.pathname === "/health") {
      return new Response(JSON.stringify({ status: "ok" }), {
        headers: { "Content-Type": "application/json" },
      });
    }

    return new Response("Not Found", { status: 404 });
  },
};
```

---

## 6. Hosting the Target MCP Servers

### Option A — Google Cloud Run (Recommended)

```bash
git clone https://github.com/alphavantage/alpha_vantage_mcp
cd alpha_vantage_mcp
gcloud builds submit --tag gcr.io/YOUR_PROJECT/alphavantage-mcp
gcloud run deploy alphavantage-mcp \
  --image gcr.io/YOUR_PROJECT/alphavantage-mcp \
  --platform managed \
  --region asia-east1 \
  --set-env-vars ALPHAVANTAGE_API_KEY=your_key_here \
  --allow-unauthenticated
```

### Option B — MCP HTTP Transport (Local Testing)

```bash
pip install alpha-vantage-mcp
ALPHAVANTAGE_API_KEY=your_key alphavantage-mcp --transport http --port 8080
ngrok http 8080
```

---

## 7. Deployment

```bash
wrangler dev          # Local test
wrangler deploy       # Deploy to production
curl https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/health
# Expected: {"status":"ok"}
```

---

## 8. Testing the OAuth Flow

```bash
# Step 1 — Get token
curl -X POST https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/oauth/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=ge-mcp-client&client_secret=YOUR_SECRET"

# Step 2 — Call MCP tool
curl https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/mcp \
  -H "Authorization: Bearer eyJ..." \
  -H "Content-Type: application/json" \
  -d '{"tool": "get_stock_quote", "parameters": {"symbol": "AAPL"}}'
```

---

## 9. GE Data Store Configuration

| Field | Value |
|---|---|
| **MCP Server URL** | `https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/mcp` |
| **Authorization URL** | `https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/oauth/authorize` |
| **Authorization URL Parameters** | *(leave blank)* |
| **Token URL** | `https://ge-mcp-oauth-proxy.YOUR_SUBDOMAIN.workers.dev/oauth/token` |
| **Client ID** | Value you set in `OAUTH_CLIENT_ID` secret |
| **Client Secret** | Value you set in `OAUTH_CLIENT_SECRET` secret |

---

## 10. Adding SEC EDGAR MCP

```toml
[env.sec-edgar]
name = "ge-mcp-sec-edgar-proxy"

[env.sec-edgar.vars]
TARGET_MCP_URL = "https://your-sec-edgar-mcp.run.app"
TOKEN_TTL_SECONDS = "3600"
```

```bash
wrangler deploy --env sec-edgar
```

---

## 11. Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| `invalid_client` | Wrong credentials | Re-check `wrangler secret put` values |
| `invalid_token` | JWT expired/tampered | Get a new token; check `JWT_SECRET` |
| `token_revoked` | KV TTL expired | Align `TOKEN_TTL_SECONDS` with KV TTL |
| MCP proxy 502 | `TARGET_MCP_URL` unreachable | Verify Cloud Run URL |
| GE "form fields incorrect" | Auth URL issue | Ensure `/oauth/authorize` returns redirect |

---

## 12. Handover Checklist for Tommy

- [ ] Wrangler installed and `wrangler login` done
- [ ] KV namespaces created and IDs in `wrangler.toml`
- [ ] All 4 secrets set via `wrangler secret put`
- [ ] Alpha Vantage MCP deployed to Cloud Run
- [ ] `TARGET_MCP_URL` updated in `wrangler.toml`
- [ ] Worker deployed: `wrangler deploy`
- [ ] Token endpoint tested with `curl`
- [ ] MCP proxy call tested with Bearer token
- [ ] GE Data Store form filled and saved
- [ ] (Optional) SEC EDGAR second deployment done

---

## References

- [Cloudflare Workers KV Docs](https://developers.cloudflare.com/kv/)
- [Cloudflare Workers Secrets](https://developers.cloudflare.com/workers/configuration/secrets/)
- [Alpha Vantage MCP Repo](https://github.com/alphavantage/alpha_vantage_mcp)
- [SEC EDGAR MCP Repo](https://github.com/stefanoamorelli/sec-edgar-mcp)
- [Existing OAuth Worker Reference](https://mcp-github-oauth.roy-cheng.workers.dev)
