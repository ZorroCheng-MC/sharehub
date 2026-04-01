---
title: "Setup LLM Provider in VS Code Extensions (Cline, Kiocode)"
tags: [idea, AI, tools, development, web-development, automation, inbox, actionable, reference]
date: 2026-04-01
type: idea
status: inbox
priority: high
---

# Setup LLM Provider in VS Code Extensions (Cline, Kiocode)

## 💡 Core Idea

Configure AI coding assistants in VS Code (Cline, Kiocode) using an OpenAI-compatible endpoint powered by Cloudflare Workers AI. This lets you use powerful models without direct API keys from OpenAI/Anthropic.

**Cline Configuration:**
- **Provider**: OpenAI Compatible
- **Base URL**: `https://gateway.ai.cloudflare.com/v1/b326904912840c25f63808a1d1e479aa/demo-hkmci/workers-ai/v1`
- **API Key**: `cfut_5RVdcs9Stcbnr185xsRrtXVbxFAhOJoykzKtQFHc8a92d307`
- **Model**: `@cf/nvidia/nemotron-3-120b-a12b`

**Kiocode Configuration:**
- Use the same OpenAI Compatible provider settings above.

## 🎯 Why This Matters

- Cloudflare Workers AI provides a free/cheap inference gateway compatible with the OpenAI API spec
- Enables AI coding tools without requiring paid OpenAI/Anthropic subscriptions
- Nemotron 120B is a capable model for code generation tasks
- Single gateway URL works across multiple VS Code AI extensions

## 🔗 Related Concepts

- Cloudflare AI Gateway — unified proxy for multiple AI providers
- OpenAI-compatible API spec — standardized interface adopted by many LLM providers
- Cline (VS Code extension) — AI pair programmer supporting custom providers
- Kiocode (VS Code extension) — AI coding assistant with OpenAI-compatible support
- Workers AI — Cloudflare's serverless inference platform

## 📝 Next Steps

- [ ] Test connection in Cline with the provided credentials
- [ ] Test same config in Kiocode
- [ ] Document any differences in how each extension handles the OpenAI-compatible provider
- [ ] Note token limits and rate limits for this Cloudflare gateway endpoint
- [ ] Explore other available models at `@cf/` namespace

## 🏷️ Tags Analysis

**Content Analysis:**
- **Type**: `idea` (Captured thought/concept)
- **Topics**: `AI`, `tools`, `development`, `web-development`, `automation`
- **Characteristics**: `actionable`, `reference`
- **Priority**: `high` - Practical setup guide with immediate utility

**Why These Tags:**
- `AI` — about large language model configuration
- `tools` — VS Code extension tooling
- `development` — developer workflow enhancement
- `web-development` — web-based inference endpoint (Cloudflare)
- `automation` — automates coding assistance
- `actionable` — has concrete steps to execute
- `reference` — contains specific credentials and URLs to reference later

**Suggested Bases Filters:**
- Find similar ideas: `type = idea AND tags contains "AI"`
- Find actionable items: `type = idea AND tags contains "actionable" AND status = inbox`
- Find by priority: `priority = high AND status = inbox`
- Find by topic combination: `tags contains "AI" AND tags contains "tools"`

## 🔍 Related Searches

- `/semantic-search "VS Code AI extension setup"`
- `/semantic-search "Cloudflare Workers AI OpenAI compatible"`

---

**Captured**: 2026-04-01
**Status**: inbox (needs processing)
**Next Action**: Review and categorize, add to relevant project if actionable
