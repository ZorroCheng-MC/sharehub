---
title: "OpenClaw × Cloudflare AI Gateway — Pilot Project Plan"
tags: [project, AI, tools, automation, business, development, technical, actionable]
date: 2026-03-12
type: project
status: inbox
priority: high
---

# OpenClaw × Cloudflare AI Gateway — Pilot Project Plan

![Hero Image](/images/openclaw-cloudflare-lobster-farm.png)

## 🎯 Project Overview

**Objective**: Deploy OpenClaw as a **24/7 production service** on Cloudflare Moltworker with intelligent AI Gateway routing. Users access via Telegram with zero downtime. The goal is to demonstrate serverless AI agent architecture at scale, validate cost efficiency through smart model selection, and identify key selling points for Cloudflare professional services.

**Key Innovation**: AI Gateway's **intelligent routing + fallback** handles model selection based on task complexity, latency targets, and cost in real-time. No manual intervention needed.

**Sponsor**: Cloudflare (USD $5,000 token budget)
**Project Lead**: Zorro (Master Concept)
**Duration**: ~3 weeks (5-day setup + 2-week live operation + monitoring)

---

## 🛠️ Technology Stack (Moltworker)

**Cloudflare Products:**
- **Workers** — Entrypoint API router + proxy; always-on (no cold starts)
- **Sandbox SDK** — Isolated container per user (OpenClaw Gateway runtime)
- **Sandbox Containers** — Full Docker support for OpenClaw; auto-scale per demand
- **AI Gateway** — Intelligent LLM routing, model fallbacks, cost optimization
- **Durable Objects** — Background job queue for long-running tasks (>30s)
- **Browser Rendering** — Puppeteer/Playwright for web automation
- **R2** — Persistent storage for credentials, session files, logs
- **Secrets Store** — Encrypted storage for OAuth tokens, API keys, LLM credentials
- **Zero Trust Access** — JWT authentication for admin UI
- **Workers Analytics** — Real-time cost & performance metrics

**OpenClaw Integration:**
- GitHub: `cloudflare/moltworker` (official Cloudflare implementation)
- OpenClaw Gateway runs inside Sandbox Container
- Telegram routing via Moltworker API layer
- GWS credentials stored securely in R2 + Secrets (encrypted at rest)

**Standard Packages per Instance:**
- **Vercel Agent Browser** — Web automation, form filling, screenshot capture
  - GitHub: `vercel/agent-browser`
  - Integration: MCP-compatible tool for OpenClaw
  - Use cases: Web scraping, automated testing, form submission
  - Configuration: Via secrets.json (if auth required)

**Monitoring & Analytics:**
- Cloudflare AI Gateway dashboard (request logs, token usage, costs)
- Moltworker custom metrics (Sandbox CPU/memory, container uptime)
- OpenClaw internal metrics (session count, tool usage, errors)

---

## 👥 Stakeholders, Roles & Responsibilities

### 🟠 MC-Cloudflare Team

| Name | Role | Responsibilities |
|------|------|-----------------|
| Kelvin | Sponsor / Gateway Admin | Set up Cloudflare AI Gateway policies; configure dynamic LLM routing; manage billing and token quotas; define AI Firewall rules |
| Kitty | Sponsor / Gateway Admin | Co-manage AI Gateway and Firewall configuration; monitor policy enforcement; provide usage analytics and reports |

**Privileges**: Full control over Cloudflare AI Gateway dashboard, billing, quota allocation, and firewall rule configuration. No access to OpenClaw containers or user data.

---

### 🔵 OpenClaw Team (Master Concept)

| Name | Role | Responsibilities |
|------|------|-----------------|
| Zorro | Project Lead | Overall project coordination; consumer invitation and onboarding; stakeholder communication; final findings report; go/no-go decisions |
| Tommy | Engineer | Moltworker deployment on Cloudflare (via GitHub repo); Sandbox Container provisioning per user; GWS service integration (Gmail, Calendar, Drive) via R2/Secrets Store; credential lifecycle management |
| Alfred | Engineer | Telegram bot routing setup (via Moltworker API); OpenClaw Gateway configuration within sandbox; custom skills adaptation; monitoring and debugging within Sandbox environment |

**Privileges**: Full access to Moltworker codebase, Cloudflare Workers dashboard, Sandbox SDK; R2 bucket management for user data; Secrets Store configuration; review aggregate usage logs; no direct access to Cloudflare billing (managed by MC-Cloudflare team).

---

### 🟢 Consumer Team

| Name | Status | Access |
|------|--------|--------|
| Suee | Active (invited) | Telegram → OpenClaw container |
| Casey | Active (invited) | Telegram → OpenClaw container |
| Future users | Invited by Zorro | Same as above |

**Privileges**: Use OpenClaw via Telegram only. No direct access to containers, LLM provider credentials, or Cloudflare dashboard.

**Agreed Terms (all consumers must acknowledge before onboarding)**:
1. All traffic between OpenClaw and LLM providers is monitored by Cloudflare AI Gateway
2. The AI Firewall may block or redact sensitive information (e.g. API keys, credentials) before it reaches the LLM
3. This is a sponsored POC — token consumption is free within the allocated budget

---

## 🏗️ Architecture Overview

```
Consumer (Telegram)
        │
        ▼
Cloudflare Moltworker (Serverless Edge)
  - Runs on Cloudflare Workers + Sandbox Containers
  - One worker instance per user (isolated)
  - GWS tools enabled (Gmail, Calendar, Drive via R2 credential storage)
  - Browser Rendering for web automation
  - Credential management via Cloudflare Secrets
        │
        ▼
Cloudflare AI Gateway
  - Dynamic LLM routing
  - Candidate providers: Claude Sonnet, MiniMax, Gemini Flash, GLM
  - AI Firewall (sensitive data protection)
  - Full traffic logging & analytics
  - Unified billing (consume sponsored credits)
        │
        ▼
LLM Providers (Cloud)
```

**Key Architectural Changes:**
- **No local hardware** — Moltworker runs entirely on Cloudflare's global network (24/7 uptime, no maintenance)
- **Sandboxed isolation** — Each user gets isolated Sandbox Container via Sandbox SDK
- **Persistent storage** — R2 bucket for session memory, configs, GWS credentials (encrypted)
- **Credential security** — Secrets Store for LLM keys, OAuth tokens, API credentials
- **Browser Rendering** — Puppeteer/Playwright via Cloudflare (no local Chromium needed)
- **Background jobs** — Durable Objects handle tasks >30s (e.g., long-running research, document processing)

---

## 🧠 AI Gateway Intelligent Model Routing Strategy

### Model Tier System

**Tier 1: Fast & Cheap (Entry tasks)**
- **Models**: GLM-5, MiniMax M2.5, Claude Haiku
- **Use case**: Simple Q&A, email drafts, quick lookups, transcription
- **Cost**: ~50% less than Sonnet
- **Latency**: <500ms (usually <200ms)
- **Accuracy**: 85-90% for routine tasks

**Tier 2: Balanced (Standard tasks)**
- **Models**: Claude Sonnet 4.5, Gemini Flash, GLM-5-Plus
- **Use case**: Content creation, analysis, tool usage, complex reasoning
- **Cost**: Baseline (reference point)
- **Latency**: 500-1500ms
- **Accuracy**: 95%+ for most tasks

**Tier 3: Powerful (Complex tasks)**
- **Models**: Claude Opus (when available via API), Gemini 2.0
- **Use case**: Deep analysis, novel problem-solving, edge cases
- **Cost**: 2-3x Sonnet
- **Latency**: 1-3s
- **Accuracy**: 99% for specialized reasoning

### AI Gateway Configuration (Intelligent Routing)

**AI Gateway will be configured with:**

```
Primary model: Claude Sonnet 4.5
Fallback chain: [MiniMax M2.5 → GLM-5 → Gemini Flash]
Cost threshold: Route to Tier 1 if task is classified as "simple"
Latency constraint: If <500ms needed → use Tier 1 only
```

**How It Works:**
1. Request arrives from Telegram user (via Moltworker)
2. AI Gateway inspects request metadata:
   - Task type (from OpenClaw classification)
   - Token estimate (word count heuristic)
   - Time constraint (implicit from Telegram interaction)
   - User quota remaining
3. **Smart routing decision**:
   - **Simple task** (Q&A, lookup) → Route to GLM-5 (cheapest)
   - **Standard task** (normal chat) → Route to Sonnet 4.5 (balanced)
   - **Complex task** (reasoning, analysis) → Route to Opus (if quota allows)
   - **Primary unavailable** → Automatic fallback to next tier
   - **Budget at risk** (>80% consumed) → Force downgrade to cheaper model
4. Request forwarded to selected provider
5. Response returned to Moltworker → Telegram user

### Cost Optimization Features (Built into AI Gateway)

