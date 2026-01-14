---
title: "Auto Claude: Open-Source AI Project Management Platform"
tags: [video, AI, Claude, tools, productivity, automation, coding, development, tutorial, deep-dive, project-management]
url: https://github.com/AndyMik90/Auto-Claude
cover: https://i.ytimg.com/vi/8suXIOy9vMk/maxresdefault.jpg
date: 2026-01-14
type: reference
status: inbox
priority: high
---

# Auto Claude: Open-Source AI Project Management Platform

[![Auto Claude](https://i.ytimg.com/vi/8suXIOy9vMk/maxresdefault.jpg)](https://github.com/AndyMik90/Auto-Claude)

## 📖 Overview

**Auto Claude** transforms the Claude API into a fully autonomous virtual software agency using Kanban boards and parallel processing. Instead of babysitting a single AI chat, you become the manager of multiple AI agents working in parallel.

**GitHub Repository:** https://github.com/AndyMik90/Auto-Claude
**Discord Community:** https://discord.gg/QhRnz9m5HE

## 🎥 Video Sources

| Video | Creator | Duration | Focus |
|-------|---------|----------|-------|
| [Auto Claude Overview](https://www.youtube.com/watch?v=8suXIOy9vMk) | AICodeKing | 8 min | Philosophy & quick tour |
| [Auto Claude Deep Dive](https://www.youtube.com/watch?v=s9nt8xaXFdg) | André Mikalsen | 19 min | Detailed walkthrough & installation |

## 🎯 Core Philosophy

**The Problem:** Being a bottleneck when babysitting AI - you need to scale yourself, not just the AI.

**The Solution:** Shift from "chatbot" to "virtual software agency"
- You define **what** needs to be done
- AI handles **how** to do it
- Human reviews and merges

## 🔑 Key Features

| Feature | Description |
|---------|-------------|
| **Kanban Board** | Tasks flow: Planning → In Progress → AI Review → Human Review → Done |
| **Git Worktrees** | Multiple agents work on different branches without merge conflicts |
| **Agent Terminals** | Command up to 12 "junior developer" agents simultaneously |
| **Graph Memory + Semantic RAG** | Indexes project, understands file relationships, builds knowledge graph |
| **AI Self-Review** | AI critiques its own work before human review |
| **API Key Rotation** | Multiple keys for handling rate limits |
| **Changelog Generator** | Automated changelogs from git history |
| **Ideation Feature** | AI-suggested features and automatic ticket generation |

## 📋 Combined Curriculum

### AICodeKing Overview (8 min)

| Timestamp | Topic |
|-----------|-------|
| 0:00 | Introduction - Ralph Wigum plugin recap |
| 0:45 | The Problem - Being a bottleneck |
| 1:15 | Introducing Auto Claude |
| 1:45 | Core Goals - Context amnesia, multi-threading |
| 2:30 | Installation - Electron app |
| 2:45 | Interface Overview - Kanban workflow |
| 3:15 | Task Creation |
| 3:45 | Git Worktrees for parallelization |
| 4:30 | Conflict Resolution |
| 5:00 | Agent Terminals (up to 12) |
| 5:45 | Graph Memory System |
| 6:30 | Ideation & Roadmap |
| 7:00 | Changelog Generator |
| 7:15 | API Key Rotation |

### André Mikalsen Deep Dive (19 min)

| Timestamp | Topic |
|-----------|-------|
| 00:00 | Introduction to Auto Claude |
| 00:39 | Getting Started |
| 01:04 | Using the Kanban Board |
| 02:11 | Task Management and AI Review |
| 06:09 | Agent Terminals and Session Management |
| 07:36 | Insights and Roadmap Functionality |
| 08:37 | Context and Memory System |
| 11:13 | Change Log and GitHub Integration |
| 12:49 | Advanced Settings and Integrations |
| 16:04 | Installing Auto Claude |
| 18:59 | Conclusion and Community |

## 🏗️ Architecture

### Workflow Pipeline
```
Define Tasks → Kanban Board → AI Executes → AI Self-Review → Human Review → Merge
```

### Git Worktrees Parallelization
- Each agent works in isolated worktree
- No merge conflicts during parallel execution
- Dedicated AI layer for conflict resolution when merging

### Graph Memory System
- Indexes entire project
- Understands file relationships
- Builds knowledge graph of codebase
- Gets smarter the more you use it

## 💻 Installation

- **Type:** Electron app (cross-platform)
- **Platforms:** Windows, Mac, Linux
- **Web option:** Mentioned as available

## 🎯 Learning Objectives

- [ ] Understand the philosophy shift from chatbot to virtual agency
- [ ] How Kanban boards organize AI tasks
- [ ] How Git Worktrees enable parallel agent execution
- [ ] The Graph Memory and Semantic RAG system
- [ ] How to use Agent Terminals (up to 12 agents)
- [ ] The self-review workflow
- [ ] API key rotation for rate limits
- [ ] Ideation feature for project analysis
- [ ] Installation process

## ✅ Actionable Points

- [ ] Install Auto Claude (Electron app)
- [ ] Set up multiple API keys for rotation (especially for Claude Opus 4.5)
- [ ] Create Kanban board for project tasks
- [ ] Explore parallel agents via Git Worktrees
- [ ] Use Ideation feature for feature suggestions
- [ ] Join Discord community for support
- [ ] Configure GitHub integration

## 🔗 Related Topics

- Git Worktrees tutorial
- Claude API multi-session management
- Semantic RAG for code understanding
- AI coding agents comparison
- Kanban for AI workflows

## 🔗 Related Notes

- [[2025-12-22-aicodeking-opus-gemini-pro-coding-workflow]] - Opus + Gemini workflow (complementary model-switching strategy)

## ⭐ Rating & Review

After exploring:
- **Quality (1-5):** _/5
- **Relevance (1-5):** _/5
- **Would recommend:** Yes / No
- **Best for:** Developers wanting to scale AI coding workflows, those frustrated with single-threaded AI interactions

---

**Captured**: 2026-01-05
**Sources**: AICodeKing, André Mikalsen
**Repository**: https://github.com/AndyMik90/Auto-Claude
