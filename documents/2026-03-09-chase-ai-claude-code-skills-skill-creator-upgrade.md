---
title: "Claude Code Skills Just Got a MASSIVE Upgrade"
tags: [video, AI, Claude, tools, automation, productivity, inbox, tutorial, technical]
url: https://www.youtube.com/watch?v=UxfeF4bSBYI
cover: https://i.ytimg.com/vi/UxfeF4bSBYI/maxresdefault.jpg
date: 2026-03-09
type: video
status: inbox
read: false
priority: high
duration: 12 minutes
channel: Chase AI
video_date: 2026-03-04
---

# Claude Code Skills Just Got a MASSIVE Upgrade

[![Watch on YouTube](https://i.ytimg.com/vi/UxfeF4bSBYI/maxresdefault.jpg)](https://www.youtube.com/watch?v=UxfeF4bSBYI)

## 📖 Description

Anthropic released the **Skill Creator** — a brand new tool that lets you test, benchmark, and optimize your Claude Code skills using plain language. This video breaks down everything you need to know: skill types explained, what the Skill Creator enables, and a step-by-step workflow demo building a skill from scratch and running an eval.

**Channel**: Chase AI
**URL**: https://www.youtube.com/watch?v=UxfeF4bSBYI

## 🎯 Learning Objectives

By the end of this video, you will understand:
- The two types of Claude Code skills (capability uplift vs. encoded preference) and why the distinction matters for evals
- How the Skill Creator enables AB testing, benchmarking, and trigger description optimization
- How to install and use the Skill Creator skill inside Claude Code
- Why testing removes the "black box" uncertainty from skill performance
- How to ensure skills reliably trigger with optimized descriptions

## 📋 Curriculum/Contents

| Timestamp | Topic |
|-----------|-------|
| 0:00 | Why This is Huge — the problems with skills before this update |
| 2:47 | Skill Types & Evals — capability uplift vs. encoded preference |
| 7:27 | The Tests — regression catching, AB testing, multi-agent parallel tests, trigger optimization |
| 8:59 | Using Skill Creator in Claude Code — installation and live demo |
| 11:15 | More Resources |

## 📝 Notes & Key Takeaways

### Main Insights

1. **Two skill types require different evals:**
   - **Capability uplift** — makes Claude better at something it was weak at (e.g., front-end design skill). These may become obsolete as models improve; evals detect when that happens.
   - **Encoded preference** — enforces a specific workflow/process Claude could already do, but in a defined order/style (e.g., YouTube → NotebookLM pipeline). Evals verify fidelity to the workflow.

2. **Skill triggering is a real problem.** Skills aren't always preloaded — Claude reads a title + ~100-word description to decide which skill to invoke. Too broad = false triggers; too narrow = never fires. Skill Creator optimizes this description automatically.

3. **AB testing is now built-in.** You can run skill vs. no-skill benchmarks, or compare two skill versions, with multiple parallel agents simultaneously. Key metrics: pass rate, token usage, total time.

4. **Evals prevent model upgrade regressions.** When a newer Claude model drops, a capability uplift skill that helped the old model may actually *hurt* the new one. Evals catch this.

5. **Encoded preference skills are durable but need fidelity checks.** The Skill Creator confirms each step of a workflow pipeline is executing in the correct order.

### Actionable Points

- [ ] Install the Skill Creator: `/plugin` → search "skill creator" → install → `/exit` → restart
- [ ] Audit existing skills: identify which are capability uplift vs. encoded preference
- [ ] Run evals on high-priority skills to get baseline pass rates
- [ ] Use Skill Creator to optimize trigger descriptions for any skill that doesn't fire reliably
- [ ] Re-run evals after Claude model upgrades to detect regressions

### Personal Reflections

*Add your own notes here after watching*

## ⭐ Rating & Review

After completion:
- **Quality (1-5):** _/5
- **Relevance (1-5):** _/5
- **Would recommend:** Yes / No
- **Best for:** Claude Code power users who build custom skills and want systematic quality control

## 🏷️ Auto-Generated Tags

**Content Analysis:**
- **Type**: `video` (YouTube content)
- **Topics**: `AI`, `Claude`, `tools`, `automation`, `productivity`
- **Complexity**: Intermediate — assumes familiarity with Claude Code and skills basics
- **Priority**: High — directly actionable for anyone building or maintaining Claude Code skills

**Why These Tags:**
- `AI` + `Claude` — core topic is Anthropic's Claude Code platform
- `tools` — covers a specific new tool (Skill Creator)
- `automation` — skill pipelines and automated eval runs
- `productivity` — improving reliability and performance of AI workflows
- `tutorial` — step-by-step installation and demo included
- `technical` — covers evals, benchmarking, AB testing concepts

**Suggested Bases Filters:**
- Find similar content: `type = video AND tags contains "Claude"`
- Find high-priority learning: `priority = high AND status = inbox`

## 🔗 Related Topics & Further Research

**Related Searches:**
- Anthropic Skill Creator blog post: https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills
- Chase AI community: https://www.skool.com/chase-ai-community
- Claude Code official plugins list
- Front-end design skill (capability uplift example)
- NotebookLM + Claude Code integration (encoded preference example)

---

**Captured**: 2026-03-09
**Source**: https://www.youtube.com/watch?v=UxfeF4bSBYI
**Channel**: Chase AI

**Connection to Other Notes:**
- Related to Claude Code skills workflow
- See also: NotebookLM integration notes
