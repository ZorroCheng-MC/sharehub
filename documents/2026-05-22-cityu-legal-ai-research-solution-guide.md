---
read: true
---
# AI-Assisted Legal Research — Solution Guide for CityU Legal Department
## Choosing Between Manus and Claude Code + opencli

**Date:** 2026-05-22  
**Audience:** CityU Department of Law — academic researchers  
**Purpose:** Help researchers pick the right tool for their teaching and research needs  
**Relates to:** [[2026-05-22-cityu-legal-dept-academic-research-setup]] | [[2026-05-22-manus-vs-claude-code-hermes-legal-research]]  
**Tags:** #cityu #legal #academic #manus #opencli #decision-guide

---

## What Both Solutions Do

Both tools connect to your existing CityU library accounts for **Lexis+ Hong Kong** and **Westlaw Asia** — using the Chrome browser sessions you already have. Neither stores your passwords or bypasses the library's authentication. You log in to the databases normally in Chrome; the AI then drives the browser on your behalf.

The difference is **how they work** and **what kind of output they produce**.

---

## Quick Decision Guide

### Choose Manus if you answer yes to most of these:
- I want to describe my research task in plain English and get a ready-to-use result
- I need formatted output: lecture outlines, case summary tables, reading lists, slide content
- I want to work from my phone or tablet occasionally
- I am not comfortable with technical setup
- My tasks are exploratory — I follow leads, read documents, synthesise across sources

### Choose Claude Code + opencli if you answer yes to most of these:
- I need the same search run the same way every time, reliably
- I want to extract data from many cases in a consistent structure (e.g. citation, ratio, outcome as a spreadsheet)
- I need results to feed into another system — a case bank, shared drive, or database
- I am comfortable with a one-time technical setup
- I run the same type of research repeatedly across the semester

---

## Side-by-Side Comparison

| | Manus | Claude Code + opencli |
|--|--|--|
| **What it feels like** | Talking to a research assistant | Running a research script |
| **How you give instructions** | Natural language in chat | Natural language in Claude Code terminal |
| **Output** | Formatted documents, tables, outlines, slide content | Structured data, CSV, JSON, markdown |
| **Reliability on complex searches** | Good for most tasks; occasional variability | Deterministic — same input, same output |
| **Mobile access** | ✅ iOS and Android app | ✅ Via Telegram (Hermes, optional) |
| **Setup on your desktop** | 15 minutes, no technical skill needed | 1–2 hours + adapter development |
| **Dedicated machine needed** | No — runs on your existing desktop | No — but recommended for shared dept use |
| **Cost** | Free via Campus Program (see below) | Claude API: ~USD 5–15/month typical usage |
| **Works offline / on LAN** | No — requires internet to Manus cloud | Yes — only API calls need internet |

---

## Cost: Manus May Be Free for CityU

Manus runs a **Campus Program** for universities. Researchers with a `@cityu.edu.hk` email may get instant free access — no waitlist, no credit card.

**How to check:**
1. Go to `manus.im/edu`
2. Sign in with your `@cityu.edu.hk` email
3. If CityU is in the eligible list, access is granted immediately

If CityU is not yet listed, you can submit a school request on the same page. The paid Pro plan is USD 20/month if the Campus Program is unavailable.

Claude Code + opencli has no subscription fee. You pay only for Claude API usage — typically USD 5–15/month for regular research use.

---

## Where to Run It: Your Desktop vs a Dedicated Machine

### Running on your own workstation (recommended for most researchers)

Both tools work on your existing computer. For individual researchers doing their own teaching preparation, there is no need for a separate machine.

- **Manus**: Install the Browser Operator extension in Chrome. Done.
- **Claude Code + opencli**: Install Node.js, opencli, and Claude Code. One-time setup.

### A dedicated shared machine (optional, for department-wide use)

If the department wants a **shared research pipeline** — one setup that all staff can query — a dedicated machine makes sense. This is a single computer (e.g. a Mac mini) that stays on, holds the library credentials, and runs the AI stack. Researchers query it remotely.

| | Individual workstation | Dedicated shared machine |
|--|--|--|
| Who sets it up | Each researcher | IT or one designated person |
| Credentials | Each person's own CityU login | One shared library account |
| Access from mobile | Manus app / Telegram | Telegram (via Hermes) |
| Best for | Personal research prep | Dept-wide shared pipeline |
| When to consider | Default starting point | When 3+ researchers want shared access |

---

## Use Cases Mapped to Solutions

| Teaching / Research Task | Recommended Tool |
|--------------------------|-----------------|
| Draft a lecture outline on a legal topic | **Manus** |
| Summarise 5–10 cases for student handouts | **Manus** |
| Build a comparative table (HK vs UK vs AU) | **Manus** |
| Generate an annotated reading list | **Manus** |
| Quick case lookup while away from desk | **Manus** (mobile app) |
| Extract 30–50 cases with consistent fields for a case bank | **Claude Code + opencli** |
| Run the same jurisdiction search every week | **Claude Code + opencli** |
| Feed results into a shared spreadsheet or database | **Claude Code + opencli** |
| Search both Lexis+ and Westlaw and deduplicate | **Claude Code + opencli** |
| Both tools needed: draft outline + structured case data | **Both** — Manus for writing, opencli for data |

---

## Recommended Starting Point

For most CityU legal academics preparing teaching materials:

1. **Try Manus first** — check `manus.im/edu` with your CityU email this week
2. Install the Browser Operator extension in Chrome (5 minutes)
3. Log in to Lexis+ HK and Westlaw Asia in Chrome as normal
4. Run a test: ask Manus to find and summarise 5 cases on a topic you teach
5. If the output quality meets your needs → you are done, no further setup required
6. If you need structured batch extraction → add Claude Code + opencli for those specific tasks

---

## Footnote: Cybersecurity and Confidentiality

> **For academic teaching research, the risk level is low.** The notes below are for awareness, not alarm.

**Manus (cloud-based):**  
Your research queries pass through Manus's cloud servers. For academic work — searching published case law for teaching purposes — this is acceptable. Manus states they do not use user queries to train their base models, and the Campus Program is subject to their standard privacy policy. As a general practice, avoid including unpublished student work, confidential draft materials, or personal data in Manus prompts.

**Claude Code + opencli (local-first):**  
The browser session, credentials, and database navigation stay entirely on your machine. Only the research query and results are sent to Anthropic's Claude API (the same exposure as using Claude.ai in a browser). This makes it the more conservative choice if your institution has strict data governance policies.

**Browser credentials — both tools:**  
Neither tool stores your CityU EID or library passwords. They reuse the session cookies that Chrome already holds after you log in manually. Your credentials are never extracted or transmitted. Treat the machine running these tools as you would any computer with your library account open in a browser.

**If running on a dedicated shared machine:**  
Apply standard IT hygiene — disk encryption, strong machine password, screen lock, restrict physical access. Ensure only authorised staff can log in to the machine.

**Who to ask for guidance:**  
CityU IT Services Desk for institution-specific data policies. For questions about library licence compliance with automated access, contact the Run Run Shaw Library directly.
