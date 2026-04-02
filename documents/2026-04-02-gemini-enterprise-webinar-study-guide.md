---
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
/
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

| Tier | Users | Storage | Key Features |
|---|---|---|---|
| **Business** | Up to 300 | 25 GB/user | Connectors, NotebookLM, no-code agents |
| **Standard** | Unlimited | 30 GB/user | + Gemini Code Assist, priority model access |
| **Plus** | Unlimited | 75 GB/user | Same developer tools as Standard |
| **Frontline** | Unlimited | 2 GB/user | Streamlined for field workers |

---

### Do we always get the newest Gemini models?
Yes. Gemini Enterprise is a high-priority tier and gets near-immediate access to new models. Example: Gemini 2.5 Flash was available to GE users the day after release.

---

### Which connectors are available? What if ours isn't listed?

| Type | Examples |
|---|---|
| **First-party** (Google) | Drive, BigQuery, etc. |
| **Third-party** | OneDrive, SharePoint, ServiceNow, Jira, Confluence (incl. on-prem) |

The list is under active development. If your system isn't available, you can build a custom connector via the API.

---

### Can agents be controlled with human-in-the-loop?
Yes. You can define escalation paths and confidence thresholds so agents pause for manual approval on sensitive or low-confidence tasks. For automated monitoring, implement LLM-as-judge scoring on agent outputs, combined with periodic manual sampling by domain experts.

---

### How does access control work — who sees what?
Two parallel streams run on every query: data retrieval and access control sync. Answers are only grounded in data the logged-in user is permitted to see — permissions are synced from connected third-party systems in real time. Audit logs are available to track who accessed what and when.

---

### What's included in the license price vs. billed separately?

| | What's covered |
|---|---|
| **Included in license** | Chat, Agent Designer (no-code), pre-built Google agents, image/video generation (Imagen/Veo to usage limits), connector setup |
| **Separately billed** | Compute for agents on Agent Engine or Cloud Run outside GE, processing in your own custom systems |

---

### What's the difference between Gemini Workspace and Gemini Enterprise?

| Product | Audience | What it is |
|---|---|---|
| **Gemini Pro (Google One)** | Individuals | Personal plan |
| **Google Workspace** | Enterprises | Suite with Gemini embedded in Docs, Slides, Meet, etc. |
| **Gemini Enterprise** | Enterprises | Separate agentic platform, licensed independently |

They can be used together but are sold separately.

---

### What's the difference between Gemini Enterprise and Anti-Gravity?

> *Original audience question: "What's the difference between Gemini Enterprise and Anti-Gravity?"*
> **Anti-Gravity** is now officially called **[Google Antigravity](https://antigravity.google/)** — an AI-first IDE (VS Code fork) for developers, announced November 2025 with Gemini 3. Gemini Enterprise was previously called **Agentspace**.

| Product | Target Users | Purpose |
|---|---|---|
| **Gemini Enterprise** *(formerly Agentspace)* | Every employee | Broad agentic productivity platform |
| **Google Antigravity** *(called "Anti-Gravity" in the video)* | Software developers | AI-first IDE for agentic coding workflows |

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
