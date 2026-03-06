---
title: "Openclaw + FREE GLM 5 API : Full Step By Step Guide"
tags: [video, AI, development, tools, automation, inbox, tutorial, technical]
url: https://www.youtube.com/watch?v=D7xcoJs146s
cover: https://i.ytimg.com/vi/D7xcoJs146s/maxresdefault.jpg
date: 2026-03-06
type: video
status: inbox
read: false
priority: medium
duration: 8 minutes
channel: Codedigipt
video_date: 2026-02-15
---

# Openclaw + FREE GLM 5 API : Full Step By Step Guide

[![Watch on YouTube](https://i.ytimg.com/vi/D7xcoJs146s/maxresdefault.jpg)](https://www.youtube.com/watch?v=D7xcoJs146s)

## 📖 Description

A full step-by-step guide on integrating the GLM-5 free API endpoint with OpenClaw. GLM-5 uses an advanced Mixture-of-Experts (MoE) architecture — activating only needed parameters for faster responses and lower cost with strong reasoning. The free API access is available via Modal Research until April 30th.

**Channel**: Codedigipt
**URL**: https://www.youtube.com/watch?v=D7xcoJs146s

## 🎯 Learning Objectives

By the end of this video, you will understand:
- How to install OpenClaw on Windows, Mac, and Linux
- How to configure a custom OpenAI-compatible API provider in OpenClaw
- How to obtain a free GLM-5 API key from Modal Research
- How to set the correct model ID (`zai-org/GLM-5-FP8`) and base URL
- How to start the OpenClaw gateway service and access it via browser
- How to troubleshoot common errors (context window too small, gateway port issues)
- How to edit `openclaw.json` to fix context window settings (set to 32,000 tokens)

## 📋 Curriculum/Contents

| Timestamp | Topic |
|-----------|-------|
| 0:00 | Introduction — GLM5 free API integration overview |
| 0:30 | Installing OpenClaw (Linux/Mac vs Windows cmd/PowerShell) |
| 1:30 | Running the onboard widget with `openclaw onboard` |
| 2:00 | Selecting "Quick Start" and "Full Reset" configuration |
| 2:30 | Choosing "Custom Provider" and entering the base URL |
| 3:00 | Generating an API key from Modal Research (Create Token) |
| 3:30 | Setting Model ID: `zai-org/GLM-5-FP8` and alias `GLM5` |
| 4:30 | Skipping optional hooks/skills/channels setup |
| 5:00 | Gateway service install and browser access |
| 5:30 | Troubleshooting: "model context window too small" error |
| 6:00 | Fixing `openclaw.json` — setting context window to 32,000 |
| 7:00 | Restarting gateway with `openclaw gateway port 18789` |
| 7:30 | Live demo: sending "hi" and receiving GLM-5 response |

## 📝 Notes & Key Takeaways

### Main Insights

- **GLM-5 is free until April 30th** via Modal Research — no cost to experiment
- **OpenClaw** is an AI agent framework that supports custom OpenAI-compatible endpoints
- **Base URL**: `https://api.us-west-2.modal.direct/v1`
- **Model ID**: `zai-org/GLM-5-FP8` (FP8 quantized for efficiency)
- The setup uses `openclaw onboard` wizard — select "Custom Provider" for third-party APIs
- Common gotcha: default context window is set too small (4,096 tokens); fix to 16,000–32,000 in `openclaw.json`
- Gateway command to restart: `openclaw gateway port 18789`

### Actionable Points

- [ ] Sign up at [modal.com/glm-5-endpoint](https://modal.com/glm-5-endpoint) to get free API key
- [ ] Install OpenClaw and run `openclaw onboard`
- [ ] Set Base URL: `https://api.us-west-2.modal.direct/v1`
- [ ] Set Model ID: `zai-org/GLM-5-FP8`
- [ ] After setup, check `~/openclaw/openclaw.json` and set `contextWindow` to `32000`
- [ ] Run `openclaw gateway port 18789` to start gateway, then open browser

### Personal Reflections

*Add your own notes here after watching*

## ⭐ Rating & Review

After completion:
- **Quality (1-5):** _/5
- **Relevance (1-5):** _/5
- **Would recommend:** Yes / No
- **Best for:** Developers wanting to run GLM-5 locally via OpenClaw with zero API cost

## 🏷️ Auto-Generated Tags

**Content Analysis:**
- **Type**: `video` (YouTube tutorial)
- **Topics**: `AI`, `development`, `tools`, `automation`
- **Complexity**: Beginner-friendly — step-by-step with commands in description
- **Priority**: Medium — free tier has expiry (April 30th), time-sensitive opportunity

**Why These Tags:**
- `AI` + `development` — core topic is integrating an LLM API into an agent framework
- `tools` — OpenClaw is a developer productivity tool
- `automation` — agent-based workflows are the end goal
- `tutorial` — pure walkthrough format with live demo
- `technical` — involves CLI commands, JSON config edits, API keys

**Suggested Bases Filters:**
- Find similar content: `type = video AND tags contains "tutorial"`
- Find high-priority learning: `priority = high AND status = inbox`
- AI tools collection: `type = video AND tags contains "AI" AND tags contains "tools"`

## 🔗 Related Topics & Further Research

**Related Searches:**
- OpenClaw documentation and other supported models
- GLM-5 vs GLM-4 benchmark comparisons
- Other free LLM API endpoints (Groq, Together AI, Fireworks)
- Modal Research platform — other hosted models available
- OpenAI-compatible API wrappers for local/remote LLMs

---

**Captured**: 2026-03-05
**Source**: https://www.youtube.com/watch?v=D7xcoJs146s
**Channel**: Codedigipt

**Connection to Other Notes:**
- Related to OpenClaw workspace governance analysis if previously captured
- Links to any notes on free LLM APIs or agent frameworks
