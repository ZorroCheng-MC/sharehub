---
title: "OpenClaw × Cloudflare AI Gateway — Pilot Project Plan"
tags: [project, AI, tools, automation, business, development, technical, actionable]
date: 2026-03-12
type: project
status: inbox
priority: high
---

# OpenClaw × Cloudflare AI Gateway — Pilot Project Plan

![Hero Image](/sharehub/images/openclaw-cloudflare-lobster-farm.png)

## 🎯 Project Overview

**Objective**: Run a controlled POC where invited users access OpenClaw via Telegram, with all LLM traffic routed through Cloudflare AI Gateway. The goal is to observe how Cloudflare AI Gateway and AI Firewall behave under real usage, and identify specific value propositions Master Concept can bring to enterprise customers — enabling us to sell Cloudflare professional services with strong, evidence-based selling points.

**Sponsor**: Cloudflare (USD $5,000 token budget)
**Project Lead**: Zorro (Master Concept)
**Duration**: ~3 weeks (5-day setup + 2-week monitoring)

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
| Tommy | Engineer | Docker container setup and deployment; one container per user on Mac Studio (Ollama); GWS service integration (Gmail, Calendar, Drive) |
| Alfred | Engineer | Telegram bot configuration per container; routing containers through Cloudflare AI Gateway endpoint; troubleshooting |

**Privileges**: Full access to infrastructure (Mac Studio, Docker, Ollama); manage consumer onboarding; review aggregate usage; no visibility into Cloudflare billing.

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
OpenClaw Container (Docker, Mac Studio)
  - One container per user
  - GWS tools enabled (Gmail, Calendar, Drive)
  - Configured with Ollama for local model support
        │
        ▼
Cloudflare AI Gateway
  - Dynamic LLM routing
  - Candidate providers: Claude Sonnet, MiniMax, Gemini Flash, GLM
  - AI Firewall (sensitive data protection)
  - Full traffic logging & analytics
        │
        ▼
LLM Providers (Cloud)
```

---

## 📅 Project Timeline

### Phase 1: Setup (Week 1 — Mon to Fri, 5 days)

| Day | Task | Owner |
|-----|------|-------|
| Mon | Cloudflare AI Gateway provisioned; dynamic routing policy drafted | Kelvin, Kitty |
| Mon | Mac Studio / Ollama environment confirmed; Docker base image prepared | Tommy, Alfred |
| Tue | First container deployed and connected to Cloudflare AI Gateway endpoint | Tommy, Alfred |
| Tue | GWS integration tested (Gmail, Calendar, Drive) in container | Tommy |
| Wed | Telegram bot configured and linked per container | Alfred |
| Wed | AI Firewall rules baseline configured | Kelvin, Kitty |
| Thu | End-to-end test with internal accounts (Zorro/Tommy/Alfred as test users) | All OpenClaw team |
| Thu | Consumer containers (Suee, Casey) provisioned | Tommy, Alfred |
| Fri | Consumer onboarding — send invitation, Telegram setup, confirm access | Zorro |
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
| 1 | Docker container image (OpenClaw + GWS + TG) | Tommy, Alfred | Day 3 |
| 2 | Cloudflare AI Gateway configured with dynamic LLM routing | Kelvin, Kitty | Day 2 |
| 3 | AI Firewall baseline rules | Kelvin, Kitty | Day 3 |
| 4 | Consumer containers deployed (Suee, Casey) | Tommy | Day 5 |
| 5 | Weekly Gateway usage analytics report | Kelvin, Kitty | End of each week |
| 6 | Final findings report with enterprise selling points | Zorro | End of Week 3 |

---

## 💰 Budget & Quota

- **Total Sponsorship**: USD $5,000 (provided by Cloudflare)
- **Managed by**: MC-Cloudflare team (Kelvin, Kitty)
- **Token allocation per user**: TBD by MC-Cloudflare team based on number of active consumers
- **Overage policy**: Zorro to be notified if consumption approaches 80% of budget; no new users onboarded until confirmed safe

---

## ✅ Success Criteria

The POC is considered successful if we can document at least **two of the following**:

1. **Cost saving evidence** — Dynamic LLM routing demonstrably reduces token cost vs. using a single premium provider
2. **Security event caught** — AI Firewall intercepted sensitive data (API keys, PII, credentials) before it reached an LLM provider
3. **Routing intelligence** — Cloudflare AI Gateway demonstrated intelligent fallback or load balancing between LLM providers
4. **Enterprise use case identified** — At least one concrete scenario where a customer would pay for this architecture (e.g. compliance, cost control, multi-model strategy)
5. **Selling point documented** — A clear, evidence-based narrative Zorro/team can use in customer conversations about Cloudflare professional services

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

**Created**: 2026-03-12
**Lead**: Zorro (Master Concept)
**Status**: Planning → Setup begins next Monday
