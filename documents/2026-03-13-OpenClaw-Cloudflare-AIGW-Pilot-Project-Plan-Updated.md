# OpenClaw × Cloudflare AI Gateway — Pilot Project Plan (Updated 2026-03-13)

## 🎯 Project Overview

Objective: Deploy OpenClaw as a 24/7 production service on Cloudflare Moltworker with intelligent AI Gateway routing. Users access via Telegram with zero downtime. The goal is to demonstrate serverless AI agent architecture at scale, validate cost efficiency through smart model selection, and identify key selling points for Cloudflare professional services.

Key Innovation: AI Gateway's intelligent routing + fallback handles model selection based on task complexity, latency targets, and cost in real-time. No manual intervention needed.

**Sponsor:** Cloudflare (USD $5,000 token budget)  
**Project Lead:** Zorro (Master Concept)  
**Duration:** ~3 weeks (5-day setup + 2-week live operation + monitoring)

## 🛠️ Technology Stack (Moltworker)

### Cloudflare Products:

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

### OpenClaw Integration:

- **GitHub:** cloudflare/moltworker (official Cloudflare implementation)
- **OpenClaw Gateway** runs inside Sandbox Container
- **Telegram routing** via Moltworker API layer
- **GWS credentials** stored securely in R2 + Secrets (encrypted at rest)

### Standard Packages per Instance:

- **Vercel Agent Browser** — Web automation, form filling, screenshot capture
  - GitHub: vercel/agent-browser
  - Integration: MCP-compatible tool for OpenClaw
  - Use cases: Web scraping, automated testing, form submission
  - Configuration: Via secrets.json (if auth required)

### Monitoring & Analytics:

- Cloudflare AI Gateway dashboard (request logs, token usage, costs)
- Moltworker custom metrics (Sandbox CPU/memory, container uptime)
- OpenClaw internal metrics (session count, tool usage, errors)

## 👥 Stakeholders, Roles & Responsibilities

### 🟠 MC-Cloudflare Team

| Name | Role | Responsibilities |
|------|------|------------------|
| Kelvin | Sponsor / Gateway Admin | Set up Cloudflare AI Gateway policies; configure dynamic LLM routing; manage billing and token quotas; define AI Firewall rules |
| Kitty | Sponsor / Gateway Admin | Co-manage AI Gateway and Firewall configuration; monitor policy enforcement; provide usage analytics and reports |

**Privileges:** Full control over Cloudflare AI Gateway dashboard, billing, quota allocation, and firewall rule configuration. No access to OpenClaw containers or user data.

### 🔵 OpenClaw Team (Master Concept)

| Name | Role | Responsibilities |
|------|------|------------------|
| Zorro | Project Lead | Overall project coordination; consumer invitation and onboarding; stakeholder communication; final findings report; go/no-go decisions |
| Tommy | Engineer | Moltworker deployment on Cloudflare (via GitHub repo); Sandbox Container provisioning per user; GWS service integration (Gmail, Calendar, Drive) via R2/Secrets Store; credential lifecycle management; **Create @dev.hkmci.com account for each consumer and subscribe to Brave Search API** |
| Alfred | Engineer | Telegram bot routing setup (via Moltworker API); OpenClaw Gateway configuration within sandbox; custom skills adaptation; monitoring and debugging within Sandbox environment |

**Privileges:** Full access to Moltworker codebase, Cloudflare Workers dashboard, Sandbox SDK; R2 bucket management for user data; Secrets Store configuration; review aggregate usage logs; no direct access to Cloudflare billing (managed by MC-Cloudflare team).

### 🟢 Consumer Team

| Name | Status | Access |
|------|--------|--------|
| Suee | Active (invited) | Telegram → OpenClaw container |
| Casey | Active (invited) | Telegram → OpenClaw container |
| Future users | Invited by Zorro | Same as above |

**Privileges:** Use OpenClaw via Telegram only. No direct access to containers, LLM provider credentials, or Cloudflare dashboard.

**Agreed Terms (all consumers must acknowledge before onboarding):**
- All traffic between OpenClaw and LLM providers is monitored by Cloudflare AI Gateway
- The AI Firewall may block or redact sensitive information (e.g. API keys, credentials) before it reaches the LLM
- This is a sponsored POC — token consumption is free within the allocated budget

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

### Key Architectural Changes:

