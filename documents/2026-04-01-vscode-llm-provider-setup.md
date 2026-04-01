---
date: 2026-04-01
title: Setup LLM Provider in VS Code Extensions (Cline, Kiocode)
tags:
  - idea
  - AI
  - tools
  - development
  - web-development
  - automation
  - inbox
  - actionable
  - reference
date: 2026-04-01
type: idea
status: inbox
priority: high
---

# Setup LLM Provider in VS Code Extensions (Cline, Kiocode)

> All models are accessed through **Cloudflare AI Gateway** (`demo-hkmci`). Users only need the gateway API token — no direct provider keys required.

---

## 👤 User Guide: Quick Model Reference

### Gateway API Token (same for all remote models)

```
API Token: [Ask your Cloudflare admin]
```

### ⚠️ Local vs Remote Base URL

| Type | Base URL pattern | Example |
|---|---|---|
| **Remote** (Cloudflare Gateway) | `https://gateway.ai.cloudflare.com/v1/{account}/{gateway}/{provider-path}` | See table below |
| **Local** (Ollama, LM Studio) | `http://localhost:{port}/v1` | `http://localhost:11434/v1` |

Use the remote URLs below when connected to the internet via the Cloudflare gateway. Switch to a local URL if running models on your own machine.

---

### Available Models

#### Cloudflare Workers AI

Provider Type: **OpenAI Compatible**
Base URL:
```
https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/workers-ai/v1
```

| Model | Model ID |
|---|---|
| Nvidia Nemotron 3 120B | `@cf/nvidia/nemotron-3-120b-a12b` |
| Moonshot Kimi K2.5 | `@cf/moonshotai/kimi-k2.5` |
| GLM 4.7 Flash | `@cf/zai-org/glm-4.7-flash` |

---

#### Google Gemini (via Google AI Studio)

Provider Type: **OpenAI Compatible**
Base URL:
```
https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/google-ai-studio/v1beta/openai
```

| Model | Model ID | Notes |
|---|---|---|
| Gemini 3.1 Pro | `gemini-3.1-pro` | New, Preview |
| Gemini 3 Flash | `gemini-3.0-flash` | Preview |
| Gemini 3.1 Flash-Lite | `gemini-3.1-flash-lite` | New, Preview |

> Preview model IDs may change — verify at `aistudio.google.com`

---

#### Anthropic Claude *(Under Investigation)*

Two paths are available — choose based on what the admin has configured:

**Option A — Direct Anthropic via Gateway** *(requires Anthropic BYOK key in gateway)*
Provider Type: **Anthropic**
Base URL:
```
https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/anthropic
```

**Option B — Claude via GCP Vertex AI** *(requires GCP billing enabled + Claude access approved)* ⚠️ Pending
Provider Type: **Anthropic**
Base URL:
```
https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/google-vertex-ai/v1/projects/mcps-testing-cloudflare-ai/locations/us-east5/publishers/anthropic/models
```

| Model | Model ID |
|---|---|
| Claude Opus 4.6 | `claude-opus-4-6` |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` |

> **Note:** Claude models on GCP Vertex AI are Anthropic's models hosted by Google — they are NOT Google's own models. Vertex AI billing must be enabled and Claude access must be approved in [Vertex AI Model Garden](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) before these become available.

---

### Screenshots

**Kilocode:**
![Kilocode Provider Setup](/images/kilocode-provider-setup.png)

**Cline:**
![Cline Provider Setup](/images/cline-provider-setup.png)

---

## 🔧 Admin Guide: Gateway Setup

### Architecture Overview

```
Cline / Kiocode
      │
      │  cfut_ token (gateway auth)
      ▼
Cloudflare AI Gateway (demo-hkmci)
      │
      ├── /workers-ai/v1        → Cloudflare Workers AI (built-in)
      ├── /google-ai-studio/... → Google Gemini  [BYOK: Google AI Studio key]
      ├── /anthropic            → Anthropic Claude [BYOK: Anthropic key]
      └── /google-vertex-ai/... → GCP Vertex AI  [BYOK: Service account JSON] ⚠️ billing required
```

No provider keys are exposed to users — all credentials are stored as **Provider Keys (BYOK)** in the gateway.

---

### Gateway Details

| Setting | Value |
|---|---|
| Account | Master Concept Demo (mcmsp.dev) |
| Account ID | `b326904912840c25f63808a1d1e479aa` |
| Gateway Name | `demo-hkmci` |
| Authentication | Enabled |
| Gateway Token | `cfut_...` (distribute to users) |

---

### Provider Keys (BYOK) Configuration

Navigate to: **Cloudflare Dashboard → AI → AI Gateway → demo-hkmci → Provider Keys**

| Provider | Alias | Key Source | Status |
|---|---|---|---|
| Google AI Studio | `default` | `aistudio.google.com/apikey` | ✅ Configured |
| Google Vertex AI | `default` | GCP service account JSON | ⚠️ Billing not enabled |
| Anthropic | `default` | `console.anthropic.com` | ⬜ Not configured |

> Always set alias to `default` so the gateway injects it automatically without requiring users to hold provider keys.

---

### How to Deliver Config to Users

Send users the following (nothing else — no provider keys):

```
Cloudflare AI Gateway – VS Code Setup

Gateway API Token: cfut_...

For model-specific Base URLs and setup instructions, see:
https://sharehub.zorro.hk/documents/2026-04-01-vscode-llm-provider-setup.html
```

---

### GCP Vertex AI Setup

The Vertex AI service account has been created and the key is stored in the gateway. Blocked on billing.

- **GCP Project**: `mcps-testing-cloudflare-ai`
- **Service Account**: `cloudflare-ai-gateway-vertex@mcps-testing-cloudflare-ai.iam.gserviceaccount.com`
- **Action required**: Enable billing at `https://console.developers.google.com/billing/enable?project=mcps-testing-cloudflare-ai`

Once billing is enabled, Vertex AI models (Gemini Enterprise tier) will be available via:
```
https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/google-vertex-ai/v1/projects/mcps-testing-cloudflare-ai/locations/us-central1/publishers/google/models
```

---

### Adding a New Provider

1. Get the API key for the provider
2. Go to **demo-hkmci → Provider Keys → find provider → click `+`**
3. Paste the key, set alias to `default`, save
4. Update the User Guide table above with the new Base URL and model IDs
5. Republish this note: `/kf-cli:publish 2026-04-01-vscode-llm-provider-setup.md`

---

**Captured**: 2026-04-01
**Last Updated**: 2026-04-01