| Feature | Benefit | How It Helps |
|---------|---------|-------------|
| **Request caching** | Deduplicate identical prompts | Same question twice = 1st hit cached |
| **Token-level compression** | Reduce token count before sending | Markdown formatting → saves ~5-10% |
| **Batch processing** | Combine multiple small requests | Mail summary → 1 request vs. N |
| **Fallback cost handling** | If Tier 2 fails, retry Tier 1 (cheaper) | Fault tolerance + cost savings |
| **Quota-aware routing** | Dynamically degrade quality to save budget | Never exceed $5K limit |

### 24/7 Uptime Strategy

**Cloudflare provides:**
- ✅ Global edge network (no SPOF)
- ✅ Auto-scaling Sandbox Containers (handle traffic spikes)
- ✅ Worker cold-start: **~50ms** (no "starting up" delays)
- ✅ SLA: 99.99% uptime (Cloudflare guarantees)

**Moltworker provides:**
- ✅ Always-on Workers (billing per request, not per hour)
- ✅ Persistent session data in R2 (survive container restarts)
- ✅ Durable Objects for background tasks (don't timeout)

**OpenClaw integration:**
- ✅ Stateless design (each request independent)
- ✅ Session memory persisted to R2 (resume interrupted conversations)
- ✅ No scheduled restarts needed (unlike local Docker)

**Result**: Service runs 24/7 with zero planned downtime.

### Long-Running Tasks (>30s)

The 30-second Sandbox timeout applies per HTTP request. For longer tasks:

**Solution: Durable Objects as Job Queue**

```
User request (Telegram)
        │
        ▼
Moltworker receives → Immediately returns "processing..."
        │
        ▼
Enqueues job in Durable Object
        │
        ▼
Durable Object processes in background (no timeout)
        │
        ▼
Stores result in R2
        │
        ▼
User gets update notification when done
```

**Example flow:**
1. User: "Analyze the top 100 GitHub issues for my repo"
2. Moltworker: "Got it, analyzing. I'll send results via Telegram when ready" (instant response)
3. Durable Object: Spawns long-running Sandbox, iterates issues, synthesizes report (~2-5 min)
4. User: Receives "Analysis complete: [summary]" via Telegram notification

**Cost**: Durable Objects billed separately (~$0.15/M reads + $1.15/M writes). Likely trivial for this POC.

---

## 📅 Project Timeline

### Phase 1: Setup (Week 1 — Mon to Fri, 5 days)

| Day | Task | Owner |
|-----|------|-------|
| Mon | Cloudflare AI Gateway provisioned; dynamic routing policy drafted; Unified Billing configured | Kelvin, Kitty |
| Mon | Moltworker repo cloned; Cloudflare account and Workers setup confirmed; R2 bucket + Secrets Store created | Tommy, Alfred |
| Tue | Moltworker deployed to Cloudflare Workers; initial Sandbox Container spawned and tested | Tommy, Alfred |
| Tue | GWS credential integration tested (OAuth flow for Gmail, Calendar, Drive stored in Secrets) | Tommy |
| Wed | Telegram bot routing configured via Moltworker API; test message flow through Gateway | Alfred |
| Wed | AI Firewall rules baseline configured; credential masking rules set | Kelvin, Kitty |
| Thu | End-to-end test with internal accounts (Zorro/Tommy/Alfred as test users); Browser Rendering validation | All OpenClaw team |
| Thu | Consumer Sandbox instances (Suee, Casey) provisioned via Moltworker dashboard | Tommy, Alfred |
| Fri | Consumer onboarding — send invitation, Telegram link, confirm first message received | Zorro |
| Fri | Buffer / fix day; Phase 1 sign-off meeting | All teams |

### Phase 2: Monitoring & Study (Weeks 2–3, 10 business days)

| Period | Activity | Owner |
|--------|----------|-------|
| Week 2 | Live usage by consumers; daily check-in on Gateway logs | All teams |
| Week 2 | MC-Cloudflare team shares weekly Gateway analytics snapshot | Kelvin, Kitty |
| Week 2 | OpenClaw team logs any issues, routing failures, firewall blocks | Tommy, Alfred |
| Week 3 | Identify patterns: cost saving opportunities, security events caught by firewall | Zorro + MC-Cloudflare team |
| Week 3 | Draft findings: what worked, what didn't, enterprise value propositions | Zorro |
| Week 3 (Fri) | Final debrief meeting; findings presentation | All stakeholders |

---

## 📦 Deliverables

| # | Deliverable | Owner | Due |
|---|------------|-------|-----|
| 1 | Moltworker deployed to Cloudflare Workers (from cloudflare/moltworker repo) | Tommy, Alfred | Day 2 |
| 2 | Cloudflare AI Gateway configured with dynamic LLM routing + Unified Billing | Kelvin, Kitty | Day 1 |
| 3 | AI Firewall baseline rules (credential masking, sensitive data detection) | Kelvin, Kitty | Day 3 |
| 4 | R2 bucket provisioned for user session persistence; Secrets Store configured | Tommy | Day 2 |
| 5 | Consumer Sandbox instances created and Telegram routing tested (Suee, Casey) | Tommy, Alfred | Day 5 |
| 6 | Moltworker admin dashboard setup (monitoring, user management, logs) | Alfred | Day 3 |
| 7 | Weekly Gateway usage analytics + Sandbox performance report | Kelvin, Kitty | End of each week |
| 8 | Final findings report with enterprise selling points (serverless advantage, cost, scaling) | Zorro | End of Week 3 |

---

## 💰 Budget & Quota

- **Total Sponsorship**: USD $5,000 (provided by Cloudflare)
- **Managed by**: MC-Cloudflare team (Kelvin, Kitty)
- **Token allocation per user**: TBD by MC-Cloudflare team based on number of active consumers
- **Overage policy**: Zorro to be notified if consumption approaches 80% of budget; no new users onboarded until confirmed safe

---

## ✅ Success Criteria

The POC is considered successful if we can document at least **FOUR of the following**:

### Primary Metrics (Must have 2+)

1. **Intelligent model routing reduces costs by >20%**
   - *Success*: AI Gateway's dynamic routing saves tokens vs. always using Sonnet
   - *Measurement*: Compare actual spend to "all Sonnet" baseline
   - *Target*: Reduce $5K budget by >$1K through smart tier selection

2. **24/7 uptime achieved with zero downtime**
   - *Success*: System runs continuously for 2 weeks with no planned/unplanned restarts
   - *Measurement*: Cloudflare uptime dashboard shows 100% or >99.9%
   - *Target*: Prove serverless is more reliable than local Docker

3. **AI Firewall blocks sensitive data**
   - *Success*: Firewall catches and redacts at least 1 real PII/API key attempt
   - *Measurement*: Firewall logs show blocked patterns with redaction
   - *Target*: Demonstrate security value of gateway inspection

### Secondary Metrics (Pick 2 more for full success)

4. **Sandbox scalability validated**
   - *Success*: Easily onboard 5+ new consumers without infra changes
   - *Measurement*: Add Suee, Casey, +3 more in <30 min each
   - *Target*: Prove Sandbox auto-scaling works

5. **Edge latency advantage proven**
   - *Success*: Cloudflare edge responds faster than API-direct calls
   - *Measurement*: p50/p95 latency to Cloudflare vs. direct LLM provider
   - *Target*: Show 200-500ms edge advantage for typical requests

6. **Durable Objects handles long-running tasks**
   - *Success*: At least 1 background job (>30s) completes successfully
   - *Example*: Analyze 10+ GitHub issues, email thread summarization
   - *Measurement*: Task completes, user notified, result correct

7. **Cost model beats alternatives**
   - *Success*: Total 2-week spend demonstrates value vs. alternatives
   - *Comparison*: Moltworker cost vs. Mac Studio + local Ollama vs. traditional VPS
   - *Target*: Moltworker = cheapest per-user option

8. **Enterprise customer conversation ready**
   - *Success*: Zorro can pitch specific customer segments (startups, enterprises, security-conscious)
   - *Selling points*: "24/7 serverless", "cost-optimized AI", "built-in security", "no ops overhead"
   - *Measurement*: 3-5 written talking points with evidence from this POC

---

---

## 🧑‍💼 Consumer Preparation Checklist

**Each consumer needs to prepare BEFORE tech team setup begins.**

### Consumer Self-Service Checklist (Pre-Setup)

| # | Task | Owner | Example | Status |
|---|------|-------|---------|--------|
| 1 | Create new Gmail account (independent from personal email) | Consumer | `suee-openclaw@gmail.com` | ⏳ |
| 2 | Create new GCP project (for OAuth credentials) | Consumer | `suee-openclaw-gcp` (project ID) | ⏳ |
| 3 | Share Gmail + GCP Project ID with Zorro | Consumer | Email or Telegram | ⏳ |
| 4 | Confirm Telegram username/ID for bot link | Consumer | `@suee_tz` | ⏳ |
| 5 | Review & acknowledge pilot terms (data monitoring, security) | Consumer | Sign-off email | ⏳ |

---

### Pre-Setup Email to Consumers (Send 1 week before tech setup)

**Subject: OpenClaw Pilot — Action Required (Setup Prep)**

> Hi [Name],
>
> Great news — you're approved to join the OpenClaw pilot! 🎉
>
> Before our tech team can set you up, we need you to prepare a few things (15 min, one-time):
>
> **Step 1: Create a new Gmail account** (separate from personal email)
> - Go to https://accounts.google.com/signup
> - Use format: `[your-name]-openclaw@gmail.com` (e.g., suee-openclaw@gmail.com)
> - Save the password somewhere safe (you'll need it later)
> - ✅ Confirm account creation
>
> **Step 2: Create a new Google Cloud project** (for OAuth credentials)
> - Go to https://console.cloud.google.com
> - Click "Select a Project" (top-left)
> - Click "NEW PROJECT"
> - Enter name: `[your-name]-openclaw` (e.g., suee-openclaw)
> - Click "CREATE"
> - Wait 30 seconds for project to initialize
> - Note the **Project ID** (will be shown in dashboard, usually auto-generated)
> - ✅ Project created successfully
>
> **Step 3: Share credentials with tech team**
> - Reply to this email with:
>   - **New Gmail**: suee-openclaw@gmail.com
>   - **GCP Project ID**: [from step 2]
>   - **Your Telegram username**: @suee_tz (or ID: 123456789)
> - ✅ Credentials submitted
>
> **Step 4: Review pilot terms**
> - All traffic monitored by Cloudflare AI Gateway (for research)
> - AI Firewall may block sensitive data before it reaches LLM (for security)
> - This is a free pilot funded by Cloudflare
> - Reply: "I acknowledge and accept the above terms"
> - ✅ Terms acknowledged
>
> **Timeline:**
> - You submit above: Friday
> - Tech team prepares setup: Monday
> - You get Telegram bot link: Tuesday
> - Live access: Wednesday
>
> Questions? Reply to this email.
>
> Best,  
> Zorro

---

### Tech Team Setup Checklist (Tommy & Alfred)

**Once consumer submits their prep info, follow this checklist per consumer.**

#### Consumer Onboarding Template

```yaml
Consumer: [NAME]
Status: Ready for setup
Date: 2026-03-[XX]

CONSUMER_PROVIDED:
  gmail: [EMAIL]           # e.g., suee-openclaw@gmail.com
  gcp_project_id: [ID]    # e.g., suee-openclaw-gcp
  telegram_username: [TG] # e.g., @suee_tz
  telegram_id: [ID]       # e.g., 123456789
  terms_acknowledged: Yes

TECH_TEAM_TASKS:
  ☐ Task 1: Enable GWS APIs in GCP project
  ☐ Task 2: Create OAuth 2.0 credentials
  ☐ Task 3: Complete OAuth flow (out-of-band)
  ☐ Task 4: Upload credentials to Cloudflare Secrets Store
  ☐ Task 5: Create Telegram bot (BotFather)
  ☐ Task 6: Configure Telegram webhook
  ☐ Task 7: Create Sandbox instance in Moltworker
  ☐ Task 8: Verify end-to-end test
  ☐ Task 9: Send Telegram bot link to consumer
  ☐ Task 10: Consumer confirms access

CREDENTIALS_CREATED:
  telegram_bot_token: [TOKEN]  # Secrets Store
  oauth_refresh_tokens: [STORED] # Secrets Store
  sandbox_instance: [CREATED] # Moltworker
  r2_session_folder: /[consumer]/session.json # R2
```

---

### Tech Team Step-by-Step (Per Consumer)

**Step 1: Enable GWS APIs in GCP project** (Tommy)

```bash
# Consumer provides:
# - GCP Project ID: suee-openclaw-gcp
# - Gmail: suee-openclaw@gmail.com

# Via GCP Console:
# 1. Go to https://console.cloud.google.com
# 2. Select project: suee-openclaw-gcp
# 3. Click "APIs & Services" → "Library"
# 4. Search and enable EACH of these:
#    - Google Drive API
#    - Google Calendar API
#    - Gmail API
#    - Google Workspace Admin API (optional, for advanced features)
# 5. Screenshot enabled APIs and save for records
```

**Step 2: Create OAuth 2.0 Credentials** (Tommy)

```bash
# Still in GCP Console (same project):
# 1. Go to "APIs & Services" → "Credentials"
# 2. Click "Create Credentials" → "OAuth 2.0 Client IDs"
# 3. If prompted: "Create OAuth consent screen first"
#    - User type: External
#    - Fill in app name: "[Consumer] OpenClaw Bot"
#    - Add scopes: drive.file, calendar, gmail.modify
#    - Save
# 4. Back to Credentials → "Create Credentials" → "OAuth 2.0 Client IDs"
# 5. Application type: Desktop app
# 6. Click "Create"
# 7. Download JSON file (client_secret_*.json)
# 8. Save securely: ~/Downloads/client_secret_[consumer].json
```

**Step 3: Complete OAuth Flow (Out-of-Band)** (Tommy)

```bash
# Run this ONCE per consumer (interactive, must use local browser)

python3 << 'EOF'
import json, os
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow

# Load client secret
client_secret_file = os.path.expanduser("~/Downloads/client_secret_suee.json")

# Define scopes
scopes = [
    'https://www.googleapis.com/auth/drive.file',
    'https://www.googleapis.com/auth/calendar',
    'https://www.googleapis.com/auth/gmail.modify'
]

# Run OAuth flow (opens browser for user to authorize)
flow = InstalledAppFlow.from_client_secrets_file(client_secret_file, scopes=scopes)
credentials = flow.run_local_server(port=18088)

# Save credentials to file
creds_cache = os.path.expanduser("~/.config/gcloud/suee_openclaw_creds.json")
with open(creds_cache, 'w') as f:
    f.write(credentials.to_json())

print(f"✅ Credentials saved: {creds_cache}")
print(f"Refresh token: {credentials.refresh_token}")
EOF

# Expected output:
# ✅ Credentials saved: ~/.config/gcloud/suee_openclaw_creds.json
# Refresh token: 1//0gxxxx...
```

**Step 4A: Create Consumer secrets.json File** (Tommy) — **RECOMMENDED APPROACH**

OpenClaw now uses `secrets.json` for centralized credential management (via SecretRefs).

```bash
# Create consumer-specific secrets.json file
# This will be stored in R2 and injected into each Sandbox Container

cat > /tmp/suee_secrets.json << 'EOF'
{
  "providers": {
    "gws": {
      "gmail": {
        "refreshToken": "1//0gxxxx..."  // from OAuth flow above
      },
      "calendar": {
        "refreshToken": "1//0gxxxx..."  // same token
      },
      "drive": {
        "refreshToken": "1//0gxxxx..."  // same token
      }
    },
    "anthropic": {
      "apiKey": "sk-ant-..."  // AI Gateway token or backup Anthropic key
    },
    "cloudflare": {
      "aiGatewayToken": "your_cf_token_if_gateway_auth_enabled"
    }
  }
}
EOF

# Upload to R2 bucket (one per consumer)
wrangler r2 object put openclaw-users-data/suee/secrets.json /tmp/suee_secrets.json

# Verify upload
wrangler r2 object list openclaw-users-data --prefix=suee/
# Should see: suee/secrets.json
```

**Why secrets.json + R2?**
- ✅ Centralized per-consumer credentials (not scattered across Cloudflare Secrets)
- ✅ OpenClaw's native SecretRefs can read from file source
- ✅ R2 persists across Sandbox Container restarts
- ✅ Encrypted at rest by Cloudflare
- ✅ One file = all credentials for that consumer
- ✅ Easier to rotate (update one file)

**Step 4B: Configure OpenClaw SecretRefs in Moltworker** (Alfred)

In the Sandbox Container startup, configure OpenClaw to use the `secrets.json` via SecretRefs:

```bash
# Moltworker startup script (runs inside Sandbox)
# This mounts R2 secrets.json and tells OpenClaw where to find credentials

SANDBOX_CONSUMER="suee"

# Download secrets.json from R2 to ephemeral disk
mkdir -p /workspace/secrets
wrangler r2 object get openclaw-users-data/${SANDBOX_CONSUMER}/secrets.json /workspace/secrets/secrets.json

# Start OpenClaw Gateway with SecretRefs pointing to secrets.json
openclaw gateway \
  --config-override '{
    "secrets": {
      "providers": {
        "consumer_file": {
          "source": "file",
          "path": "/workspace/secrets/secrets.json",
          "mode": "json"
        }
      }
    },
    "channels": {
      "telegram": {
        "token": { "source": "file", "provider": "consumer_file", "id": "/telegram/token" }
      }
    },
    "models": {
      "providers": {
        "anthropic": {
          "apiKey": { "source": "file", "provider": "consumer_file", "id": "/providers/anthropic/apiKey" }
        }
      }
    },
    "tools": {
      "gws": {
        "gmail": {
          "refreshToken": { "source": "file", "provider": "consumer_file", "id": "/providers/gws/gmail/refreshToken" }
        },
        "calendar": {
          "refreshToken": { "source": "file", "provider": "consumer_file", "id": "/providers/gws/calendar/refreshToken" }
        },
        "drive": {
          "refreshToken": { "source": "file", "provider": "consumer_file", "id": "/providers/gws/drive/refreshToken" }
        }
      }
    }
  }'
```

**Alternative: Cloudflare Secrets → Environment Variables → SecretRefs**

If you prefer Cloudflare Secrets Store instead of R2:

```bash
# Upload to Cloudflare Secrets
wrangler secret put GWS_GMAIL_REFRESH_TOKEN_SUEE
# Paste: 1//0gxxxx...

# Moltworker injects as env var → OpenClaw reads via env SecretRef
# Then in OpenClaw config:
{
  "tools": {
    "gws": {
      "gmail": {
        "refreshToken": { "source": "env", "provider": "default", "id": "GWS_GMAIL_REFRESH_TOKEN_SUEE" }
      }
    }
  }
}
```

**Recommendation:** Use `secrets.json` + R2 (Step 4A) because:
- Fewer Cloudflare Secrets entries (one per consumer instead of 3-5)
- Matches OpenClaw's native secrets model
- Easier to backup and rotate
- Self-contained per consumer

**Step 5: Create Telegram Bot** (Alfred)

```bash
# Talk to @BotFather on Telegram
# /newbot
# Bot name: OpenClaw Suee Bot
# Bot username: suee_openclaw_bot (MUST have "bot" suffix, be unique)
# Receive: BOT_TOKEN

# Save token
wrangler secret put TELEGRAM_BOT_TOKEN_SUEE
# Paste: 5123456789:ABCDEFGHIJKLMNOPQRSTUVWxyz...
```

**Step 6: Configure Telegram Webhook** (Alfred)

```bash
BOT_TOKEN="5123456789:ABCDEFGHIJKLMNOPQRSTUVWxyz..."
WEBHOOK_URL="https://moltworker.ACCOUNT.workers.dev/telegram/suee/hook"

curl -X POST https://api.telegram.org/bot${BOT_TOKEN}/setWebhook \
  -H "Content-Type: application/json" \
  -d "{
    \"url\": \"${WEBHOOK_URL}\",
    \"secret_token\": \"suee_secret_$(date +%s)\"
  }"

# Verify:
curl https://api.telegram.org/bot${BOT_TOKEN}/getWebhookInfo | jq .
# Should show: "url": "https://moltworker..."
```

**Step 7: Create Sandbox Instance** (Tommy)

```bash
# Via Moltworker admin or Cloudflare dashboard:
# 1. Create new Sandbox: suee-consumer
# 2. Mount R2 bucket
# 3. Inject Secrets: GWS_*_TOKEN_SUEE, TELEGRAM_BOT_TOKEN_SUEE
# 4. Set environment: CONSUMER_NAME=suee

# Verify:
# Moltworker dashboard should show "suee-consumer" instance (running)
```

**Step 7B: Configure Vercel Agent Browser in OpenClaw** (Alfred)

```bash
# Inside Sandbox Container startup, add to OpenClaw config:

cat >> /workspace/openclaw-config-overrides.json << 'EOF'
{
  "tools": {
    "browser": {
      "enabled": true,
      "provider": "vercel-agent-browser",
      "config": {
        "headless": true,
        "timeout": 30000,
        "viewport": {
          "width": 1920,
          "height": 1080
        }
      }
    }
  }
}
EOF

# Merge with existing OpenClaw config:
openclaw config apply --overlay /workspace/openclaw-config-overrides.json
```

**Step 8: End-to-End Test** (Alfred)

```bash
# Send test messages to Telegram bot link:
# https://t.me/suee_openclaw_bot

# Test 1 - Basic message:
# "hello"
# Expected: Bot responds

# Test 2 - Browser command:
# "take a screenshot of https://example.com and describe it"
# Expected: Bot uses Vercel Agent Browser → captures screenshot → describes

# Test 3 - GWS tool:
# "what emails do I have from alice@example.com?"
# Expected: Bot queries Gmail via GWS tools → returns results

# Check logs:
wrangler tail
# Should see: 
# [suee-consumer] message received
# [suee-consumer] browser automation: screenshot captured
# [suee-consumer] gws: gmail query executed
# [suee-consumer] response sent
```

**Step 9: Send Bot Link to Consumer** (Zorro)

```bash
# Template message (send via Zorro):

Hi Suee,

Your OpenClaw bot is ready! 🎉

Add this bot to Telegram:
https://t.me/suee_openclaw_bot

Then say anything — ask questions, get help, create content, whatever!

This pilot runs 24/7. All done.

—Zorro
```

**Step 10: Consumer Confirms Access** (Consumer)

- Consumer clicks link
- Sends test message ("hello" or similar)
- Confirms bot responds
- Mark complete in checklist

---

### Verification Checklist (Before going live)

| Item | Check | Evidence |
|------|-------|----------|
| Gmail account created | ✅ | Consumer confirms login works |
| GCP project created | ✅ | Project ID provided, APIs enabled |
| OAuth credentials obtained | ✅ | Refresh token saved in Secrets |
| Telegram bot created | ✅ | Bot token in Secrets, webhook configured |
| Sandbox instance live | ✅ | Dashboard shows `[consumer]-consumer` running |
| End-to-end test passed | ✅ | Test message → response verified |
| Consumer can access | ✅ | Consumer adds bot, sends message, gets reply |

---

## 📬 Consumer Invitation Email Template

> Use this for all future consumer invitations. Customise `[Name]` and send from Zorro.

---

**Subject: You're Invited — OpenClaw AI Pilot (Free Access)**

Hi [Name],

I'd like to invite you to join a private AI pilot we're running at Master Concept.

**What is this?**
We're giving a small group of selected users free access to **OpenClaw** — an AI assistant powered by multiple LLM providers (including Claude, Gemini, and others). You'll use it directly through **Telegram**, so there's nothing to install.

**What's in it for you?**
- Free AI usage during the pilot period (no cost to you)
- Access to a powerful assistant that can help with emails, calendar, documents, research, and more
- You're among the first to experience this setup before we roll it out more widely

**What you need to know (important)**
This pilot is running on infrastructure sponsored by **Cloudflare**. As part of the setup:
1. All traffic between your OpenClaw session and the AI provider is **monitored by Cloudflare AI Gateway** — this is for research and security purposes
2. An **AI Firewall** is active and may automatically block sensitive information (such as API keys or credentials) from being sent to the AI — this is to protect you
3. Your usage helps us understand how to build better, safer AI services for enterprise customers in the future

By joining, you acknowledge and agree to the above terms. There is no obligation to continue if you're not comfortable.

**How to get started**
Once you confirm you'd like to join, I'll send you a Telegram link to your personal OpenClaw instance. Setup takes less than a minute.

Interested? Just reply to this message with **"I'm in"** and I'll get you onboarded.

Looking forward to having you on board.

Best,
Zorro
Master Concept

---

## 🔗 References & Related Notes

- OpenClaw documentation: https://github.com/All-Hands-AI/OpenHands
- Cloudflare AI Gateway: https://developers.cloudflare.com/ai-gateway/
- Cloudflare AI Firewall (Firewall for AI): https://developers.cloudflare.com/ai-gateway/ai-firewall/
- gwscli setup guide: [[2026-03-08-better-stack-gwscli-claude-code]]

---

## 🔧 Moltworker Implementation Notes

### Prerequisites

**Cloudflare Account Requirements:**
- ✅ Cloudflare account with **Workers Paid plan** (required for Sandbox Containers)
- ✅ Sandbox SDK enabled in account
- ✅ AI Gateway feature enabled
- ✅ R2 bucket provisioned (free tier: 10GB included)
- ✅ Secrets Store configured
- ✅ Zero Trust Access available (for admin UI protection)

**Repository & Deployment:**
- Source: `cloudflare/moltworker` (GitHub — open source, proof-of-concept)
- Deployment: `wrangler deploy` (Cloudflare Workers CLI)
- Configuration: `wrangler.toml` + `.env` secrets
- Status: **Not an official Cloudflare product** — community-maintained with Cloudflare support

**GWS Credential Flow:**
1. User completes OAuth for Gmail/Calendar/Drive (out-of-band, first-time setup)
2. OAuth tokens stored in Secrets Store (encrypted, no personal device storage)
3. Tokens injected into Sandbox Container at runtime
4. Session memory stored in R2 (encrypted)
5. Credentials never exposed to UI or logs

### Sandbox Container Constraints

- **Max CPU**: 1 vCPU per container
- **Max Memory**: 128MB per container (sufficient for OpenClaw Gateway)
- **Max Runtime**: 30s per request (enforced)
- **Disk**: Ephemeral (mounted from R2 on startup, sync on shutdown)
- **Networking**: Outbound only (to AI Gateway, GWS, external APIs)

### Known Limitations & Mitigations

| Limitation | Impact | Mitigation |
|-----------|--------|-----------|
| 30s request timeout | Long-running tasks fail | Use Durable Objects for background jobs; implement chunked processing |
| 128MB memory | Memory-intensive models (e.g., local LLMs) won't fit | Rely on cloud LLMs via AI Gateway; disable Ollama |
| Ephemeral disk | Container state lost after request | Persist critical data to R2; design for stateless operations |
| Sandbox per-user isolation | Multiple simultaneous users consume quota | Monitor concurrent Sandbox count; set rate limits per user |

### Moltworker vs. Local Docker (Comparison)

| Aspect | Local Docker (Mac Studio) | Moltworker (Cloudflare) |
|--------|--------------------------|------------------------|
| **Hardware** | Dedicated Mac Studio | None (serverless) |
| **Cost** | Mac hardware + electricity | Workers Paid plan + Sandbox usage |
| **Scaling** | Max N users (hardware limit) | Unlimited auto-scaling |
| **Maintenance** | Manual (OS updates, Docker, Ollama) | Zero (Cloudflare managed) |
| **Latency** | Geographic (users → Mac Studio) | Low (users → closest Cloudflare edge) |
| **Security** | Shared host OS | Isolated Sandbox per user |
| **Dev speed** | Slow iteration (Docker rebuild) | Fast (instant deploy via Workers) |
| **Credentials** | Stored on Mac (encrypted?) | Secrets Store (managed encryption) |

### Moltworker Deployment Checklist

**Cloudflare Infrastructure:**
- [ ] Cloudflare account + Workers Paid plan active
- [ ] Sandbox SDK enabled + quota verified
- [ ] R2 bucket created (`openclaw-users-data`)
- [ ] Durable Objects namespace created (`job-queue`)
- [ ] Zero Trust Access policy created (admin dashboard protection)
- [ ] Workers Analytics enabled (cost tracking)

**AI Gateway Configuration:**
- [ ] AI Gateway instance created
- [ ] Unified Billing activated (consume $5K budget)
- [ ] Model routing rules configured:
  - [ ] Primary: Claude Sonnet 4.5
  - [ ] Fallback tier 1: MiniMax M2.5, GLM-5
  - [ ] Fallback tier 2: Gemini Flash
- [ ] Request caching enabled
- [ ] Token-level compression enabled
- [ ] Budget alerts set (80% threshold = Zorro + Kelvin notified)

**Secrets & Credentials:**
- [ ] Secrets Store configured with:
  - `ANTHROPIC_API_KEY` (for direct API, backup)
  - `CLOUDFLARE_AI_GATEWAY_TOKEN` (primary routing)
  - `CLOUDFLARE_AI_GATEWAY_ACCOUNT_ID` + `GATEWAY_ID`
  - GWS OAuth credentials (Gmail, Calendar, Drive)
  - Telegram bot token
  - OpenClaw Gateway admin token

**Moltworker Deployment:**
- [ ] Fork/clone `cloudflare/moltworker` repo
- [ ] Install standard packages in `package.json`:
  - [ ] `@vercel/agent-browser` (web automation)
  - [ ] Any additional MCP tools as needed
- [ ] `wrangler.toml` customized:
  - [ ] Account ID
  - [ ] R2 bucket binding
  - [ ] Durable Object binding (`job-queue`)
  - [ ] Secrets bindings
  - [ ] Route (e.g., `openclaw.zorro.workers.dev`)
- [ ] Environment variables set (`.env` local + Cloudflare Secrets)
- [ ] Local testing via `wrangler dev` (test browser automation)
- [ ] Deployed via `wrangler deploy`
- [ ] GitHub Actions workflow set up (auto-deploy on push to main)

**Monitoring & Alerting:**
- [ ] Workers Analytics dashboard configured
- [ ] Cost tracking dashboard in Cloudflare (daily snapshots)
- [ ] Telegram alert channel for errors + quota warnings
- [ ] Uptime monitoring (Cloudflare Status + custom health check)
- [ ] Session metrics exported (active users, requests/hour, latency p95)

---

---

## 📖 Deployment Runbook (Step-by-Step)

### Phase 1.1: Cloudflare Infrastructure Setup (Day 1 — Kelvin + Tommy)

**Step 1: Enable required products**

```bash
# Login to Cloudflare dashboard
# https://dash.cloudflare.com

# 1. Upgrade Workers plan to Paid (required for Sandbox)
#    - Navigate to Cloudflare Dashboard
#    - Click "Workers & Pages"
#    - Scroll down, find "Paid plan", click "Subscribe"
#    - Cost: ~$25/month base + usage

# 2. Request Sandbox SDK access
#    - Open support ticket: "Enable Sandbox SDK for account [account_id]"
#    - Usually approved within 1-2 hours
#    - Confirm access via: https://dash.cloudflare.com -> Account -> Billing

# 3. Verify AI Gateway available
#    - Navigate to "AI Gateway" in dashboard
#    - Should show "Create Gateway" button
```

**Step 2: Create R2 bucket**

```bash
# Install wrangler CLI (if not already installed)
npm install -g wrangler

# Authenticate
wrangler login

# Create R2 bucket for user data
wrangler r2 bucket create openclaw-users-data

# Verify
wrangler r2 bucket list
# Expected output: openclaw-users-data
```

**Step 3: Create Durable Objects namespace**

```bash
# Add to wrangler.toml:
[durable_objects]
bindings = [
  { name = "JOB_QUEUE", class_name = "JobQueue", script_name = "moltworker" }
]

# Deploy to create namespace (will do this in Step 5)
```

**Step 4: Create AI Gateway instance**

```bash
# Via dashboard:
# 1. Navigate to Cloudflare Dashboard
# 2. Click "AI Gateway"
# 3. Click "Create Gateway"
# 4. Fill form:
#    - Name: openclaw-ai-gateway
#    - Select account
# 5. Click "Create"
# 6. Note the GATEWAY_ID (shown on success page)
#
# Example GATEWAY_ID: 8f7a-4b2e-9d1c-6f3a
```

### Phase 1.2: Moltworker Repository Setup (Day 2 — Tommy + Alfred)

**Step 1: Clone Moltworker repo**

```bash
cd ~/clawd  # or your working directory
git clone https://github.com/cloudflare/moltworker.git
cd moltworker
npm install
```

**Step 2: Create `.env` file with secrets**

```bash
# Create .env file (DO NOT commit to Git)
cat > .env << 'EOF'
# Cloudflare Account
CLOUDFLARE_ACCOUNT_ID=your_account_id
CLOUDFLARE_API_TOKEN=your_api_token

# AI Gateway
CLOUDFLARE_AI_GATEWAY_ACCOUNT_ID=your_account_id
CLOUDFLARE_AI_GATEWAY_GATEWAY_ID=8f7a-4b2e-9d1c-6f3a  # From Step 4 above
CLOUDFLARE_AI_GATEWAY_TOKEN=optional_auth_token  # If gateway auth enabled

# OpenClaw / Claude
ANTHROPIC_API_KEY=sk-ant-...  # Backup, if not using AI Gateway
ANTHROPIC_BASE_URL=https://gateway.ai.cloudflare.com/v1/ACCOUNT_ID/GATEWAY_ID/anthropic

# Google Workspace (OAuth tokens - obtained via out-of-band flow)
GWS_GMAIL_REFRESH_TOKEN=1//...
GWS_CALENDAR_REFRESH_TOKEN=1//...
GWS_DRIVE_REFRESH_TOKEN=1//...

# Telegram
TELEGRAM_BOT_TOKEN=5123456789:ABCDEFGHIJKLMNOPQRSTUVWxyz...

# OpenClaw
OPENCLAW_GATEWAY_TOKEN=openclaw_token_...
EOF

# Verify .env is in .gitignore
echo ".env" >> .gitignore
```

**Step 2.5: Add Standard Packages** (Tommy)

```bash
# Add Vercel Agent Browser to package.json
npm install @vercel/agent-browser

# Verify installation
npm ls @vercel/agent-browser
# Expected: @vercel/agent-browser@^1.x.x
```

**Step 3: Configure wrangler.toml**

```bash
# Edit wrangler.toml
cat > wrangler.toml << 'EOF'
name = "moltworker"
main = "src/index.ts"
type = "service"
compatibility_date = "2025-01-01"
compatibility_flags = ["nodejs_compat"]

account_id = "YOUR_ACCOUNT_ID"
workers_dev = true
route = "moltworker.example.workers.dev"  # Update domain

# R2 Bucket binding
[[r2_buckets]]
binding = "USER_DATA"
bucket_name = "openclaw-users-data"
jurisdiction = "eu"  # or preferred region

# Durable Objects
[[durable_objects.bindings]]
name = "JOB_QUEUE"
class_name = "JobQueue"
script_name = "moltworker"

# Secrets (injected from .env or Cloudflare dashboard)
[env.production.vars]
ENVIRONMENT = "production"
DEBUG = false
LOG_LEVEL = "info"

# Build configuration
[build]
command = "npm install && npm run build"
watch_paths = ["src/**/*.ts"]
EOF
```

**Step 4: Test locally**

```bash
# Run Moltworker in dev mode
wrangler dev

# Expected output:
# ⛅️ wrangler 3.x.x
# ▲ [localhost:8787]

# Test health check:
# curl http://localhost:8787/health
# Expected: {"status": "ok"}
```

### Phase 1.3: Secrets & Credentials (Day 2 — Tommy)

**Step 1: Obtain GWS OAuth tokens (out-of-band)**

```bash
# This requires interactive OAuth flow - done ONCE per service account

# Save credentials template:
cat > oauth_setup.py << 'EOF'
import json
import os
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request

# For Gmail
scopes_gmail = ['https://www.googleapis.com/auth/gmail.modify']
# For Calendar
scopes_cal = ['https://www.googleapis.com/auth/calendar']
# For Drive
scopes_drive = ['https://www.googleapis.com/auth/drive.file']

# Follow TOOLS.md GCP_API_SCOPE_ADDITION_PROCEDURE.md for full setup
# Credentials will be cached locally, then moved to Cloudflare Secrets
EOF

# Run the OAuth flow (using client_secret_*.json from your GCP setup)
python oauth_setup.py

# Cache credentials locally to ~/.config/gcloud/
# Then move to Cloudflare Secrets (next step)
```

**Step 2: Upload secrets to Cloudflare**

```bash
# Upload each secret via wrangler
wrangler secret put CLOUDFLARE_API_TOKEN
# Paste your token, press Ctrl+D

wrangler secret put ANTHROPIC_API_KEY
# Paste API key

wrangler secret put GWS_GMAIL_REFRESH_TOKEN
# Paste token

# Repeat for all secrets in .env

# Verify secrets uploaded
wrangler secret list
# Shows list of uploaded secrets (no values)
```

### Phase 1.4: Deploy to Cloudflare (Day 3 — Tommy)

**Step 1: Build**

```bash
npm run build
```

**Step 2: Deploy**

```bash
wrangler deploy

# Expected output:
# ✓ Deployed to moltworker.ACCOUNT.workers.dev
# ✓ Dashboard: https://dash.cloudflare.com/...
```

**Step 3: Verify deployment**

```bash
# Test health check on live deployment
curl https://moltworker.ACCOUNT.workers.dev/health

# Expected: {"status":"ok","timestamp":"2026-03-13T..."}
```

### Phase 1.5: Configure AI Gateway Routing (Day 3 — Kelvin + Kitty)

**Step 1: Set AI Gateway routing rules**

```bash
# Via Cloudflare Dashboard:
# 1. Navigate to AI Gateway
# 2. Click "openclaw-ai-gateway"
# 3. Click "Routing"
# 4. Set Primary Model: Claude Sonnet 4.5
# 5. Set Fallbacks:
#    - Tier 1: MiniMax M2.5
#    - Tier 2: GLM-5
#    - Tier 3: Gemini Flash
```

**Step 2: Configure AI Firewall rules**

```bash
# Via Cloudflare Dashboard:
# 1. Navigate to AI Gateway
# 2. Click "Firewall"
# 3. Create rule: Block sensitive patterns
#    - Pattern: /api[_-]?key/i (case-insensitive)
#    - Action: REDACT
#    - Log: Yes
# 4. Create rule: Block PII patterns
#    - Pattern: SSN, credit card regex
#    - Action: BLOCK
#    - Log: Yes
# 5. Enable "Cost limit" alert at 80% of $5K budget
```

**Step 3: Activate Unified Billing**

```bash
# Via Cloudflare Dashboard:
# 1. Navigate to AI Gateway
# 2. Click "Billing"
# 3. Select "Unified Billing"
# 4. Top up account with $5,000 (Kelvin handles)
# 5. Confirm budget limit set
```

### Phase 1.6: Telegram Bot Setup (Day 4 — Alfred)

**Step 1: Create new Telegram bot**

```bash
# Talk to @BotFather on Telegram
# /newbot
# Follow prompts:
#   - Name: OpenClaw Suee (or Casey)
#   - Username: openclaw_suee_bot (must be unique)
# Receive: BOT_TOKEN (save securely)

# Example: 5123456789:ABCDEFGHIJKLMNOPQRSTUVWxyz123...
```

**Step 2: Configure webhook**

```bash
# Tell Telegram to send updates to Moltworker
curl -X POST https://api.telegram.org/bot${BOT_TOKEN}/setWebhook \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://moltworker.ACCOUNT.workers.dev/telegram/hook",
    "secret_token": "your_secret_for_verification"
  }'

# Verify:
curl https://api.telegram.org/bot${BOT_TOKEN}/getWebhookInfo
```

**Step 3: Upload bot token to Cloudflare**

```bash
wrangler secret put TELEGRAM_BOT_TOKEN
# Paste: 5123456789:ABCDEFGHIJKLMNOPQRSTUVWxyz...
```

### Phase 1.7: Consumer Setup (Day 5 — Tommy + Zorro)

**Step 1: Create consumer Sandbox instances**

```bash
# Via Moltworker admin dashboard or wrangler:

# For Suee:
wrangler durable-object:create JOB_QUEUE "suee-consumer"

# For Casey:
wrangler durable-object:create JOB_QUEUE "casey-consumer"

# Create R2 folders for session data:
# /suee/session.json
# /casey/session.json
```

**Step 2: Send Telegram invites**

```bash
# Template message (send via Zorro):

Hi Suee,

You're invited to test OpenClaw — a personal AI assistant.

To get started:
1. Add this bot: t.me/openclaw_suee_bot
2. Send any message
3. Say hi! 👋

This is a free pilot. All done!

—Zorro
```

**Step 3: Monitor first messages**

```bash
# Check Cloudflare Workers logs:
wrangler tail

# Or via dashboard:
# https://dash.cloudflare.com/[account]/workers/view/moltworker/logs

# Expected: User sends message → Telegram webhook → Moltworker processes → Response sent
```

---

### Troubleshooting Quick Reference

| Issue | Cause | Fix |
|-------|-------|-----|
| `Error: Sandbox SDK not enabled` | Account doesn't have Sandbox access | Open support ticket (1-2 hrs) |
| `401 Unauthorized on AI Gateway` | Token expired or wrong account ID | Check Secrets; re-auth if needed |
| `Telegram webhook returns 403` | Secret token mismatch | Verify `secret_token` in setWebhook call |
| `R2 bucket not found` | Bucket name typo in wrangler.toml | Run `wrangler r2 bucket list` to verify |
| `Response timeout (>30s)` | Sandbox request too long | Use Durable Objects for long tasks |
| `Cost spike` | Model routing misconfigured | Check AI Gateway logs; manually switch to cheaper model |
| `SecretRef resolution fails` | secrets.json not mounted or path wrong | Verify R2 upload; check Sandbox mount path |
| `OpenClaw can't find credentials` | SecretRef path typo (JSON pointer) | Check `/path/to/credential` in config; use tester tool |

---

## 🤖 Command Automation Log — For Future Skill Development

**⚠️ CRITICAL: Document all commands used in initial setup here.** This enables future automation via Claude Code skill (kf-claude or similar).

### Consumer Onboarding Command Log

**For each consumer setup, Tommy/Alfred MUST capture:**
1. **All shell commands executed** (copy-paste from terminal)
2. **All interactive responses** (what was entered when prompted)
3. **All file paths** (exact paths, not generic)
4. **All secrets** (tokenized as [PLACEHOLDER])
5. **Order of operations** (step sequence matters)

**Example capture template:**

```markdown
## Consumer: Suee — Setup Commands Log

**Date**: 2026-03-17
**Engineer**: Tommy
**Duration**: 45 minutes

### Phase 1: GCP Setup

**Command 1: Create GCP project**
```
gcloud projects create suee-openclaw-gcp --name="Suee OpenClaw"
gcloud config set project suee-openclaw-gcp
```

**Input**: [GCP_PROJECT_ID]=suee-openclaw-gcp

**Phase 2: OAuth Flow**

**Command 2: Run OAuth flow**
```
python3 << 'EOF'
import json, os
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow

client_secret_file = os.path.expanduser("~/Downloads/client_secret_suee.json")
scopes = [
    'https://www.googleapis.com/auth/drive.file',
    'https://www.googleapis.com/auth/calendar',
    'https://www.googleapis.com/auth/gmail.modify'
]

flow = InstalledAppFlow.from_client_secrets_file(client_secret_file, scopes=scopes)
credentials = flow.run_local_server(port=18088)

creds_cache = os.path.expanduser("~/.config/gcloud/suee_openclaw_creds.json")
with open(creds_cache, 'w') as f:
    f.write(credentials.to_json())

print(f"✅ Credentials saved: {creds_cache}")
EOF
```

**Output**: 
```
✅ Credentials saved: ~/.config/gcloud/suee_openclaw_creds.json
Refresh token: [REFRESH_TOKEN_SUEE]
```

**Manual Input**: 
- Browser opened: ✅ Yes
- User authorized: ✅ Yes
- Credentials cached: ✅ /Users/tommy/.config/gcloud/suee_openclaw_creds.json

### Phase 3: Cloudflare + R2

**Command 3: Create secrets.json**
```bash
cat > /tmp/suee_secrets.json << 'EOF'
{
  "providers": {
    "gws": {
      "gmail": { "refreshToken": "[REFRESH_TOKEN_SUEE]" },
      "calendar": { "refreshToken": "[REFRESH_TOKEN_SUEE]" },
      "drive": { "refreshToken": "[REFRESH_TOKEN_SUEE]" }
    },
    "anthropic": { "apiKey": "[ANTHROPIC_API_KEY]" }
  }
}
EOF
```

**Command 4: Upload to R2**
```bash
wrangler r2 object put openclaw-users-data/suee/secrets.json /tmp/suee_secrets.json
```

**Output**: `Uploaded: 1.2 KB`

**Command 5: Verify upload**
```bash
wrangler r2 object list openclaw-users-data --prefix=suee/
```

**Output**: `suee/secrets.json`

### Phase 4: Telegram Bot

**Command 6: Create Telegram bot (via BotFather)**
- Manual action in Telegram
- Output: `BOT_TOKEN=[TELEGRAM_BOT_TOKEN_SUEE]`

**Command 7: Set webhook**
```bash
BOT_TOKEN="[TELEGRAM_BOT_TOKEN_SUEE]"
WEBHOOK_URL="https://moltworker.ACCOUNT.workers.dev/telegram/suee/hook"

curl -X POST https://api.telegram.org/bot${BOT_TOKEN}/setWebhook \
  -H "Content-Type: application/json" \
  -d "{
    \"url\": \"${WEBHOOK_URL}\",
    \"secret_token\": \"suee_secret_$(date +%s)\"
  }"
```

**Output**: `{"ok":true,"result":true}`

### Phase 5: Moltworker Sandbox

**Command 8: Create Sandbox instance**
```bash
wrangler durable-object:create JOB_QUEUE "suee-consumer"
```

**Output**: `Instance ID: [SANDBOX_INSTANCE_ID]`

**Command 9: Deploy Moltworker**
```bash
wrangler deploy
```

**Output**: `✓ Deployed to moltworker.ACCOUNT.workers.dev`

### Phase 6: End-to-End Test

**Command 10: Test message via Telegram**
- Manual action: Send "hello" to bot
- Expected response: OpenClaw greeting from Sandbox
- **Status**: ✅ Pass / ❌ Fail

---

### Key Findings for Skill Automation

1. **Human interaction required**: BotFather (Telegram bot creation) — cannot automate
2. **Authentication required**: OAuth flow needs browser → can be wrapped in script with error handling
3. **Timing dependencies**: Some steps wait for API availability (R2 upload, Sandbox creation) — need retry logic
4. **Token sensitivity**: All tokens must be carefully handled → use environment variables, not hardcode
5. **Validation gates**: Verify each step before proceeding to next
6. **Idempotency**: Can re-run commands safely (R2 overwrites, Terraform-like)

### Commands Suitable for Automation (Claude Code Skill)

These commands can be wrapped in a skill for future consumers:

- ✅ GCP project creation (gcloud)
- ✅ OAuth flow with browser integration (Python script)
- ✅ secrets.json creation (jq or Python JSON)
- ✅ R2 object upload (wrangler)
- ✅ Telegram webhook setup (curl)
- ✅ Sandbox provisioning (wrangler durable-object)
- ✅ Moltworker deployment (wrangler)
- ✅ End-to-end testing (curl + verification)

### Commands Requiring Manual Input

These need human intervention:

- ⚠️ Telegram bot creation (@BotFather chat)
- ⚠️ GCP project authorization (browser OAuth)
- ⚠️ Consumer name/Telegram username confirmation

---

### Future Skill: `kf-claude:provision-openclaw-instance`

**What this skill does:** Takes onboarding information (consumer name, Gmail, GCP project, Telegram username) and **fully provisions a new OpenClaw instance on Moltworker** with all credentials, secrets, and routing configured.

**Proposed command:**
```bash
/kf-claude:provision-openclaw-instance \
  --consumer-name suee \
  --consumer-email suee-openclaw@gmail.com \
  --consumer-gcp-project suee-openclaw-gcp \
  --consumer-tg-username @suee_tz \
  --consumer-tg-id 123456789
```

**Skill workflow:**

1. **Input validation**
   - Verify consumer info provided (name, email, GCP project, TG username/ID)
   - Check prerequisites (GCP project exists, Gmail account created)

2. **OAuth & Credentials (semi-automated)**
   - Launch OAuth browser flow for GWS (Gmail, Calendar, Drive)
   - Capture refresh tokens → store in memory
   - Prompt for Telegram bot creation (via BotFather) → receive bot token

3. **Secrets Management (fully automated)**
   - Generate `secrets.json` from captured tokens
   - Upload to R2: `/openclaw-users-data/[consumer]/secrets.json`
   - Verify upload successful

4. **Cloudflare Resources (fully automated)**
   - Create Durable Objects instance: `[consumer]-openclaw-instance`
   - Configure Sandbox with secrets.json mount
   - Deploy Moltworker with consumer-specific routing

5. **Standard Packages Installation (fully automated)**
   - Install Vercel Agent Browser (`npm install @vercel/agent-browser`)
   - Register as MCP tool in OpenClaw config
   - Verify browser automation capabilities

6. **Telegram Bot Setup (fully automated)**
   - Configure webhook: `https://moltworker.ACCOUNT.workers.dev/telegram/[consumer]/hook`
   - Test webhook connectivity
   - Verify bot responds to test message

7. **OpenClaw Instance Startup (fully automated)**
   - Start OpenClaw Gateway inside Sandbox
   - Load secrets.json via SecretRefs
   - Initialize GWS tools (Gmail, Calendar, Drive)
   - Initialize Vercel Agent Browser
   - Test end-to-end: Telegram message → Gateway → AI → Response (including browser commands)

8. **Output & Handoff**
   - Generate consumer bot invite link: `https://t.me/[consumer]_openclaw_bot`
   - Save all commands executed to: `memory/openclaw-instances/[consumer]/provisioning-log.md`
   - Verify installed tools: GWS (Gmail, Calendar, Drive) + Vercel Agent Browser
   - Output: Ready-to-use OpenClaw instance with credentials + standard packages
   - Slack/Telegram notification to Zorro: "Instance ready for [consumer]"

**What's automated vs manual:**

| Phase | Manual Input | Automated |
|-------|-------------|-----------|
| Consumer info | Consumer provides (email, GCP) | ✓ |
| OAuth flow | Browser authorization required | Python script handles rest |
| BotFather | Create bot in Telegram | ✓ |
| Bot token | User provides | ✓ |
| GWS credentials | Browser OAuth | Script captures tokens |
| secrets.json | Generated from tokens | ✓ Upload to R2 |
| Cloudflare setup | Account/project exists | ✓ Create instances |
| Moltworker deploy | Already deployed | ✓ Configure per consumer |
| OpenClaw startup | Image already built | ✓ Start with secrets mounted |
| Testing | Manual test message (optional) | ✓ Automated E2E test |

**Skill location:** `~/clawd/kf-claude/commands/provision-openclaw-instance.md`

**Input file format (for batch provisioning):**
```json
{
  "consumers": [
    {
      "name": "suee",
      "email": "suee-openclaw@gmail.com",
      "gcp_project": "suee-openclaw-gcp",
      "tg_username": "@suee_tz",
      "tg_id": 123456789
    },
    {
      "name": "casey",
      "email": "casey-openclaw@gmail.com",
      "gcp_project": "casey-openclaw-gcp",
      "tg_username": "@casey_bot",
      "tg_id": 987654321
    }
  ]
}
```

**Batch command:**
```bash
/kf-claude:provision-openclaw-instance --from-file consumers.json
```

**Output per consumer:**
```
✅ Instance created: suee-openclaw-instance
✅ Secrets stored: s3://R2/openclaw-users-data/suey/secrets.json
✅ Bot link: https://t.me/suee_openclaw_bot
✅ OpenClaw online at: https://moltworker.ACCOUNT.workers.dev/consumer/suee
📋 Setup log: memory/openclaw-instances/suee/provisioning-log.md
```

**Benefits of this skill:**
- 🚀 One command = full instance provisioning (45 min → 5 min)
- 📝 Captures all commands for reproducibility/auditing
- 🔄 Enables batch provisioning (multiple consumers at once)
- 🛡️ Enforces security best practices (secrets in R2, not plaintext)
- 🔌 Integrates with onboarding workflow seamlessly
- 📊 Generates provisioning reports + troubleshooting logs
- 🤖 Fully repeatable (same consumers = identical setup)
```

---

## 🌐 Standard Packages: Vercel Agent Browser

### Overview

**Vercel Agent Browser** is a web automation tool packaged with every OpenClaw instance on Moltworker.

- **GitHub**: https://github.com/vercel/agent-browser
- **Type**: MCP (Model Context Protocol) tool
- **Integration**: Native OpenClaw tool
- **Runtime**: Chromium headless (provided by Vercel)

### Capabilities

| Capability | Use Case | Example |
|------------|----------|---------|
| **Screenshot** | Capture web page state | "Take a screenshot of example.com" |
| **Navigate** | Go to URLs | "Visit github.com and list repositories" |
| **Form fill** | Auto-complete web forms | "Sign up for service X with email Y" |
| **Click** | Interact with buttons/links | "Click the login button" |
| **Extract** | Parse page content | "Get all article titles from news site" |
| **JavaScript exec** | Run custom JS | "Execute script to fetch API data" |
| **Wait** | Handle dynamic content | "Wait for table to load, then extract data" |

### Configuration

All instances have Vercel Agent Browser enabled by default:

```json
{
  "tools": {
    "browser": {
      "enabled": true,
      "provider": "vercel-agent-browser",
      "config": {
        "headless": true,
        "timeout": 30000,
        "viewport": { "width": 1920, "height": 1080 }
      }
    }
  }
}
```

### Example Commands (via Telegram)

```
User: "Find the latest news on TechCrunch"
→ Bot: Uses Vercel Agent Browser to navigate TechCrunch → extract titles → summarize

User: "What's the weather in Hong Kong?"
→ Bot: Navigate weather.com → screenshot → extract forecast → reply

User: "Automate form submission on example.com"
→ Bot: Fill form fields → click submit → capture confirmation screenshot
```

### Integration with GWS Tools

Both work together:
- **GWS tools** (Gmail, Calendar, Drive) — Direct API access
- **Vercel Agent Browser** — Web automation for sites without APIs

Example: "Check my calendar and book a meeting on Calendly"
1. Query Calendar via GWS API → get free slots
2. Use Browser to navigate Calendly → fill booking form
3. Confirm with Calendar API

### Performance & Limits

- **Timeout**: 30 seconds per operation (configurable)
- **Memory**: ~100MB per browser instance
- **Concurrency**: 1 browser per Sandbox Container (Moltworker auto-scales)
- **Cost**: Included in Cloudflare Workers billing (no extra charge)

### Future Enhancements

Potential additions to Vercel Agent Browser integration:
- [ ] Optical Character Recognition (OCR) for image text extraction
- [ ] PDF generation from web pages
- [ ] Video recording of user actions
- [ ] Proxy rotation for high-volume scraping
- [ ] JavaScript breakpoint debugging

---

## 🔐 Secrets Integration Strategy: OpenClaw SecretRefs + Cloudflare

### Architecture Decision: secrets.json + R2 vs Cloudflare Secrets

**OpenClaw's native approach uses `secrets.json` + SecretRefs** (recommended for this POC):

```
Cloudflare Moltworker Sandbox (per consumer)
  ↓
  └─→ R2 bucket: /[consumer]/secrets.json (encrypted at rest)
      ↓
      └─→ OpenClaw Gateway reads SecretRefs
          ├─→ GWS tokens (Gmail, Calendar, Drive)
          ├─→ Anthropic API key (or AI Gateway token)
          └─→ Custom provider keys
```

**Why this works:**
- ✅ OpenClaw's native security model (designed for secrets.json)
- ✅ R2 provides encryption + persistence
- ✅ One file per consumer = easy audit + rotation
- ✅ SecretRefs use file source → no environment variable pollution
- ✅ Matches Cloudflare's secrets management philosophy

**Alternative (less preferred):**
- Cloudflare Secrets Store → Environment variables → OpenClaw env SecretRefs
- Issue: Scatters credentials across multiple systems

### Implementation Flow

**Setup Phase:**
1. Consumer provides: Gmail, GCP Project ID, Telegram username
2. Tech team: OAuth flow → refresh tokens
3. Tech team: Create `secrets.json` with consumer credentials
4. Tech team: Upload to R2 (`/[consumer]/secrets.json`)
5. Moltworker Sandbox startup: Download from R2 → mount to `/workspace/secrets/`
6. OpenClaw Gateway startup: Read from `/workspace/secrets/secrets.json` via SecretRefs

**Runtime Phase:**
- OpenClaw resolves SecretRefs at startup (eager resolution)
- All credentials in memory (no re-reading from disk)
- Rotation: Update `secrets.json` in R2 → restart Sandbox → new tokens loaded

### OpenClaw SecretRef Documentation

- **Docs**: https://docs.openclaw.ai/gateway/secrets
- **Key concept**: SecretRefs are opt-in, non-plaintext credential references
- **Sources**: `env`, `file`, `exec` (we use `file` for this POC)
- **Validation**: Eager at startup (fail-fast model)
- **Atomicity**: Secrets snapshot swaps atomically on reload

### Integration Checklist

- [ ] Understand OpenClaw SecretRef model (read: https://docs.openclaw.ai/gateway/secrets)
- [ ] Test `secrets audit` command locally to find plaintext credentials
- [ ] Create template `secrets.json` file structure
- [ ] Practice OAuth flow → tokens → secrets.json → R2 upload workflow
- [ ] Test Moltworker Sandbox mounting R2 → reading secrets.json
- [ ] Verify OpenClaw can resolve SecretRefs at startup
- [ ] Document consumer secrets.json schema (for automation)

---

## 📚 Reference Links

- **Moltworker GitHub**: https://github.com/cloudflare/moltworker
- **Moltworker Deployment Guide**: https://github.com/cloudflare/moltworker/blob/main/README.md
- **Cloudflare Sandbox SDK**: https://developers.cloudflare.com/sandbox/
- **Cloudflare AI Gateway**: https://developers.cloudflare.com/ai-gateway/
- **Cloudflare Durable Objects**: https://developers.cloudflare.com/durable-objects/
- **Telegram Bot API**: https://core.telegram.org/bots/api
- **OpenClaw Docs**: https://docs.openclaw.ai
- **Cloudflare Blog (Moltworker intro)**: https://blog.cloudflare.com/moltworker-self-hosted-ai-agent/

---

## 🎯 Key Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-03-12 | Migrate from local Docker → Cloudflare Moltworker | Eliminates Mac Studio ops, enables 24/7 uptime, scales infinitely, reduces maintenance |
| 2026-03-12 | Implement intelligent AI Gateway routing (3-tier model system) | Optimize costs per-request, prove dynamic routing value, handle >80% of tasks with cheaper models |
| 2026-03-12 | Use Durable Objects for background jobs (>30s) | Work around 30s Sandbox timeout, support long-running tasks (analysis, research) |
| 2026-03-12 | Fresh start for consumer instances (Suee, Casey) | Avoid migration complexity, clean state for measurements, simpler debugging |
| 2026-03-12 | Emphasis on 24/7 uptime + cost-efficiency | Shift narrative from "local assistant" → "production AI platform" for enterprise customers |

---

**Created**: 2026-03-12 (Original)
**Last Revised**: 2026-03-12 22:15 GMT+8 (Comprehensive revision for Moltworker + 24/7 + intelligent routing)
**Lead**: Zorro (Master Concept)
**Status**: ✅ REVISED & FINALIZED → Deployment runbook ready → Setup begins Monday 2026-03-17

**Next Actions:**
1. ✅ Share revised plan with Kelvin/Kitty (MC-Cloudflare team) — **Friday 2026-03-14**
2. ⏳ Cloudflare infrastructure setup (Kelvin/Kitty) — **Monday 2026-03-17 morning**
3. ⏳ Moltworker deployment (Tommy/Alfred) — **Monday-Tuesday 2026-03-17-18**
4. ⏳ Consumer onboarding (Zorro) — **Friday 2026-03-21**
5. ⏳ Live monitoring (all teams) — **Week of 2026-03-24 (Weeks 2-3)**
