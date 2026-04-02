---
date: 2026-04-02
title: "Study Guide: Gemini Enterprise Webinar"
tags:
  - study-guide
  - AI
  - tools
  - learning
  - development
  - processing
  - technical
  - deep-dive
source: https://www.youtube.com/watch?v=x7Y0xc2SAzU
date: 2026-04-02
type: study-guide
status: processing
difficulty: advanced
estimated-time: ~20 min (watch) + Q&A reading
priority: medium
read: true
---

# Study Guide: Gemini Enterprise Webinar

[![Watch on YouTube](https://i.ytimg.com/vi/x7Y0xc2SAzU/maxresdefault.jpg)](https://www.youtube.com/watch?v=x7Y0xc2SAzU)

**For experienced GE teams.** Demo: watch in full (~20 min). Q&A: read here, skip in video.

> **Skip 0:00–5:54** — intro, agenda, and AI landscape overview your team already knows.

---

## 📋 Part 1 — Demo (watch in full)

### Search & summarization across connectors
`6:07` [Jump to 6:07 →](https://www.youtube.com/watch?v=x7Y0xc2SAzU&t=367s)

Study Alex's narration technique — he frames each action with a business persona before showing the feature. The citation UI and multi-source blending are worth replicating in your own demos.

---

### Content creation grounded in company data
`8:45` [Jump to 8:45 →](https://www.youtube.com/watch?v=x7Y0xc2SAzU&t=525s)

Marketing one-pager generated live from enterprise sources with citations. Note how the presenter links the business outcome directly to the feature — reusable structure.

---

### Actioning — scheduling directly from chat
`9:23` [Jump to 9:23 →](https://www.youtube.com/watch?v=x7Y0xc2SAzU&t=563s)

Completes the **Search → Create → Action** loop in one continuous workflow. This three-step sequence is the core narrative arc worth borrowing for your training flow.

---

### No-code agent builder — Safety Expert multi-agent
`9:50` [Jump to 9:50 →](https://www.youtube.com/watch?v=x7Y0xc2SAzU&t=590s)

Best ready-made template for explaining agent orchestration to non-technical audiences. Shows a main agent with two subagents using Gmail and Calendar connectors.

---

### ADK custom agent — BigQuery + image generation
`12:54` [Jump to 12:54 →](https://www.youtube.com/watch?v=x7Y0xc2SAzU&t=774s)

Natural language to SQL via ADK agent, followed by AI image generation. Watch if your team handles developer-facing or technical training.

---

## 📝 Part 2 — Q&A Answers (read here, skip video 17:58 onward)

### What are the license tiers?
Four tiers:
- **Business** — up to 300 users, 25 GB/user, core AI features (connectors, NotebookLM, no-code agents)
- **Standard** — unlimited users, 30 GB/user, adds Gemini Code Assist and priority model access
- **Plus** — unlimited users, 75 GB/user, same developer tools as Standard
- **Frontline** — streamlined tier for field workers, 2 GB/user only

---

### Do we always get the newest Gemini models?
Yes. Gemini Enterprise is a high-priority tier and gets near-immediate access to new models. Example: Gemini 2.5 Flash was available to GE users the day after release.

---

### Which connectors are available? What if ours isn't listed?
Two categories:
- **First-party** (Google — Drive, BigQuery, etc.)
- **Third-party** (OneDrive, SharePoint, ServiceNow, Jira, Confluence including on-prem)

The list is under active development. If your system isn't available, you can build a custom connector via the API. Check the public documentation for the current list.

---

### Can agents be controlled with human-in-the-loop?
Yes. You can define escalation paths and confidence thresholds so agents pause for manual approval on sensitive or low-confidence tasks. For automated monitoring, implement LLM-as-judge scoring on agent outputs, combined with periodic manual sampling by domain experts.

---

### How does access control work — who sees what?
Two parallel streams run on every query: data retrieval and access control sync. Answers are only grounded in data the logged-in user is permitted to see — permissions are synced from connected third-party systems in real time. Audit logs are available to track who accessed what and when.

---

### What's included in the license price vs. billed separately?
- **Included:** all usage within the GE interface — chat, Agent Designer (no-code), pre-built Google agents, image/video generation (Imagen/Veo to usage limits), and connector setup
- **Separately billed:** compute for agents deployed on Agent Engine or Cloud Run outside GE, and any processing in your own custom systems

---

### What's the difference between Gemini Workspace and Gemini Enterprise?
Three distinct products:
- **Gemini Pro (Google One)** — personal plan for individuals
- **Google Workspace** — enterprise suite with Gemini embedded in Docs, Slides, Meet, etc.
- **Gemini Enterprise** — separate agentic platform, not included in Workspace, licensed independently

They can be used together but are sold separately.

---

### What's the difference between Gemini Enterprise and Agentspace?
- **Gemini Enterprise** — broad productivity platform for every employee
- **Agentspace** (previously "Anti-Gravity" in the video) — specialized agent-driven tool for software developers only

Both can be used together as they serve different audiences.

---

### Is there a minimum number of licenses?
No minimum. Standard and Plus tiers start at one license. That said, testing with a meaningful user base is strongly recommended to surface real use cases and demonstrate ROI before a wider rollout.

---

### How are agents and NotebookLM connected?
NotebookLM is included in GE and supports shared org-wide access in the enterprise version. Agents can use a single NotebookLM as a grounded knowledge tool. Connecting multiple NotebookLMs to one agent requires ADK to orchestrate the data flows — this is not available out of the box via Agent Designer.

---

## ⏭️ Skip Entirely

| Section | Timestamp | Reason |
|---|---|---|
| Opening intro, agenda & AI landscape overview | 0:00–5:54 | Nothing your team doesn't already know |
| Getting started CTA, sign-up prompts & closing remarks | 15:03–17:57 | Commercial wrap-up with no training value |
| Entire Q&A section | 17:58–end | GDPR detail, n8n integration, RAG explainer, free trial, Google Ads connector — all summarised above |

---

## 🏷️ Tags Analysis

- **Type**: `study-guide` — structured watch guide for a GE webinar
- **Topics**: AI, tools, learning, development
- **Difficulty**: Advanced — assumes existing GE familiarity
- **Separately billed**: Agent Engine/Cloud Run compute + custom system processing (not covered by license)

**Suggested Bases Filters:**
- `type = study-guide AND tags contains "AI"`
- `type = study-guide AND status = processing`

---

**Created**: 2026-04-02
**Status**: Processing - Active Study
**Next Action**: Watch demo section (6:07–15:03), then review Q&A answers above
