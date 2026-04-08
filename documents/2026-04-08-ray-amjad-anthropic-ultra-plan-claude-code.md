---
title: "Anthropic Just Dropped Ultra Plan for Claude Code"
tags: [video, AI, Claude, tools, development, productivity, inbox, technical, actionable]
url: https://www.youtube.com/watch?v=UNhA17l6CWw
cover: https://i.ytimg.com/vi/UNhA17l6CWw/maxresdefault.jpg
date: 2026-04-08
type: video
status: inbox
read: false
priority: medium
duration: 7:52
channel: Ray Amjad
video_date: 2026-04-06
---

# Anthropic Just Dropped Ultra Plan for Claude Code

[![Watch on YouTube](https://i.ytimg.com/vi/UNhA17l6CWw/maxresdefault.jpg)](https://www.youtube.com/watch?v=UNhA17l6CWw)

## 📖 Description

Ray Amjad reviews Claude Code's new `/ultraplan` command — a feature that offloads planning to a web-based Claude Code session. After running 10 Ultra Plans vs 10 local plans, he discovers that Ultra Plan secretly has 3 different planning modes assigned server-side, making it essentially an A/B/C test. The deep plan mode uses 4 sub-agents with a critique pass and is genuinely better for complex cross-app changes, but users have no control over which mode they receive. He recommends extracting the deep plan prompt and using it as a standalone custom skill instead.

**Channel**: Ray Amjad
**URL**: https://www.youtube.com/watch?v=UNhA17l6CWw

## 🎯 Learning Objectives

By the end of this video, you will understand:
- How the `/ultraplan` command works and what the web-based planning UI offers
- The 3 hidden planning modes inside Ultra Plan (simple, visual, deep/multi-agent)
- When Ultra Plan genuinely outperforms local planning vs when it's equivalent
- How Anthropic uses server-side A/B/C testing to evaluate planning prompts
- How to extract and repurpose the deep plan system prompt as a custom skill

## 📋 Curriculum/Contents

- **~0:00** — Introduction: what Ultra Plan is and why you might be underwhelmed
- **~0:30** — Demo: running `/ultraplan` in Claude Code, getting session URL
- **~1:00** — Web UI walkthrough: inline comments, thumbs up/down, approve plan
- **~1:45** — Teleport options: run on cloud vs teleport back to terminal
- **~2:30** — Benchmark results: 10 Ultra Plans vs 10 local plans, key differences
- **~3:30** — Discovery: 3 hidden planning modes found via binary string analysis
  - Simple plan (same as local)
  - Visual plan (adds ASCII + Mermaid diagrams)
  - Deep plan / "free subagents with critique" (multi-agent exploration)
- **~4:30** — Deep plan architecture: 4 sub-agents (code understanding, file identification, risk analysis, critique)
- **~5:00** — A/B/C testing revelation: Anthropic controls assignment via remote config
- **~5:45** — Hypothesis: Anthropic measuring plan acceptance rates to benchmark prompts and future models
- **~6:15** — Use cases where Ultra Plan excels: blast radius analysis, dependency upgrades
- **~6:45** — Recommendation: extract deep plan prompt as standalone custom skill (link in description)
- **~7:20** — Summary: Ultra Plan is faster and better for multitasking, but mode assignment is opaque

## 📝 Notes & Key Takeaways

### Main Insights

- **Ultra Plan is an A/B/C test in disguise** — Anthropic assigns one of 3 planning modes server-side; users have zero visibility or control over which they receive
- **Deep plan = multi-agent + critique**: 4 sub-agents handle code understanding, file discovery, risk identification, and a final critique pass — this is meaningfully better for complex tasks
- **Consistently 2x faster** than local plan across all 10 test runs, likely due to cloud compute
- **Better for blast radius analysis**: caught more issues when migrating tRPC v10→v11; outperformed local plan on cross-app changes
- **Not always better**: for simpler isolated changes (e.g., swapping one model for another), Ultra Plan provided no advantage over local planning
- **Mermaid/ASCII diagrams** appear when assigned to the visual plan variant — their presence/absence now has an explanation

### Actionable Points

1. Use `/ultraplan` for complex dependency upgrades or multi-file refactors where blast radius matters
2. Leave inline comments on the web plan UI before approving — this refines the plan before execution
3. **Extract the deep plan system prompt** from Claude Code binary strings and deploy it as a custom `/deepplan` skill (Ray shares this in description)
4. When evaluating Ultra Plan output, consider you may have received the simple or visual variant — if the plan looks mid, it's not necessarily Ultra Plan's ceiling
5. Use Ultra Plan for multitasking: fire off several plans simultaneously, review async in web UI

### Personal Reflections

*Add your own notes here after watching*

## ⭐ Rating & Review

After completion:
- **Quality (1-5):** _/5
- **Relevance (1-5):** _/5
- **Would recommend:** Yes / No
- **Best for:** Claude Code power users who want to understand what's happening under the hood with Ultra Plan, and practitioners looking to replicate the deep plan behavior as a custom skill

## 🏷️ Auto-Generated Tags

**Content Analysis:**
- **Type**: `video` (YouTube content)
- **Topics**: AI, Claude, tools, development, productivity
- **Complexity**: Intermediate — assumes familiarity with Claude Code; some binary/reverse-engineering context is advanced
- **Priority**: Medium — Ultra Plan is genuinely useful but still experimental; extracting the deep plan prompt is the most durable takeaway

**Why These Tags:**
- `AI` + `Claude` — core subject is a Claude Code feature
- `tools` — evaluates a specific tool feature and workflow
- `development` — relevant to software development workflows using AI assistants
- `productivity` — faster planning and multitasking angles
- `technical` — includes binary string analysis and system prompt inspection
- `actionable` — concrete recommendation: extract deep plan prompt as custom skill

**Suggested Bases Filters:**
- Find similar content: `type = video AND tags contains "Claude"`
- Find high-priority learning: `priority = high AND status = inbox`

## 🔗 Related Topics & Further Research

**Related Searches:**
- Claude Code planning mode comparison (local vs Ultra)
- Multi-agent critique patterns for software planning
- Claude Code custom skills and system prompts
- A/B testing in AI product development
- tRPC v10 → v11 migration (referenced as test case)

---

**Captured**: 2026-04-08
**Source**: https://www.youtube.com/watch?v=UNhA17l6CWw
**Channel**: Ray Amjad

**Connection to Other Notes:**
- Related to Claude Code features and workflow optimization
- The deep plan prompt extraction connects to custom skills development (kf-claude, kf-cli)