- **No local hardware** — Moltworker runs entirely on Cloudflare's global network (24/7 uptime, no maintenance)
- **Sandboxed isolation** — Each user gets isolated Sandbox Container via Sandbox SDK
- **Persistent storage** — R2 bucket for session memory, configs, GWS credentials (encrypted)
- **Credential security** — Secrets Store for LLM keys, OAuth tokens, API credentials
- **Browser Rendering** — Puppeteer/Playwright via Cloudflare (no local Chromium needed)
- **Background jobs** — Durable Objects handle tasks >30s (e.g., long-running research, document processing)

## 🧠 AI Gateway Intelligent Model Routing Strategy

### Model Tier System

**Tier 1: Fast & Cheap (Entry tasks)**
- Models: GLM-5, MiniMax M2.5, Claude Haiku
- Use case: Simple Q&A, email drafts, quick lookups, transcription
- Cost: ~50% less than Sonnet
- Latency: <500ms (usually <200ms)
- Accuracy: 85-90% for routine tasks

**Tier 2: Balanced (Standard tasks)**
- Models: Claude Sonnet 4.5, Gemini Flash, GLM-5-Plus
- Use case: Content creation, analysis, tool usage, complex reasoning
- Cost: Baseline (reference point)
- Latency: 500-1500ms
- Accuracy: 95%+ for most tasks

**Tier 3: Powerful (Complex tasks)**
- Models: Claude Opus (when available via API), Gemini 2.0
- Use case: Deep analysis, novel problem-solving, edge cases
- Cost: 2-3x Sonnet
- Latency: 1-3s
- Accuracy: 99% for specialized reasoning

### AI Gateway Configuration (Intelligent Routing)

AI Gateway will be configured with:
- **Primary model:** Claude Sonnet 4.5
- **Fallback chain:** [MiniMax M2.5 → GLM-5 → Gemini Flash]
- **Cost threshold:** Route to Tier 1 if task is classified as "simple"
- **Latency constraint:** If <500ms needed → use Tier 1 only

**How It Works:**

1. Request arrives from Telegram user (via Moltworker)
2. AI Gateway inspects request metadata:
   - Task type (from OpenClaw classification)
   - Token estimate (word count heuristic)
   - Time constraint (implicit from Telegram interaction)
   - User quota remaining
3. Smart routing decision:
   - Simple task (Q&A, lookup) → Route to GLM-5 (cheapest)
   - Standard task (normal chat) → Route to Sonnet 4.5 (balanced)
   - Complex task (reasoning, analysis) → Route to Opus (if quota allows)
   - Primary unavailable → Automatic fallback to next tier
   - Budget at risk (>80% consumed) → Force downgrade to cheaper model
4. Request forwarded to selected provider
5. Response returned to Moltworker → Telegram user

### Cost Optimization Features (Built into AI Gateway)

| Feature | Benefit | How It Helps |
|---------|---------|--------------|
| Request caching | Deduplicate identical prompts | Same question twice = 1st hit cached |
| Token-level compression | Reduce token count before sending | Markdown formatting → saves ~5-10% |
| Batch processing | Combine multiple small requests | Mail summary → 1 request vs. N |
| Fallback cost handling | If Tier 2 fails, retry Tier 1 (cheaper) | Fault tolerance + cost savings |
| Quota-aware routing | Dynamically degrade quality to save budget | Never exceed $5K limit |

## 24/7 Uptime Strategy

### Cloudflare provides:

- ✅ Global edge network (no SPOF)
- ✅ Auto-scaling Sandbox Containers (handle traffic spikes)
- ✅ Worker cold-start: ~50ms (no "starting up" delays)
- ✅ SLA: 99.99% uptime (Cloudflare guarantees)

### Moltworker provides:

