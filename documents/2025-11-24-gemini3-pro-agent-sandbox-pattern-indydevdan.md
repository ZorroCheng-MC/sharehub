---
title: "Gemini 3 Pro & Agent Sandbox Pattern"
type: video
source: youtube
url: https://youtu.be/V5IhsHEHXOg
channel: IndyDevDan
duration: 29 minutes
date_published: 2025-11-24
date_captured: 2025-11-24
priority: high
tags:
  - video
  - AI
  - Claude
  - Gemini
  - development
  - architecture
  - coding
  - automation
  - technical
  - tutorial
  - actionable
  - inbox
thumbnail: https://img.youtube.com/vi/V5IhsHEHXOg/maxresdefault.jpg
---
# Gemini 3 Pro & Agent Sandbox Pattern

## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/V5IhsHEHXOg" frameborder="0" allowfullscreen></iframe>

| Field | Value |
|-------|-------|
| **Channel** | IndyDevDan |
| **Duration** | 29 minutes |
| **Published** | 2025-11-24 |
| **URL** | [Watch on YouTube](https://youtu.be/V5IhsHEHXOg) |

## Description

> **TL;DR**: Model intelligence isn't the limitation anymore - YOU are. The breakthrough is giving AI agents their own dedicated computers (sandboxes) to operate autonomously. This enables "Best of N" pattern: spin up multiple agents in parallel, let them compete, choose the best result.

## Summary

IndyDevDan explores the paradigm shift in AI agent development: model benchmarks matter less while agent architecture matters more. The video demonstrates giving AI agents dedicated virtual computers (sandboxes) via E2B, enabling true autonomous coding with isolation, security, and scale. Key pattern introduced is "Best of N" - spinning up multiple agent sandboxes in parallel across different models (Gemini 3 Pro, Claude Code, Codex) and selecting the best result.

## Curriculum

### Module 1: Agent Sandboxes Introduction (0:00-5:50)
- [ ] Understanding dedicated virtual computers for AI agents
- [ ] E2B as sandbox provider
- [ ] Benefits: autonomy, isolation, security, scale
- [ ] Zero touch to local machine concept

### Module 2: Do Models Matter Anymore? (5:50-11:52)
- [ ] Benchmark analysis vs real-world performance
- [ ] Why agentic experience beats raw benchmarks
- [ ] The shift from model capability to architecture
- [ ] 100k subscriber milestone discussion

### Module 3: Reprogramming Agents (11:52-17:22)
- [ ] Creating memory files (CLAUDE.md, GEMINI.md, AGENTS.md)
- [ ] Defining custom backslash command syntax
- [ ] Mapping commands to skill prompts
- [ ] Universal agent skill sharing

### Module 4: Full Stack Agent Results (17:22-26:45)
- [ ] Comparing Gemini 3 Pro, Claude Code, Codex 5.1 Max
- [ ] SQLite CRUD interface builds
- [ ] Note-taking app with persistence
- [ ] SVG and image generation tests

### Module 5: Agent Sandbox Skill Breakdown (26:45-29:00)
- [ ] Skill directory structure
- [ ] Prompt templates and workflows
- [ ] plan-build-host-test workflow pattern
- [ ] Extending for custom use cases

## Key Concepts

### 1. Agent Sandboxes
- **What**: Dedicated virtual computers for AI agents to operate
- **Provider**: E2B (e2b.dev) offers cloud-based agent sandboxes
- **Benefits**:
  - More autonomy for agents, less management for you
  - Complete isolation = security
  - Scale to many agents solving many problems simultaneously
  - Zero touch to local machine

### 2. The "Best of N" Pattern
```
1. Fire up 15 agent sandboxes across:
   - Gemini 3 Pro
   - Claude Code (Sonnet 4.5)
   - Codex 5.1 Max

2. Give them same prompt (full-stack app)

3. Let them work in parallel

4. Review results, choose best one

5. Download winning code locally
```

### 3. Reprogramming Agents with Universal Skills

The key insight: use **memory files** to teach different AI agents the same custom syntax.

#### How Each Agent Reads the Rules

| Agent | Memory File | Reads From | Result |
|-------|-------------|------------|--------|
| Claude Code | `CLAUDE.md` | Direct rules | Understands `\` commands |
| Gemini CLI | `GEMINI.md` | `@CLAUDE.md` reference | Same rules, same commands |
| Codex CLI | `AGENTS.md` | `@CLAUDE.md` reference | Same rules, same commands |

#### The CLAUDE.md Rules Definition
```markdown
# Engineering Rules

## Executing Reusable Prompts

- Anytime the engineer starts a command with `\<prompt>`, look for the file:
  - **Standard prompts**: `\<prompt>` → `.claude/commands/<prompt>.md`
  - **Nested prompts**: `\sandbox:host` → `.claude/commands/sandbox/host.md`
  - **Skill Prompts**: `\<prompt>` → `.claude/skills/<skill>/<prompt>.md`
  - **Agent sandbox prompts**: `\agent-sandboxes:<prompt>` → `.claude/skills/agent-sandboxes/prompts/<prompt>.md`
```

#### The GEMINI.md File (Simple!)
```markdown
# Engineering Rules

Read @CLAUDE.md
```

The `@` syntax tells Gemini CLI to read and incorporate that file into its memory.

#### The AGENTS.md File (For Codex)
```markdown
# Engineering Rules

Read @CLAUDE.md
```

**All three agents now share identical skills!**

## Agent Sandbox Skill Architecture

### Full Repository Structure
```
agent-sandbox-skill/
├── CLAUDE.md              # Rules for Claude Code
├── GEMINI.md              # Rules for Gemini CLI → @CLAUDE.md
├── AGENTS.md              # Rules for Codex CLI → @CLAUDE.md
├── .env.sample            # E2B_API_KEY template
├── .claude/
│   └── skills/
│       └── agent-sandboxes/
│           ├── SKILL.md           # Core skill documentation (18KB)
│           ├── build_template.py  # Build automation
│           ├── sandbox_cli/       # Python CLI tool (sbx)
│           ├── examples/          # Usage examples
│           └── prompts/
│               ├── sandbox.md                  # \sandbox command
│               ├── plan-build-host-test.md     # Full workflow
│               ├── plan-full-stack.md          # Planning phase
│               ├── build.md                    # Build phase
│               ├── host.md                     # Hosting phase
│               └── test.md                     # Testing phase
└── prompts/
    └── full_stack/
        ├── codex/         # Prompts optimized for Codex
        ├── gemini/        # Prompts optimized for Gemini
        └── sonnet/        # Prompts optimized for Claude
```

### The `\sandbox` Command Definition

**File**: `.claude/skills/agent-sandboxes/prompts/sandbox.md`
```markdown
# Purpose

Build and manage E2B sandboxes to run code in isolation.

## Variables

USER_REQUEST: $1

## Workflow

1. Read and execute `.claude/skills/agent-sandboxes/SKILL.md` to validate environment
2. Execute on the `USER_REQUEST` using sandbox skill end to end
3. If user requests 'host', use `get_host` to retrieve the public URL
   - Test with `curl <public url>` to validate access
   - Restart server before presenting URL to user

## Report

Report sandbox ID and URL if applicable.
```

### The Full Workflow Command

**Command**: `\agent-sandboxes:plan-build-host-test <prompt> <workflow_id>`

**File**: `.claude/skills/agent-sandboxes/prompts/plan-build-host-test.md`

**Workflow Steps**:
1. **Initialize Sandbox** - `uv run sbx init --template fullstack-vue-fastapi-node22 --timeout 3600`
2. **Plan** - `\agent-sandboxes:plan-full-stack [USER_PROMPT]` - Generates implementation plan
3. **Build** - `\build [path_to_plan]` - Implements in sandbox
4. **Host** - `\host [sandbox_id] [PORT]` - Exposes with public URL
5. **Test** - `\agent-sandboxes:test [sandbox_id] [public_url]` - Validates everything
6. **Report** - Summarizes with sandbox ID and URL

**Tech Stack** (Pre-configured template):
- Frontend: Vite + Vue 3 + TypeScript + Pinia
- Backend: FastAPI + uvicorn + Python (uv)
- Database: SQLite
- Template: `fullstack-vue-fastapi-node22`

### CLI Commands Reference

The `sbx` CLI provides these command groups:

| Command | Description |
|---------|-------------|
| `sbx init` | Quick sandbox initialization with timeout |
| `sbx sandbox` | Lifecycle: create, connect, kill, pause, info, get-host |
| `sbx files` | File ops: ls, read, write, upload, download, rm, mkdir |
| `sbx exec` | Run commands in sandbox (most powerful) |
| `sbx browser` | Playwright automation for visual validation |

**Key exec options**:
```bash
uv run sbx exec $SANDBOX_ID "command" [options]
  --cwd PATH          # Working directory
  --env KEY=VALUE     # Environment variables
  --root              # Run as root
  --shell             # Enable pipes, redirections
  --timeout SECONDS   # Command timeout (default: 60)
  --background        # Run in background
```

## Setup Guide

### Step 1: Clone the Repository
```bash
git clone https://github.com/disler/agent-sandbox-skill.git
cd agent-sandbox-skill
```

### Step 2: Configure E2B API Key
```bash
cp .env.sample .env
echo "E2B_API_KEY=sbx_your_key_here" >> .env
```
Get your key from [E2B Dashboard](https://e2b.dev/dashboard/keys)

### Step 3: Run Any Agent
```bash
# Claude Code
claude

# Gemini CLI
gemini

# Codex CLI
codex
```

### Step 4: Execute Commands
```bash
# Simple sandbox task
\sandbox "Create a Python script that generates random passwords"

# Full workflow
\agent-sandboxes:plan-build-host-test "Build a todo list app" "todo-v1"
```

### Starter Prompts by Difficulty

**Very Easy**:
```bash
\agent-sandboxes:plan-build-host-test "$(cat prompts/full_stack/sonnet/very_easy_counter.md)" "counter"
```

**Easy**:
```bash
\agent-sandboxes:plan-build-host-test "$(cat prompts/full_stack/sonnet/easy_notes_app.md)" "notes"
```

**Medium**:
```bash
\agent-sandboxes:plan-build-host-test "$(cat prompts/full_stack/sonnet/medium_habit_tracker.md)" "habits"
```

**Hard**:
```bash
\agent-sandboxes:plan-build-host-test "$(cat prompts/full_stack/sonnet/hard_api_testing.md)" "api-test"
```

## Main Insights

1. **Model Performance < Agent Architecture**: Gemini 3 Pro dominates benchmarks but Claude Code still delivers most reliable results due to complete agentic experience

2. **The Real Benchmark**: "The only benchmark that truly matters is YOUR specific use case... Do models matter anymore? Every single release, they matter less."

3. **What Actually Matters Now**:
   - The complete agentic experience
   - The agent architecture you build
   - How well tooling and workflows work together
   - Your ability to scale compute

4. **Agent Sandboxes Unlock Scale**: Give agents dedicated computers for isolation, security, and parallel execution

5. **Best of N Selection**: Let multiple approaches compete, then choose the winner

## Results Comparison

| Model | Reliability | Strengths | Weaknesses |
|-------|-------------|-----------|------------|
| **Gemini 3 Pro** | 4/5 apps | Best benchmarks, good SVG generation | Some stalls on complex workflows |
| **Claude Code (Sonnet 4.5)** | 5/5 apps | Most reliable, best consistency | Lower raw benchmarks |
| **Codex 5.1 Max** | 4/5 apps | Best UI aesthetics | Some API integration issues |

## Practical Applications Built

1. **SQLite CRUD Interface** - Full backend + frontend + database
2. **Note-taking App** - With persistence and search
3. **Nano Banana Pro UI** - Image generation interface
4. **SVG Generators** - Pokemon cards, pelican skateboard

## Actionable Points

- [ ] Clone [agent-sandbox-skill](https://github.com/disler/agent-sandbox-skill) repository
- [ ] Get E2B API key from [e2b.dev/dashboard/keys](https://e2b.dev/dashboard/keys)
- [ ] Set up `.env` with `E2B_API_KEY`
- [ ] Test `\sandbox` command with simple prompt
- [ ] Run full workflow with `\agent-sandboxes:plan-build-host-test`
- [ ] Implement Best of N pattern: run same prompt across Claude, Gemini, Codex
- [ ] Create custom skills by adding prompts to `.claude/skills/`

## Resources

- [Agent Sandbox Skill (GitHub)](https://github.com/disler/agent-sandbox-skill) - IndyDevDan's open-source skill for running agents in E2B sandboxes with backslash command syntax
- [E2B - Agent Sandbox Provider](https://e2b.dev/) - Cloud platform providing isolated virtual machines for AI agents to execute code autonomously
- [E2B Dashboard](https://e2b.dev/dashboard/keys) - Get your API key here
- [Gemini CLI GitHub](https://github.com/google-gemini/gemini-cli) - Open-source AI agent bringing Gemini to terminal
- [Gemini CLI Commands](https://geminicli.com/docs/cli/commands/) - Full command reference including `/memory`, `/tools`, MCP support
- [Gemini 3 Pro Announcement](https://blog.google/technology/developers/gemini-3-developers/) - Google's developer blog post on capabilities
- [Agentic Horizon Course](https://agenticengineer.com/tactical-agentic-coding) - IndyDevDan's paid course on tactical agentic coding patterns
- [IndyDevDan YouTube](https://www.youtube.com/@indydevdan) - Channel focusing on AI agent development and practical engineering

## Target Audience

Developers building AI agent systems who want to move beyond single-model usage to multi-agent architectures. Ideal for those interested in scaling AI coding workflows, comparing model performance in real scenarios, and implementing production-ready agent patterns.

## Related Topics

- AI agent architecture
- E2B sandboxes
- Best of N selection
- Claude Code SDK
- Gemini CLI
- Multi-agent systems
- Agentic coding patterns
- Backslash command reprogramming

## Connections

- [[Claude Code SDK Documentation]]
- [[AI Agent Architecture Patterns]]
- [[E2B Sandbox Setup]]
- [[Multi-Agent Orchestration]]
- [[Gemini CLI Setup]]

---

## Tag Analysis

**Content Type**: video (tutorial format)
**Topics**: AI, Claude, Gemini, development, architecture (multi-model comparison)
**Priority**: high - Directly actionable agent patterns
**Metadata**: tutorial (step-by-step), technical (deep implementation), actionable (practical techniques)

### Bases Filtering Suggestions
- `type = video AND tags contains "AI" AND tags contains "architecture"` - AI architecture videos
- `tags contains "Claude" OR tags contains "Gemini"` - AI model comparisons
- `tags contains "actionable" AND tags contains "development"` - Practical dev content
- `tags contains "automation" AND tags contains "coding"` - Agentic coding content

---
*Captured: 2025-11-24*
*Channel: IndyDevDan*
*Duration: 29 minutes*