- ✅ Always-on Workers (billing per request, not per hour)
- ✅ Persistent session data in R2 (survive container restarts)
- ✅ Durable Objects for background tasks (don't timeout)

### OpenClaw integration:

- ✅ Stateless design (each request independent)
- ✅ Session memory persisted to R2 (resume interrupted conversations)
- ✅ No scheduled restarts needed (unlike local Docker)

**Result:** Service runs 24/7 with zero planned downtime.

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
- User: "Analyze the top 100 GitHub issues for my repo"
- Moltworker: "Got it, analyzing. I'll send results via Telegram when ready" (instant response)
- Durable Object: Spawns long-running Sandbox, iterates issues, synthesizes report (~2-5 min)
- User: Receives "Analysis complete: [summary]" via Telegram notification

**Cost:** Durable Objects billed separately (~$0.15/M reads + $1.15/M writes). Likely trivial for this POC.

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

## 📦 Deliverables

| # | Deliverable | Owner | Due |
|---|-------------|-------|-----|
| 1 | Moltworker deployed to Cloudflare Workers (from cloudflare/moltworker repo) | Tommy, Alfred | Day 2 |
| 2 | Cloudflare AI Gateway configured with dynamic LLM routing + Unified Billing | Kelvin, Kitty | Day 1 |
| 3 | AI Firewall baseline rules (credential masking, sensitive data detection) | Kelvin, Kitty | Day 3 |
| 4 | R2 bucket provisioned for user session persistence; Secrets Store configured | Tommy | Day 2 |
| 5 | Consumer Sandbox instances created and Telegram routing tested (Suee, Casey) | Tommy, Alfred | Day 5 |
| 6 | Moltworker admin dashboard setup (monitoring, user management, logs) | Alfred | Day 3 |
| 7 | Weekly Gateway usage analytics + Sandbox performance report | Kelvin, Kitty | End of each week |
| 8 | Final findings report with enterprise selling points (serverless advantage, cost, scaling) | Zorro | End of Week 3 |

## 💰 Budget & Quota

- **Total Sponsorship:** USD $5,000 (provided by Cloudflare)
- **Managed by:** MC-Cloudflare team (Kelvin, Kitty)
- **Token allocation per user:** TBD by MC-Cloudflare team based on number of active consumers
- **Overage policy:** Zorro to be notified if consumption approaches 80% of budget; no new users onboarded until confirmed safe

## ✅ Success Criteria

The POC is considered successful if we can document at least FOUR of the following:

### Primary Metrics (Must have 2+)

**Intelligent model routing reduces costs by >20%**
- Success: AI Gateway's dynamic routing saves tokens vs. always using Sonnet
- Measurement: Compare actual spend to "all Sonnet" baseline
- Target: Reduce $5K budget by >$1K through smart tier selection

**24/7 uptime achieved with zero downtime**
- Success: System runs continuously for 2 weeks with no planned/unplanned restarts
- Measurement: Cloudflare uptime dashboard shows 100% or >99.9%
- Target: Prove serverless is more reliable than local Docker

**AI Firewall blocks sensitive data**
- Success: Firewall catches and redacts at least 1 real PII/API key attempt
- Measurement: Firewall logs show blocked patterns with redaction
- Target: Demonstrate security value of gateway inspection

### Secondary Metrics (Pick 2 more for full success)

**Sandbox scalability validated**
- Success: Easily onboard 5+ new consumers without infra changes
- Measurement: Add Suee, Casey, +3 more in <30 min each
- Target: Prove Sandbox auto-scaling works

**Edge latency advantage proven**
- Success: Cloudflare edge responds faster than API-direct calls
- Measurement: p50/p95 latency to Cloudflare vs. direct LLM provider
- Target: Show 200-500ms edge advantage for typical requests

**Durable Objects handles long-running tasks**
- Success: At least 1 background job (>30s) completes successfully
- Example: Analyze 10+ GitHub issues, email thread summarization
- Measurement: Task completes, user notified, result correct

**Cost model beats alternatives**
- Success: Total 2-week spend demonstrates value vs. alternatives
- Comparison: Moltworker cost vs. Mac Studio + local Ollama vs. traditional VPS
- Target: Moltworker = cheapest per-user option

**Enterprise customer conversation ready**
- Success: Zorro can pitch specific customer segments (startups, enterprises, security-conscious)
- Selling points: "24/7 serverless", "cost-optimized AI", "built-in security", "no ops overhead"
- Measurement: 3-5 written talking points with evidence from this POC

## 🧑💼 Consumer Preparation Checklist

Each consumer needs to prepare BEFORE tech team setup begins.

### Consumer Self-Service Checklist (Pre-Setup) — SIMPLIFIED

| # | Task | Owner | Example | Status |
|---|------|-------|---------|--------|
| 1 | Confirm Telegram username/ID for bot link | Consumer | @suee_tz or ID: 123456789 | ⏳ |
| 2 | Review & acknowledge pilot terms (data monitoring, security) | Consumer | Reply: "I acknowledge and accept" | ⏳ |

### Pre-Setup Email to Consumers (Send 1 week before tech setup) — SIMPLIFIED

**Subject:** OpenClaw Pilot — Action Required (2 Quick Steps)

---

Hi [Name],

Great news — you're approved to join the OpenClaw pilot! 🎉

Our tech team will handle all the infrastructure setup. You just need to do 2 quick things (2 min, one-time):

**Step 1: Confirm your Telegram contact info**

Reply to this email with:
- Your Telegram username: @suee_tz (or Telegram ID: 123456789)

✅ Telegram info confirmed

**Step 2: Review & acknowledge pilot terms**

Please acknowledge the following:

- All traffic between OpenClaw and LLM providers is monitored by Cloudflare AI Gateway (for research purposes)
- Cloudflare's AI Firewall may block or redact sensitive information (e.g., API keys, credentials) before it reaches the LLM (for security)
- This is a sponsored pilot funded by Cloudflare — token consumption is free within the allocated budget
- You understand and accept these terms

Reply: "I acknowledge and accept the above terms"

✅ Terms acknowledged

**Timeline:**
- You submit above: Friday
- Tech team prepares your account: Monday–Tuesday
- You get Telegram bot link: Wednesday
- Live access: Wednesday–Thursday

Questions? Reply to this email.

Best,  
Zorro

---

## Tech Team Setup Checklist (Tommy & Alfred)

Once consumer submits their prep info, follow this checklist per consumer.

### Consumer Onboarding Template

```
Consumer: [NAME]
Status: Ready for setup
Date: 2026-03-[XX]

CONSUMER_PROVIDED:
 telegram_username: [TG] # e.g., @suee_tz
 telegram_id: [ID] # e.g., 123456789
 terms_acknowledged: Yes

TECH_TEAM_TASKS (TOMMY RESPONSIBLE FOR INFRASTRUCTURE):
 ☐ Task 1: Create @dev.hkmci.com account for consumer (TOMMY)
 ☐ Task 2: Create GCP project for consumer (TOMMY)
 ☐ Task 3: Enable GWS APIs in GCP project (TOMMY)
 ☐ Task 4: Create OAuth 2.0 credentials (TOMMY)
 ☐ Task 5: Complete OAuth flow (out-of-band) (TOMMY)
 ☐ Task 6: Subscribe to Brave Search API and obtain API key (TOMMY)
 ☐ Task 7: Upload credentials to Cloudflare Secrets Store (TOMMY)
 ☐ Task 8: Create Telegram bot (BotFather) (ALFRED)
 ☐ Task 9: Configure Telegram webhook (ALFRED)
 ☐ Task 10: Create Sandbox instance in Moltworker (TOMMY)
 ☐ Task 11: Verify end-to-end test (ALFRED)
 ☐ Task 12: Send Telegram bot link to consumer (ZORRO)
 ☐ Task 13: Consumer confirms access (CONSUMER)

CREDENTIALS_CREATED:
 gcp_project_id: [ID] # e.g., suee-openclaw-gcp (CREATED BY TOMMY)
 developer_email: [EMAIL] # @dev.hkmci.com (CREATED BY TOMMY)
 telegram_bot_token: [TOKEN] # Secrets Store
 oauth_refresh_tokens: [STORED] # Secrets Store
 brave_search_api_key: [KEY] # Secrets Store (CREATED BY TOMMY)
 sandbox_instance: [CREATED] # Moltworker
 r2_session_folder: /[consumer]/session.json # R2
```

### Tech Team Step-by-Step (Per Consumer)

#### Step 1: Create @dev.hkmci.com Account (Tommy)

```bash
# Create new email account for consumer
# Consumer: Suee → Email: suee@dev.hkmci.com
#
# Via hkmci.com admin panel or account manager:
# 1. Go to https://admin.hkmci.com or contact admin
# 2. Create new account: suee@dev.hkmci.com
# 3. Send temporary password to consumer (separate channel)
# 4. Ask consumer to set permanent password on first login
# 5. Note: This email will be used for:
#    - GCP OAuth credentials (service account alternative)
#    - OpenClaw credential management
#    - Future integrations
```

#### Step 1.5: Create GCP Project (Tommy) — NEW

```bash
# Create a new Google Cloud project for the consumer
# This is NOW TECH TEAM responsibility (not consumer self-service)
#
# Via GCP Console:
# 1. Log in to https://console.cloud.google.com (use techadmin account or Zorro account)
# 2. Click "Select a Project" (top-left)
# 3. Click "NEW PROJECT"
# 4. Enter project name: [consumer-name]-openclaw (e.g., suee-openclaw)
# 5. Click "CREATE"
# 6. Wait 30 seconds for project initialization
# 7. Note the Project ID (auto-generated, e.g., suee-openclaw-gcp)
# 8. Proceed to Step 2 with this Project ID
#
# Store for records:
# - GCP Project ID: suee-openclaw-gcp
# - Owner email: suee@dev.hkmci.com (from Step 1)
```

#### Step 2: Enable GWS APIs in GCP project (Tommy)

```bash
# Consumer provides (via tech team):
# - GCP Project ID: suee-openclaw-gcp (from Step 1.5)
# - Developer Email: suee@dev.hkmci.com (from Step 1)

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

#### Step 3: Create OAuth 2.0 Credentials (Tommy)

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

#### Step 4: Complete OAuth Flow (Out-of-Band) (Tommy)

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

#### Step 5: Subscribe to Brave Search API and Obtain Key (Tommy)

```bash
# Subscribe consumer to Brave Search API (new step for 2026-03-13 update)
#
# Prerequisites:
# - Consumer has @dev.hkmci.com email account (from Step 1)
# - Developer account created with that email
#
# Steps:
# 1. Go to https://api.search.brave.com/dashboard
# 2. Sign in with suee@dev.hkmci.com (or create account)
# 3. Click "Subscription" or "Plans"
# 4. Choose appropriate tier:
#    - Free: 100 requests/day
#    - Paid: $0.25/1000 requests (for higher volume)
# 5. Complete payment setup (if paid tier)
# 6. Go to "API Keys" or "Credentials"
# 7. Create new API key (if not auto-generated)
# 8. Copy API key (looks like: "YOUR_BRAVE_API_KEY_HERE")
# 9. Save key securely (will upload to Secrets Store in Step 6)

# Verify API works:
# curl -X GET "https://api.search.brave.com/res/v1/web/search?q=test" \
#   -H "Accept: application/json" \
#   -H "X-Subscription-Token: YOUR_BRAVE_API_KEY_HERE"
# Expected: JSON response with search results
```

#### Step 4A: Create Consumer secrets.json File (Tommy) — RECOMMENDED APPROACH

OpenClaw now uses secrets.json for centralized credential management (via SecretRefs).

```bash
# Create consumer-specific secrets.json file
# This will be stored in R2 and injected into each Sandbox Container

cat > /tmp/suee_secrets.json << 'EOF'
{
 "providers": {
   "gws": {
     "gmail": {
       "refreshToken": "1//0gxxxx..." // from OAuth flow above
     },
     "calendar": {
       "refreshToken": "1//0gxxxx..." // same token
     },
     "drive": {
       "refreshToken": "1//0gxxxx..." // same token
     }
   },
   "anthropic": {
     "apiKey": "sk-ant-..." // AI Gateway token or backup Anthropic key
   },
   "braveSearch": {
     "apiKey": "YOUR_BRAVE_API_KEY_HERE" // from Step 5
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

#### Step 4B: Configure OpenClaw SecretRefs in Moltworker (Alfred)

In the Sandbox Container startup, configure OpenClaw to use the secrets.json via SecretRefs:

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
     "braveSearch": {
       "apiKey": { "source": "file", "provider": "consumer_file", "id": "/providers/braveSearch/apiKey" }
     },
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

wrangler secret put BRAVE_SEARCH_API_KEY_SUEE
# Paste: YOUR_BRAVE_API_KEY_HERE

# Moltworker injects as env var → OpenClaw reads via env SecretRef
# Then in OpenClaw config:
{
 "tools": {
   "gws": {
     "gmail": {
       "refreshToken": { "source": "env", "provider": "default", "id": "GWS_GMAIL_REFRESH_TOKEN_SUEE" }
     }
   },
   "braveSearch": {
     "apiKey": { "source": "env", "provider": "default", "id": "BRAVE_SEARCH_API_KEY_SUEE" }
   }
 }
}
```

**Recommendation:** Use secrets.json + R2 because:
- Fewer Cloudflare Secrets entries (one per consumer instead of 3-5)
- Matches OpenClaw's native secrets model
- Easier to backup and rotate
- Self-contained per consumer

#### Step 6: [Continue with existing steps 5-10, renumbered as 7-13]

*(Remaining tech team steps continue as per original document, with step numbers adjusted)*

---

## 🔍 Brave Search API Integration (NEW SECTION — 2026-03-13)

### Overview

Brave Search API provides fast, privacy-focused web search capabilities. This integration allows OpenClaw consumers to perform web searches within their Telegram conversations without relying on third-party search providers.

### Key Features

- **Privacy-first:** Brave Search does not track user searches
- **Fast results:** Real-time web search without ads or surveillance
- **Developer-friendly API:** Simple REST API with rate limiting
- **Cost-effective:** Competitive pricing ($0.25/1000 requests for standard tier)

### Setup & Configuration

#### A. Subscribe Consumer to Brave Search API

**Prerequisites:**
- Consumer has @dev.hkmci.com email account
- Developer dashboard access

**Steps:**
1. Consumer logs into https://api.search.brave.com/dashboard using @dev.hkmci.com email
2. Selects desired subscription tier:
   - **Free Tier:** 100 requests/day (for testing)
   - **Standard Tier:** $0.25 per 1,000 requests (recommended for production)
3. Completes subscription and payment setup
4. Generates API Key from Dashboard → API Keys section
5. Stores API key securely (tech team uploads to Secrets Store in Step 5 above)

#### B. API Key Distribution

**Tech Team Process:**
1. Store API key in `secrets.json` under `providers.braveSearch.apiKey`
2. Include Brave Search key in Cloudflare Secrets Store for backup
3. Document key rotation schedule (recommended: every 90 days)
4. Communicate key to consumer via secure channel (never email plaintext)

#### C. OpenClaw Integration

**Available Commands (via web_search skill):**

OpenClaw automatically exposes Brave Search API as `web_search` tool:

```bash
# Example: Search for information
web_search(query="latest AI developments", count=5)

# Returns JSON with:
# - title: Result title
# - url: Source URL
# - snippet: Preview text
```

**Configuration in OpenClaw:**

```yaml
tools:
  web_search:
    provider: "brave"
    apiKey: "${BRAVE_SEARCH_API_KEY}"
    defaultCount: 5
    timeout: 10000
```

#### D. Rate Limiting & Cost Management

| Tier | Requests/Day | Cost | Suitable For |
|------|-------------|------|--------------|
| Free | 100 | Free | Testing, low-volume usage |
| Standard | Unlimited | $0.25/1K | Production, typical usage |
| Premium | Unlimited | $0.50/1K | High-volume, enterprise |

**Consumer Billing:**
- Tech team monitors API key usage via Brave Search dashboard
- If consumer approaches usage tier limits, escalate to Zorro
- Coordinate tier upgrades as needed

#### E. Monitoring & Alerts

**Tech Team Dashboard Access:**
1. Log into https://api.search.brave.com/dashboard
2. View real-time request count and cost
3. Set alerts at 80% of daily/monthly quota

**Troubleshooting:**

| Issue | Cause | Solution |
|-------|-------|----------|
| 401 Unauthorized | Invalid or expired API key | Regenerate key from dashboard |
| 429 Too Many Requests | Rate limit exceeded | Upgrade subscription tier |
| No results | Query too broad or misspelled | Refine search query |
| High latency | Network issues | Retry or use fallback search |

### Security & Privacy Considerations

- **No tracking:** Brave Search API does not create user profiles or track searches
- **API key protection:** Keep keys in Secrets Store, never commit to version control
- **Request logging:** OpenClaw logs searches locally (not sent to Brave)
- **Consumer privacy:** Each consumer has isolated API key; no cross-consumer sharing

### Future Enhancements

- Integration with other search providers (Google, Bing) as fallbacks
- Smart query optimization (tokenization, caching)
- Search result ranking/filtering
- Custom search domains (search within specific sites only)

---

## 🧑💼 Tech Team Setup Checklist (Updated — Tommy & Alfred)

### Updated Task List with Brave Search Integration

**TECH_TEAM_TASKS (Updated 2026-03-13):**

- ☐ Task 1: Create @dev.hkmci.com account for consumer (Tommy)
- ☐ Task 2: Enable GWS APIs in GCP project (Tommy)
- ☐ Task 3: Create OAuth 2.0 credentials (Tommy)
- ☐ Task 4: Complete OAuth flow (out-of-band) (Tommy)
- ☐ Task 5: Subscribe to Brave Search API and obtain API key (Tommy)
- ☐ Task 6: Upload credentials to Cloudflare Secrets Store (Tommy)
- ☐ Task 7: Create Telegram bot (BotFather) (Alfred)
- ☐ Task 8: Configure Telegram webhook (Alfred)
- ☐ Task 9: Create Sandbox instance in Moltworker (Tommy)
- ☐ Task 10: Verify end-to-end test (Alfred)
- ☐ Task 11: Distribute Brave Search API key to consumer (Tommy)
- ☐ Task 12: Send Telegram bot link to consumer (Zorro)
- ☐ Task 13: Consumer confirms access (Consumer)

### Key Changes from Original (2026-03-13 Final Update)

**MAJOR SIMPLIFICATION: Minimal Consumer Self-Service → Tech Team Handles All Infrastructure**

1. **Removed:** "Create GCP project" from Consumer Preparation Checklist
   - **Reason:** Tech team (Tommy) now creates GCP projects; simplifies consumer onboarding
   - **Impact:** Consumers no longer need to touch GCP Console
   
2. **Removed:** GCP Project ID sharing requirement from consumer prep
   - **Now:** Tommy creates project directly; no consumer input needed
   
3. **Updated:** Consumer Preparation Checklist → SIMPLIFIED TO 2 ITEMS ONLY
   - ✅ Confirm Telegram username/ID (for bot link)
   - ✅ Review & acknowledge pilot terms (data monitoring, security)
   - ❌ REMOVED: GCP project creation
   - ❌ REMOVED: Gmail account creation (no longer needed)

4. **Updated:** Consumer Preparation Email
   - Removed all infrastructure setup steps
   - Simplified to just: Telegram username + terms acknowledgment
   - Clear timeline: Friday submit → Monday-Tuesday setup → Wednesday live

5. **Added:** "Create @dev.hkmci.com account" to Tommy's responsibilities (Engineer)
   - Step 1: Create unified corporate email for consumer
   - Timing: Day 1 of consumer setup
   
6. **Added:** "Create GCP project" to Tommy's responsibilities (Engineer)
   - Step 1.5: Create GCP project (consumer no longer does this)
   - Timing: Day 1-2 of consumer setup
   - Tech team now controls: project creation, APIs, credentials

7. **Added:** "Subscribe to Brave Search API" to Tommy's responsibilities (Engineer)
   - Step 5: Obtain Brave Search API key for consumer
   - Timing: Day 2 of consumer setup

8. **Added:** New "Brave Search API Integration" section
   - Comprehensive setup & configuration guide
   - Rate limiting & cost management
   - Security & privacy considerations

9. **Updated:** Tech Team Setup Checklist Template
   - Removed GCP Project ID from "CONSUMER_PROVIDED"
   - Added Tommy ownership flags to all infrastructure tasks
   - Tasks now clearly show: Tommy (infra/credentials), Alfred (Telegram/testing), Zorro (consumer comms)

10. **Updated:** Updated Tech Team Step-by-Step guide
    - New Step 1.5: Create GCP project (Tommy)
    - Step numbering adjusted for clarity

---

**Document Updated:** 2026-03-13 (Final Simplification Pass)  
**Updated by:** Subagent (infrastructure update workflow)  

**Summary of Changes in This Update:**
1. ✅ Removed "Create GCP project" from Consumer Preparation Checklist
2. ✅ Moved GCP project creation to Tech Team (Tommy) — Step 1.5
3. ✅ Simplified consumer prep to 2 items only: Telegram username + terms acknowledgment
4. ✅ Updated Consumer Preparation Email (removed all infra setup, 2-step process)
5. ✅ Updated Tech Team Setup Checklist (Tommy now responsible for @dev.hkmci.com account, GCP project, Brave Search API)
6. ✅ Integrated Brave Search API into Tech Team responsibilities
7. ✅ Result: Minimal consumer self-service → All infrastructure handled by tech team

**Next Steps for Zorro:**
- Review updated checklists
- Notify Tommy of new GCP project creation responsibility
- Use simplified Consumer Preparation Email for next consumer invitations
- Confirm timeline adjustments with stakeholders (Kelvin/Kitty)
