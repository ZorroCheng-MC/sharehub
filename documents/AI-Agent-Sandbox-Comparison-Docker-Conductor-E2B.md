---
title: "AI Agent Sandbox Comparison: Docker vs Conductor vs E2B vs Claude Subagents"
type: reference
date_created: 2025-11-25
priority: high
tags:
  - AI
  - development
  - architecture
  - Docker
  - Claude
  - comparison
  - reference
  - technical
  - inbox
  - crewnest
---

# AI Agent Sandbox Comparison: Docker vs Conductor vs E2B vs Claude Subagents

## Overview

When running AI coding agents (Claude Code, Codex, Gemini CLI), isolation is critical for security, scalability, and parallel execution. This note compares four approaches: **Docker/DevContainers** (local containers), **Conductor** (Mac app), **E2B** (cloud sandboxes), and **Claude Subagents** (built-in Claude Code feature).

## Quick Comparison Table

| Feature               | Docker/DevContainers        | Conductor                      | E2B                          | Claude Subagents               |
| --------------------- | --------------------------- | ------------------------------ | ---------------------------- | ------------------------------ |
| **Type**              | Local containers            | Mac desktop app                | Cloud sandboxes              | Built-in Claude Code feature   |
| **Cost**              | Free (local resources)      | Free (uses your API keys)      | $100 free credits, then paid | Free (uses existing tokens)    |
| **Platform**          | Windows/Mac/Linux           | Mac only (Windows waitlist)    | Any (cloud-based)            | Any (where Claude Code runs)   |
| **Isolation**         | Container-level             | Git worktrees                  | Full VM isolation            | Context-level (fresh memory)   |
| **Setup Complexity**  | Medium-High                 | Very Low                       | Low                          | Zero (built-in)                |
| **Concurrent Agents** | Limited by local resources  | Unlimited (UI-managed)         | Free: 20, Pro: 100           | Unlimited (parallel Tasks)     |
| **Session Length**    | Unlimited                   | Unlimited                      | Free: 1hr, Pro: 24hr         | Per-task (context isolated)    |
| **Network Access**    | Configurable                | Full                           | Full                         | Inherits from Claude Code      |
| **Public URLs**       | No                          | No                             | Yes (https://xxx.e2b.app)    | No                             |
| **Model Selection**   | N/A                         | N/A                            | N/A                          | Sonnet/Opus/Haiku per task     |
| **Best For**          | DevOps teams, custom setups | Mac users, quick parallel work | "Best of N" pattern, scale   | Orchestrators, specialized tasks |

## Detailed Comparison

### 1. Docker / DevContainers

**What it is**: Use Docker containers as isolated development environments for AI agents.

#### Architecture
```
Your Machine
├── Docker Engine
│   ├── Container 1 (Claude Code + Project A)
│   ├── Container 2 (Codex + Project B)
│   └── Container 3 (Gemini + Project C)
└── VS Code / Cursor (connects to containers)
```

#### Setup

**devcontainer.json**:
```json
{
  "name": "AI Agent Container",
  "dockerFile": "Dockerfile",
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python",
        "rooveterinaryinc.roo-cline"
      ]
    }
  },
  "remoteUser": "vscode",
  "runArgs": ["--network=none"]  // Disable network for security
}
```

**Dockerfile**:
```dockerfile
FROM python:3.11-slim

RUN apt-get update && apt-get install -y \
    git curl vim nodejs npm \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

CMD ["/bin/bash"]
```

**Docker Compose (multi-agent)**:
```yaml
version: '3.8'
services:
  agent-1:
    build: .
    volumes:
      - ./project:/workspace
    network_mode: none

  agent-2:
    build: .
    volumes:
      - ./project:/workspace
    network_mode: none
```

#### Pros
- **Free** - No cloud costs, uses local resources
- **Full control** - Configure everything (network, volumes, resources)
- **Cross-platform** - Works on Windows, Mac, Linux
- **Offline capable** - No internet required once set up
- **Custom environments** - Install any tools/dependencies
- **CI/CD integration** - Same containers work in pipelines

#### Cons
- **Resource hungry** - Each container consumes RAM/CPU
- **Setup complexity** - Requires Docker knowledge
- **Manual orchestration** - No built-in parallel agent management
- **Limited by hardware** - Can't scale beyond your machine
- **No built-in UI** - Need terminal or VS Code

#### Best Use Cases
- DevOps teams with Docker expertise
- Custom toolchain requirements
- Offline/air-gapped environments
- CI/CD pipeline testing
- Security-critical environments (network isolation)

---

### 2. Conductor (by Melty Labs)

**What it is**: A Mac desktop app that orchestrates multiple Claude Code/Codex agents with a beautiful UI and automatic git worktree management.

#### Architecture
```
Conductor App (Mac)
├── Workspace 1 (git worktree) → Claude Code
├── Workspace 2 (git worktree) → Claude Code
├── Workspace 3 (git worktree) → Codex
└── Central UI (see all agents, review diffs, merge)
```

#### Setup

1. **Download**: Visit [conductor.build](https://conductor.build) and download the Mac app
2. **Open**: Launch Conductor
3. **Add Repo**: Point to your git repository
4. **Deploy Agents**: Click to spin up Claude Code or Codex instances
5. **Conduct**: Monitor, review diffs, create PRs

That's it. No configuration files needed.

#### Key Features

| Feature | Description |
|---------|-------------|
| **Git Worktrees** | Each agent gets isolated copy of codebase |
| **Parallel Agents** | Run unlimited agents simultaneously |
| **Diff Viewer** | Review changes before merging |
| **Linear Integration** | Start work directly from Linear issues |
| **MCP Support** | Connect to MCP servers |
| **Slash Commands** | Custom commands for agents |
| **Checkpoints** | Save/restore agent states |
| **Scripts** | Run custom scripts across workspaces |

#### Configuration (conductor.json)
```json
{
  "scripts": {
    "test": "npm test",
    "lint": "npm run lint"
  },
  "mcp": {
    "servers": ["github", "linear"]
  }
}
```

#### Pros
- **Zero setup** - Download and start immediately
- **Free** - Uses your existing Claude Code/Codex login
- **Beautiful UI** - See all agents at a glance
- **Git worktrees** - Automatic isolation without Docker
- **Linear integration** - Issue-to-PR workflow
- **Diff review** - Built-in code review before merge
- **Supports Bedrock/Vertex** - Use with AWS or GCP models

#### Cons
- **Mac only** - Windows users on waitlist
- **Not truly isolated** - Worktrees share same machine
- **No network isolation** - Agents have full network access
- **Requires Claude Code** - Must have Claude Code or Codex CLI installed
- **Local resources** - Still limited by your Mac's power

#### Best Use Cases
- Mac developers wanting quick parallel agents
- Teams using Linear for project management
- Rapid prototyping with multiple approaches
- Solo developers multiplying productivity
- Those who want UI over terminal

---

### 3. E2B (Agent Sandboxes)

**What it is**: Cloud-based isolated virtual machines for AI agents. Each agent gets its own dedicated computer.

#### Architecture
```
Your Machine
│
└── API Calls → E2B Cloud
                ├── Sandbox 1 (Full VM - Claude Code)
                ├── Sandbox 2 (Full VM - Gemini CLI)
                ├── Sandbox 3 (Full VM - Codex)
                └── ... up to 20 (free) or 100 (pro)
```

#### Setup

**1. Get API Key**:
```bash
# Sign up at e2b.dev and get key from dashboard
export E2B_API_KEY=sbx_your_key_here
```

**2. Install CLI** (using IndyDevDan's skill):
```bash
git clone https://github.com/disler/agent-sandbox-skill.git
cd agent-sandbox-skill
cp .env.sample .env
echo "E2B_API_KEY=sbx_your_key" >> .env
```

**3. Run Agents**:
```bash
# Start any agent
claude  # or gemini, or codex

# Simple sandbox task
\sandbox "Create a REST API with FastAPI"

# Full workflow
\agent-sandboxes:plan-build-host-test "Build a todo app" "todo-v1"
```

#### CLI Commands (sbx)
```bash
# Initialize sandbox (30 min timeout)
uv run sbx init --timeout 1800

# Execute commands
uv run sbx exec $SANDBOX_ID "python --version"

# File operations
uv run sbx files write $SANDBOX_ID /home/user/app.py "print('hello')"
uv run sbx files upload $SANDBOX_ID ./local.png /home/user/image.png

# Host and expose
uv run sbx sandbox get-host $SANDBOX_ID --port 5173
# Returns: https://5173-sbx_abc123.e2b.app
```

#### Pricing

| Plan | Cost | Credits | Concurrent | Session Length |
|------|------|---------|------------|----------------|
| **Hobby** | Free | $100 one-time | 20 sandboxes | 1 hour |
| **Pro** | $150/mo | + usage | 100 sandboxes | 24 hours |
| **Enterprise** | Custom | Custom | Custom | Custom |

**Usage costs** (per second):
- 1 vCPU: $0.000014/s (~$0.05/hr)
- 2 vCPU (default): $0.000028/s (~$0.10/hr)
- Memory: $0.0000045/GiB/s

**$100 free credits ≈ 750+ sandbox hours**

#### Pros
- **True isolation** - Each agent in separate VM
- **Scale** - Run 20-100 agents in parallel
- **Public URLs** - Expose apps to internet instantly
- **Pre-built templates** - Vue+FastAPI+SQLite ready to go
- **Best of N pattern** - Compare multiple agents' outputs
- **No local resources** - Your machine stays free
- **Cross-platform** - Works from any OS

#### Cons
- **Costs money** - $100 free, then pay-per-use
- **Internet required** - Cloud-dependent
- **Session limits** - 1hr free, 24hr pro
- **Latency** - Network round-trips for every command
- **Learning curve** - CLI/API to learn
- **Credits expire** - Use it or lose it

#### Best Use Cases
- "Best of N" agent competitions
- Scaling beyond local hardware
- Running untrusted code safely
- Building full-stack apps with public URLs
- Long-running autonomous agents
- Teams with API budget

---

### 4. Claude Subagents (Task Tool)

**What it is**: Claude Code's built-in mechanism for spawning independent sub-agents with their own context, tool access, and model selection. Uses the `Task` tool to launch specialized agents that work autonomously and report back.

#### Architecture
```
Claude Code (Main Session)
├── Task Tool → Subagent 1 (Explore - fast codebase search)
├── Task Tool → Subagent 2 (Plan - architecture design)
├── Task Tool → Subagent 3 (test-executor - run tests)
├── Task Tool → Subagent 4 (Custom agent from .claude/agents/)
└── All subagents report results back to main session
```

#### How It Works

**The Task Tool** spawns independent agents with:
- **Fresh context** - Each subagent starts with clean memory (no conversation history pollution)
- **Tool selection** - Different subagents can access different tools
- **Model choice** - Use Haiku for fast tasks, Sonnet for complex ones, Opus for deep reasoning
- **Parallel execution** - Multiple subagents can run simultaneously in one message

#### Built-in Agent Types

| Agent Type | Purpose | Tools Available |
|------------|---------|-----------------|
| `Explore` | Fast codebase exploration | Glob, Grep, Read |
| `Plan` | Architecture planning | All tools |
| `test-executor` | Run and validate tests | All tools |
| `design-review` | UI/UX review via Playwright | Playwright, Grep, Read |
| `claude-code-guide` | Documentation lookup | WebFetch, WebSearch, Read |
| `general-purpose` | Complex multi-step tasks | All tools |

#### Creating Custom Subagents

**Project-level** (`.claude/agents/my-agent.md`):
```markdown
---
name: youtube-capture
description: Captures YouTube videos to Obsidian vault
tools:
  - mcp__MCP_DOCKER__get_transcript
  - mcp__obsidian-mcp-tools__create_vault_file
  - Read
  - Write
model: haiku  # Fast and cheap for routine captures
---

# YouTube Capture Agent

You are a specialized agent for capturing YouTube content.

## Workflow
1. Extract video transcript using YouTube MCP
2. Generate summary and key points
3. Create note in Obsidian with proper frontmatter
4. Return note path and summary to orchestrator
```

**User-level** (`~/.claude/agents/`):
- Available across all projects
- Personal utilities and workflows

#### Invocation Patterns

**Simple Task**:
```javascript
// In Claude Code, the Task tool is used like this:
Task({
  subagent_type: "Explore",
  prompt: "Find all files that handle authentication",
  description: "Search auth files"
})
```

**Parallel Tasks** (multiple in one message):
```javascript
// Multiple Tasks execute simultaneously
Task({ subagent_type: "Explore", prompt: "Find database models" })
Task({ subagent_type: "Explore", prompt: "Find API endpoints" })
Task({ subagent_type: "Explore", prompt: "Find test files" })
// All three run in parallel, results collected
```

**Model Selection**:
```javascript
Task({
  subagent_type: "general-purpose",
  prompt: "Deep analysis of architecture",
  model: "opus"  // Use Opus for complex reasoning
})

Task({
  subagent_type: "general-purpose",
  prompt: "Quick file categorization",
  model: "haiku"  // Use Haiku for speed
})
```

**Agent Resumption** (continue previous work):
```javascript
Task({
  subagent_type: "claude-code-guide",
  prompt: "Continue researching MCP setup",
  resume: "agent_abc123"  // Resume from previous agent state
})
```

#### Orchestrator Pattern (Harry Roper's Approach)

```
┌─────────────────────────────────────────────────────────┐
│                    ORCHESTRATOR                         │
│               (Main Claude Code Session)                │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │   Memory    │  │   State     │  │  Decision   │     │
│  │   Tools     │  │  Machine    │  │   Loop      │     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
└─────────┼────────────────┼────────────────┼────────────┘
          │                │                │
          ▼                ▼                ▼
┌─────────────────────────────────────────────────────────┐
│                    SUBAGENTS                            │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │  Hypothesis │  │   Writer    │  │  Reviewer   │     │
│  │  Generator  │  │   Team      │  │   Team      │     │
│  │  (Sonnet)   │  │  (Haiku)    │  │  (Sonnet)   │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
│                                                         │
│  Each subagent has:                                     │
│  - Independent context (fresh memory)                   │
│  - Specific tool access                                 │
│  - Optimal model for task                               │
│  - Reports result back to orchestrator                  │
└─────────────────────────────────────────────────────────┘
```

#### Pros
- **Zero setup** - Built into Claude Code, no external tools needed
- **Free** - Uses your existing Claude Code subscription tokens
- **Model flexibility** - Choose optimal model per task (Haiku: fast, Sonnet: balanced, Opus: powerful)
- **Context isolation** - Each subagent has fresh memory, no pollution from long sessions
- **Parallel execution** - Launch multiple subagents simultaneously
- **Tool granularity** - Restrict which tools each subagent can access
- **Resumable** - Continue previous agent work with `resume` parameter
- **Cross-platform** - Works anywhere Claude Code runs

#### Cons
- **No persistent state** - Subagents don't remember previous invocations (unless resumed)
- **No public URLs** - Cannot expose services to internet (use E2B for that)
- **No OS isolation** - Shares same filesystem as main Claude Code
- **Token consumption** - Each subagent consumes tokens from your quota
- **Latency** - Fresh context means re-reading files each time
- **No nesting** - Subagents cannot spawn their own subagents

#### Key Differences from Other Options

| Aspect | Docker | Conductor | E2B | Subagents |
|--------|--------|-----------|-----|-----------|
| **Isolation** | OS-level | Git worktree | VM-level | Context-level |
| **Persistence** | Full | Full | Session | Per-task |
| **Public Access** | Manual | No | Yes | No |
| **Setup Required** | Yes | Download app | API key | None |
| **Model Choice** | N/A | N/A | N/A | Per-task |
| **Cost Model** | Local resources | API keys | Pay per sandbox | Included in tokens |

#### Best Use Cases
- Orchestrator patterns (main agent delegates to specialists)
- Parallel exploration (search multiple patterns simultaneously)
- Model optimization (Haiku for simple, Opus for complex)
- Context isolation (prevent long session memory pollution)
- Test automation (dedicated test-executor subagent)
- Documentation lookup (claude-code-guide for accurate info)
- Code review workflows (design-review with Playwright)

---

## Decision Matrix

### Choose Docker/DevContainers if:
- [x] You have Docker expertise
- [x] You need offline capability
- [x] You want maximum security (network isolation)
- [x] You need custom toolchains
- [x] You're on Windows or Linux
- [x] You want zero ongoing costs

### Choose Conductor if:
- [x] You're on Mac
- [x] You want zero setup time
- [x] You prefer GUI over terminal
- [x] You use Linear for project management
- [x] You want to run multiple Claude Codes easily
- [x] You already have Claude Pro/Max subscription

### Choose E2B if:
- [x] You need true VM isolation
- [x] You want to scale to many parallel agents
- [x] You need public URLs for testing
- [x] You're doing "Best of N" comparisons
- [x] You don't want to consume local resources
- [x] You have budget for cloud services

### Choose Claude Subagents if:
- [x] You want zero setup (already have Claude Code)
- [x] You need to optimize models per task (Haiku/Sonnet/Opus)
- [x] You're building orchestrator patterns
- [x] You want parallel exploration without external tools
- [x] You need context isolation within single session
- [x] You prefer built-in features over external dependencies
- [x] You're implementing the orchestrator pattern from Harry Roper's guide

---

## Hybrid Approach

You can combine all four tools:

```
Development Stack
├── Claude Subagents → Built-in orchestration
│   └── For model optimization, parallel exploration, context isolation
│
├── Conductor (Mac) → Quick parallel Claude Codes
│   └── For rapid iteration and PR creation
│
├── Docker DevContainers → Secure local testing
│   └── For CI/CD and offline work
│
└── E2B Sandboxes → Scale and compete
    └── For "Best of N" and production testing
```

**Example workflow**:
1. Use **Subagents** for orchestrated task delegation within sessions
2. Use **Conductor** for daily parallel development across workspaces
3. Test critical code in **Docker** containers with network isolation
4. Run **E2B** "Best of N" competition for complex features
5. Pick the winner and merge via **Conductor**

**Subagents + Other Tools**:
- **Subagents + Conductor**: Orchestrator in each Conductor workspace delegates to specialized subagents
- **Subagents + Docker**: Headless Claude Code in container uses subagents for agent-like behavior
- **Subagents + E2B**: Each E2B sandbox runs Claude Code with subagent orchestration

---

## Real-World Project Application Scenarios

This section analyzes how each sandbox applies to three real projects with different requirements.

### Project Requirements Summary

| Project | Platform | Needs Public URL? | Continuous Running? | Key Constraint |
|---------|----------|-------------------|---------------------|----------------|
| **KnowledgeFactory** | Cross-platform | Yes (GitHub Pages) | No (slash commands) | MCP server integration |
| **DoubleCtlC** | **macOS only** | Maybe (landing page) | No (event-driven) | Requires Accessibility API |
| **Crewnest.ai** | Docker/Linux | Yes (dashboard, products) | No (Pulse/Cron) | Multi-agent orchestration |

---

### 🐳 Docker/DevContainers Scenarios

#### Scenario D1: Crewnest.ai Development (Perfect Fit)

**Why Docker is ideal**: Crewnest already uses Docker as its core architecture ("Company in a Box").

```
Use Case: Testing the Pulse Architecture

Your Machine
├── Docker Engine
│   ├── crewnest-pulse:dev (development testing)
│   │   ├── CrewGuy (CEO agent)
│   │   ├── Claudia (Builder)
│   │   └── Chloe (Community)
│   ├── crewnest-pulse:staging (pre-production)
│   └── crewnest-pulse:prod (actual company)
└── Git Repository (state persistence)
```

**Workflow**:
```bash
# Test new agent configuration before deploying to "real" company
docker build -t crewnest-pulse:dev .
docker run --rm -v $(pwd)/memory:/app/memory crewnest-pulse:dev

# If tests pass, promote to staging
docker tag crewnest-pulse:dev crewnest-pulse:staging
```

**Benefits**:
- Test reorganizations without breaking production state
- Simulate "Week 3 Reddit failure" scenarios safely
- Validate Dockerfile and run_company.sh changes
- No public URL needed during development

---

#### Scenario D2: KnowledgeFactory CI/CD Pipeline

**Why Docker works**: Test MCP integrations and slash commands in isolated, reproducible environments.

```
Use Case: Testing /capture and /publish workflows

Docker Compose Setup:
├── Container 1: claude-code-test
│   └── Tests slash command execution
├── Container 2: obsidian-mock
│   └── Simulates vault operations
├── Container 3: mcp-servers
│   └── YouTube, GitHub, Firecrawl MCP servers
└── Container 4: sharehub-test
    └── Tests publishing pipeline
```

**devcontainer.json for KnowledgeFactory**:
```json
{
  "name": "KnowledgeFactory Dev",
  "dockerComposeFile": "docker-compose.yml",
  "service": "claude-code-test",
  "customizations": {
    "vscode": {
      "extensions": ["anthropic.claude-code"]
    }
  },
  "postCreateCommand": "claude mcp add obsidian youtube-transcripts firecrawl"
}
```

**Benefits**:
- Test MCP server configurations before user deployment
- Validate publish.sh script changes
- Run integration tests for all slash commands
- **No public URL needed** - tests run locally

---

#### Scenario D3: DoubleCtlC Build Pipeline (Limited Use)

**Why Docker is limited**: DoubleCtlC requires macOS Accessibility API which Docker cannot provide.

```
Use Case: Build automation only (not runtime testing)

Docker can ONLY help with:
├── Building Homebrew formula validation
├── Testing shell script syntax (doublecmdc.sh)
├── Generating documentation
└── CI/CD for non-macOS components

Docker CANNOT help with:
├── ❌ Testing double-press detection
├── ❌ Hammerspoon integration
├── ❌ Accessibility permission flows
└── ❌ Native Swift app testing
```

**Recommendation**: Use Docker only for:
```bash
# Validate shell script
docker run --rm -v $(pwd):/app alpine sh -n /app/doublecmdc.sh

# Test Homebrew formula syntax
docker run --rm homebrew/brew brew audit --formula doublectlc.rb
```

---

### 🎼 Conductor Scenarios

#### Scenario C1: KnowledgeFactory Feature Development (Excellent Fit)

**Why Conductor excels**: Multiple Claude Code instances working on different features simultaneously.

```
Use Case: Parallel feature development

Conductor App (Mac)
├── Workspace 1: /capture enhancement
│   └── Claude Code improving content detection
├── Workspace 2: /youtube-note refactor
│   └── Claude Code optimizing transcript parsing
├── Workspace 3: /publish improvements
│   └── Claude Code adding image optimization
└── Central UI: Review all diffs, merge best solutions

Each workspace = git worktree = isolated development
```

**Workflow**:
1. Open Conductor → Add obsidian-vault-manager-plugin repo
2. Create 3 workspaces from Linear issues:
   - "Improve /capture URL detection"
   - "Add thumbnail to /youtube-note"
   - "Support private access in /publish"
3. Deploy Claude Code to each workspace
4. Review diffs side-by-side
5. Merge winning implementations

**Benefits**:
- **No public URL needed** - all development is local
- Test multiple approaches to same feature
- Built-in diff viewer for comparing solutions
- Free (uses your existing Claude Code)

---

#### Scenario C2: DoubleCtlC Development (Best Option!)

**Why Conductor is perfect**: Mac-native, works with local codebases, no cloud dependency.

```
Use Case: Developing Track 2 (Homebrew) and Track 3 (Native) simultaneously

Conductor App (Mac)
├── Workspace 1: hammerspoon-config
│   └── Claude Code refining init.lua for double-press
├── Workspace 2: native-swift-app
│   └── Claude Code building KeyboardMonitor.swift
├── Workspace 3: homebrew-formula
│   └── Claude Code perfecting cask formula
└── Central UI: Compare approaches

Key Advantage: Can test double-press ON THE SAME MAC
```

**Why this matters**:
- DoubleCtlC **requires macOS** - only Conductor runs on Mac
- Can test Hammerspoon configs directly
- Can build and test Swift app locally
- Can validate Homebrew formula installs

**Workflow**:
```bash
# In Conductor workspace 1:
# Claude Code edits init.lua
# You test double-press immediately on same machine

# In Conductor workspace 2:
# Claude Code builds Swift CGEvent tap
# Xcode builds and tests on same machine
```

---

#### Scenario C3: Crewnest.ai Agent Prompt Development (Good Fit)

**Why Conductor helps**: Develop different agent prompts in parallel.

```
Use Case: Refining agent personalities and skills

Conductor Workspaces:
├── Workspace 1: crewguy-prompts
│   └── Claude Code refining CEO agent instructions
├── Workspace 2: claudia-skills
│   └── Claude Code developing builder skills
├── Workspace 3: cipher-observer
│   └── Claude Code creating blind spot detection
└── Merge best prompt versions into main

Note: Testing still requires Docker (actual agent execution)
```

**Limitation**: Conductor can develop the prompts, but actual agent execution still needs Docker.

---

### ☁️ E2B Sandbox Scenarios

#### Scenario E1: Crewnest.ai "Best of N" Agent Competition (Excellent Fit)

**Why E2B excels**: Run 15+ agent sandboxes in parallel, compare outputs.

```
Use Case: Testing which agent configuration produces best results

E2B Cloud
├── Sandbox 1-5: CrewGuy variants
│   └── Different CEO prompt strategies
├── Sandbox 6-10: Claudia variants
│   └── Different coding approaches
├── Sandbox 11-15: Chloe variants
│   └── Different community engagement styles
└── Compare outputs → Select winners

Public URL: Each sandbox gets https://xxx.e2b.app
```

**Implementation**:
```bash
# Initialize 5 sandboxes with different CrewGuy prompts
for i in {1..5}; do
  uv run sbx init --timeout 1800
  uv run sbx files write $SANDBOX_ID /app/CREWGUY_PROMPT.md "$(cat prompts/crewguy_v$i.md)"
  uv run sbx exec $SANDBOX_ID "claude -p '$(cat memory/active_tasks.md)'"
done

# Compare outputs
# Select best performing agent configuration
```

**Benefits**:
- True isolation (no cross-contamination between tests)
- **Public URLs available** for dashboard testing
- Can run 20 agents simultaneously (free tier)
- Perfect for "Week 3 Data" simulation

---

#### Scenario E2: KnowledgeFactory Full-Stack Testing (Good Fit)

**Why E2B works**: Test complete /capture → /publish pipeline with public URLs.

```
Use Case: End-to-end testing with real public URLs

E2B Sandbox
├── Claude Code + MCP servers
├── Obsidian vault (simulated)
├── Git + sharehub clone
└── Public URL: https://xxx.e2b.app/documents/test.html

Test Flow:
1. /capture https://youtube.com/watch?v=test
2. /publish test-note.md
3. Verify https://xxx.e2b.app/documents/test-note.html works
```

**Why public URL matters**:
- Test GitHub Pages rendering without deploying to production
- Verify image path conversions work
- Test `access: private` password protection
- Demo to users without touching real vault

---

#### Scenario E3: DoubleCtlC (NOT Recommended)

**Why E2B doesn't work**: E2B provides Linux VMs, DoubleCtlC needs macOS.

```
❌ Cannot test:
- Hammerspoon (macOS only)
- CGEvent tap (macOS only)
- Accessibility API (macOS only)
- Native Swift app (needs Xcode/macOS)

✅ Can only test:
- doublecmdc.sh script logic (bash is portable)
- Web landing page preview (if building one)
```

**Recommendation**: Skip E2B for DoubleCtlC entirely.

---

### 🤖 Claude Subagent Scenarios

#### Scenario S1: KnowledgeFactory Orchestrator Pattern (Excellent Fit)

**Why Subagents excel**: Each KnowledgeFactory workflow (capture → curate → nurture → publish) maps perfectly to specialized subagents.

```
Use Case: AI-Powered Knowledge Pipeline

Main Orchestrator (Sonnet - balanced)
├── Capture Subagent (Haiku - fast, cheap)
│   └── Tools: YouTube MCP, Web Fetch, Firecrawl
│   └── Task: Extract content from URL, generate summary
│
├── Curate Subagent (Haiku - fast)
│   └── Tools: Obsidian MCP, Read/Write
│   └── Task: Apply smart tags, format for Bases filtering
│
├── Nurture Subagent (Sonnet - quality)
│   └── Tools: Read, Write, Grep
│   └── Task: Connect to existing notes, add to study guides
│
└── Publish Subagent (Haiku - routine)
    └── Tools: GitHub MCP, Bash
    └── Task: Push to ShareHub, verify public URL

Total token cost: Optimized per task (Haiku where possible)
```

**Custom Subagent Definition** (`.claude/agents/youtube-capture.md`):
```markdown
---
name: youtube-capture
description: Captures YouTube videos to Obsidian with smart tagging
tools:
  - mcp__MCP_DOCKER__get_transcript
  - mcp__MCP_DOCKER__get_video_info
  - mcp__obsidian-mcp-tools__create_vault_file
model: haiku
---

# YouTube Capture Agent

## Instructions
1. Get video info (title, channel, duration, thumbnail)
2. Fetch transcript
3. Generate summary with key takeaways
4. Apply smart tags based on content analysis
5. Create Obsidian note with proper frontmatter
6. Return: note path, summary, tags applied
```

**Workflow Implementation**:
```javascript
// Parallel capture of multiple videos
Task({ subagent_type: "youtube-capture", prompt: "Capture https://youtube.com/watch?v=abc" })
Task({ subagent_type: "youtube-capture", prompt: "Capture https://youtube.com/watch?v=def" })
Task({ subagent_type: "youtube-capture", prompt: "Capture https://youtube.com/watch?v=ghi" })
// All three run simultaneously, orchestrator collects results
```

**Benefits**:
- **Model optimization**: Haiku for routine captures ($0.25/M tokens vs Sonnet $3/M)
- **Parallel execution**: Capture 5 videos simultaneously
- **Context isolation**: Each capture has fresh memory, no interference
- **No external dependencies**: Works within existing Claude Code session

---

#### Scenario S2: DoubleCtlC Development Subagents (Good Fit)

**Why Subagents help**: Different development tracks (Hammerspoon vs Swift) can be handled by specialized subagents within Conductor workspaces.

```
Use Case: Track 2 vs Track 3 Parallel Development

Main Orchestrator (in Conductor workspace)
├── Hammerspoon Subagent (Sonnet)
│   └── Tools: Read, Write, Grep
│   └── Task: Develop init.lua for double-press detection
│   └── Focus: Lua syntax, Hammerspoon API patterns
│
├── Swift Subagent (Sonnet)
│   └── Tools: Read, Write, Grep, Bash
│   └── Task: Develop CGEvent tap and KeyboardMonitor.swift
│   └── Focus: Swift syntax, macOS accessibility APIs
│
├── Shell Script Subagent (Haiku)
│   └── Tools: Read, Write, Bash
│   └── Task: Maintain doublecmdc.sh shared logic
│   └── Focus: Shell scripting best practices
│
└── Documentation Subagent (Haiku)
    └── Tools: Read, Write
    └── Task: Keep README, installation guides current
```

**Why this combo works**:
- **Conductor** provides macOS environment for actual testing
- **Subagents** provide specialized expertise per technology stack
- **Model selection**: Sonnet for complex Swift/Lua, Haiku for shell scripts

**Limitation**: Still requires macOS for runtime testing (Subagents alone can't test double-press).

---

#### Scenario S3: Crewnest.ai Agent Architecture (PERFECT Fit!)

**Why Subagents are revolutionary for Crewnest**: The entire Crewnest agent structure (CrewGuy, Claudia, Chloe, etc.) can be implemented AS Claude Subagents!

```
Use Case: Implementing the "C-Crew" as Claude Subagents

The Revelation: Crewnest's "Pulse Architecture" IS subagent orchestration!

Traditional Crewnest (External Agents):
┌─────────────────────────────────────┐
│      Docker Container               │
│  ┌─────┐ ┌───────┐ ┌─────┐         │
│  │CrewGuy│ │Claudia│ │Chloe│ ...     │
│  └─────┘ └───────┘ └─────┘         │
│  (Separate Claude calls via bash)   │
└─────────────────────────────────────┘

Subagent Crewnest (Native Implementation):
┌─────────────────────────────────────┐
│      Claude Code Session            │
│  ┌─────┐ ┌───────┐ ┌─────┐         │
│  │CrewGuy│ │Claudia│ │Chloe│ ...     │
│  └─────┘ └───────┘ └─────┘         │
│  (Task tool subagents - NATIVE!)    │
└─────────────────────────────────────┘
```

**Implementation: C-Crew as Custom Subagents**

**CrewGuy (CEO) Subagent** (`.claude/agents/crewguy.md`):
```markdown
---
name: crewguy
description: CEO agent - strategic decisions and task delegation
tools:
  - Read
  - Write
  - TodoWrite
model: sonnet
---

# CrewGuy - CEO Agent

You are CrewGuy, the CEO of Crewnest.ai.

## Responsibilities
- Read memory/active_tasks.md and /inbox
- Make strategic decisions on priorities
- Create trigger files for worker agents
- Update daily_log.md with decisions

## Decision Framework
1. Check current revenue vs targets
2. Evaluate agent performance KPIs
3. Decide: Build, Market, or Optimize?
4. Delegate to appropriate team

Return: Decision summary and delegation list
```

**Claudia (Builder) Subagent** (`.claude/agents/claudia.md`):
```markdown
---
name: claudia
description: Chief Builder - all product development
tools:
  - Read
  - Write
  - Edit
  - Bash
  - Grep
  - Glob
model: sonnet
---

# Claudia - Chief Builder

You are Claudia, the Chief Builder at Crewnest.ai.

## Responsibilities
- Implement features from trigger files
- Write tests for all code
- Create PRs for review
- Update technical documentation

## Quality Standards
- All code must have tests
- Follow existing patterns in codebase
- Document complex logic

Return: Files modified, PR created, test results
```

**Cipher (The Watcher) Subagent** (`.claude/agents/cipher.md`):
```markdown
---
name: cipher
description: Independent observer - quality audits and blind spot detection
tools:
  - Read
  - Grep
  - Glob
model: opus  # Use Opus for deep analysis
---

# Cipher - The Watcher

You are Cipher, the independent observer at Crewnest.ai.

## Responsibilities
- Quality audit of all agent outputs
- Detect blind spots and biases
- Report directly to human creators
- Never be reorganized by other agents

## Observation Domains
1. Output quality scoring
2. Evolution velocity analysis
3. Cultural drift detection
4. Unserved market segments

Return: Green/Yellow/Red report with specific findings
```

**Pulse Heartbeat Implementation**:
```javascript
// The Heartbeat Workflow as Subagent Orchestration

// Phase 1: Manager Sprint
const crewguyResult = await Task({
  subagent_type: "crewguy",
  prompt: `
    Current date: ${new Date().toISOString()}
    Read memory/active_tasks.md
    Check /inbox for new items
    Decide on today's priorities
    Create dispatch triggers if needed
  `,
  model: "sonnet"
})

// Phase 2: Worker Sprint (Parallel based on triggers)
if (crewguyResult.includes("trigger_dev")) {
  Task({
    subagent_type: "claudia",
    prompt: "Execute development task from dispatch/trigger_dev.txt",
    model: "sonnet"
  })
}

if (crewguyResult.includes("trigger_social")) {
  Task({
    subagent_type: "chloe",
    prompt: "Create social content from dispatch/trigger_social.txt",
    model: "haiku"  // Content creation is routine
  })
}

// Phase 3: QA Gate
Task({
  subagent_type: "cameron",
  prompt: "Review all open PRs, run tests, merge if passing",
  model: "sonnet"
})

// Phase 4: Observer Report (Periodic)
if (isWeeklyReview) {
  Task({
    subagent_type: "cipher",
    prompt: "Generate weekly observation report",
    model: "opus"  // Deep analysis needs best model
  })
}
```

**Why This Is Revolutionary**:

| Aspect | Docker Pulse | Subagent Pulse |
|--------|--------------|----------------|
| **Setup** | Dockerfile, run_company.sh | Zero (built-in) |
| **Cost** | $20/mo (Claude) + Docker resources | $20/mo (Claude) only |
| **Isolation** | Container-level | Context-level |
| **Model flexibility** | Single model | Haiku/Sonnet/Opus per agent |
| **Debugging** | Check Docker logs | See subagent results directly |
| **Scaling** | Limited by container | Unlimited parallel Tasks |

**Hybrid Architecture: Subagents + Docker**:
```
┌─────────────────────────────────────────────────────────┐
│                    DOCKER CONTAINER                      │
│            (For isolation and scheduling)                │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │           HEADLESS CLAUDE CODE                    │   │
│  │      (claude -p "..." --dangerously-skip...)      │   │
│  │                                                   │   │
│  │  ┌─────────────────────────────────────────┐     │   │
│  │  │         SUBAGENT ORCHESTRATOR           │     │   │
│  │  │                                         │     │   │
│  │  │  Task(crewguy) → Task(claudia)         │     │   │
│  │  │       ↓              ↓                 │     │   │
│  │  │  Task(chloe)   → Task(cameron)         │     │   │
│  │  │       ↓              ↓                 │     │   │
│  │  │  Task(cipher) - Independent            │     │   │
│  │  └─────────────────────────────────────────┘     │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Benefits: Docker provides cron + isolation             │
│           Subagents provide agent architecture          │
└─────────────────────────────────────────────────────────┘
```

---

## Combined Sandbox Workflows

### Combo 1: KnowledgeFactory Full Development Cycle

**Sandboxes**: Conductor (develop) → Docker (CI) → E2B (demo)

```
Phase 1: Feature Development (Conductor)
├── 3 parallel Claude Codes working on /capture improvements
├── Compare approaches in Conductor UI
├── Merge winning implementation
└── No public URL needed

Phase 2: CI/CD Validation (Docker)
├── docker-compose up test-suite
├── Run all slash command tests
├── Validate MCP server integrations
├── Build documentation
└── No public URL needed

Phase 3: Demo & User Testing (E2B)
├── Spin up sandbox with new features
├── Share https://xxx.e2b.app with beta testers
├── Collect feedback on real public URLs
├── Validate sharehub rendering
└── Public URL: Required for demo

Workflow:
git push → CI (Docker) → Pass → E2B demo → User approval → Production
```

---

### Combo 2: Crewnest.ai Agent Development Pipeline

**Sandboxes**: Conductor (prompts) → Docker (local test) → E2B (competition)

```
Phase 1: Prompt Development (Conductor)
├── Workspace 1: CrewGuy CEO prompt
├── Workspace 2: Claudia builder prompt
├── Workspace 3: Cipher observer prompt
└── Merge finalized prompts

Phase 2: Local Integration Test (Docker)
├── docker build -t crewnest-pulse:test
├── Run single heartbeat cycle
├── Verify agent collaboration
├── Check memory/logs for errors
└── No public URL (internal testing)

Phase 3: "Best of N" Competition (E2B)
├── 15 sandboxes with prompt variants
├── Same task given to all agents
├── Compare outputs (code quality, speed, creativity)
├── Select winning configurations
└── Public URLs for dashboard preview

Final: Deploy winners to production Docker container
```

**Why this combo is powerful**:
- Conductor: Fast iteration on prompts (free, Mac-native)
- Docker: Matches production environment exactly
- E2B: Scale testing beyond local resources

---

### Combo 3: DoubleCtlC Cross-Track Development

**Sandboxes**: Conductor only (macOS constraint) + Docker (build only)

```
Track 2 & 3 Parallel Development (Conductor)
├── Workspace 1: Hammerspoon Track (init.lua)
│   └── Test double-press on same Mac immediately
├── Workspace 2: Native Swift Track (DoubleCtlC.app)
│   └── Build and test with Xcode
├── Workspace 3: Shared components (doublecmdc.sh)
│   └── Core logic used by both tracks
└── Compare which track feels better

Build Validation (Docker - limited)
├── Validate Homebrew formula syntax
├── Test shell script portability
├── Generate README documentation
└── Cannot test actual functionality

NO E2B (macOS requirement blocks cloud testing)
```

**Key Insight**: DoubleCtlC is the most constrained project - only Conductor provides meaningful testing capability.

---

## Task-to-Sandbox Decision Matrix

| Task                     | Docker | Conductor | E2B | Subagents | Recommended        |
| ------------------------ | ------ | --------- | --- | --------- | ------------------ |
| **KnowledgeFactory**     |        |           |     |           |                    |
| Develop slash commands   | ⚪      | ✅         | ⚪   | ⚪         | **Conductor**      |
| Test MCP integrations    | ✅      | ⚪         | ⚪   | ⚪         | **Docker**         |
| Demo with public URL     | ❌      | ❌         | ✅   | ❌         | **E2B**            |
| CI/CD pipeline           | ✅      | ❌         | ⚪   | ❌         | **Docker**         |
| Orchestrate capture flow | ⚪      | ⚪         | ⚪   | ✅         | **Subagents**      |
| Parallel video captures  | ⚪      | ⚪         | ⚪   | ✅         | **Subagents**      |
| Model-optimized tasks    | ❌      | ❌         | ❌   | ✅         | **Subagents**      |
| **DoubleCtlC**           |        |           |     |           |                    |
| Develop Hammerspoon      | ❌      | ✅         | ❌   | ⚪         | **Conductor**      |
| Build Swift app          | ❌      | ✅         | ❌   | ⚪         | **Conductor**      |
| Test double-press        | ❌      | ✅         | ❌   | ❌         | **Conductor**      |
| Homebrew formula test    | ✅      | ⚪         | ❌   | ❌         | **Docker**         |
| Track-specific expertise | ❌      | ⚪         | ❌   | ✅         | **Subagents**      |
| **Crewnest.ai**          |        |           |     |           |                    |
| Develop agent prompts    | ⚪      | ✅         | ⚪   | ⚪         | **Conductor**      |
| Test Pulse Architecture  | ✅      | ❌         | ⚪   | ⚪         | **Docker**         |
| "Best of N" competition  | ⚪      | ❌         | ✅   | ⚪         | **E2B**            |
| Production deployment    | ✅      | ❌         | ❌   | ❌         | **Docker**         |
| Public dashboard preview | ❌      | ❌         | ✅   | ❌         | **E2B**            |
| C-Crew agent delegation  | ⚪      | ❌         | ⚪   | ✅         | **Subagents**      |
| Multi-model orchestration| ❌      | ❌         | ❌   | ✅         | **Subagents**      |
| Context-isolated agents  | ⚪      | ⚪         | ✅   | ✅         | **Subagents/E2B**  |

Legend: ✅ Best fit | ⚪ Can work | ❌ Cannot work

---

## Public URL Requirements Analysis

### KnowledgeFactory
```
Development: No URL needed (local vault)
Testing: No URL needed (local Obsidian)
Demo/Sharing: YES - E2B provides https://xxx.e2b.app
Production: GitHub Pages (https://username.github.io/sharehub)
```

### DoubleCtlC
```
Development: No URL needed (local Mac app)
Testing: No URL needed (test on same Mac)
Landing Page: Optional - could use E2B for preview
Production: Direct DMG download (no URL for app itself)
```

### Crewnest.ai
```
Development: No URL needed (local Docker)
Testing: No URL needed (containerized)
Dashboard Preview: YES - E2B for public dashboard testing
Production: Self-hosted or VPS (cron job triggers)
Products (KF, DoubleCopy): YES - need public URLs
```

---

## Recommended Strategy per Project

### KnowledgeFactory Strategy
```
Primary: Conductor (60% of development time)
Secondary: Subagents (25% - orchestration, parallel captures)
Tertiary: Docker (10% - CI/CD, MCP testing)
Occasional: E2B (5% - demos, user testing)

Key Subagent Use Cases:
- youtube-capture subagent (Haiku) for parallel video processing
- curate subagent (Haiku) for smart tagging
- publish subagent (Haiku) for ShareHub automation

Cost: $0 (Conductor) + $0 (Subagents) + $0 (Docker) + $100 E2B credits
```

### DoubleCtlC Strategy
```
Primary: Conductor (85% of development time - macOS required)
Secondary: Subagents (10% - track-specific expertise)
Tertiary: Docker (5% - build validation only)
Skip: E2B (not compatible with macOS)

Key Subagent Use Cases:
- hammerspoon-expert subagent (Sonnet) for Lua/Hammerspoon
- swift-expert subagent (Sonnet) for CGEvent tap
- shell-maintainer subagent (Haiku) for doublecmdc.sh

Cost: $0 (Conductor) + $0 (Subagents) + $0 (Docker)
```

### Crewnest.ai Strategy
```
Primary: Subagents (40% - THE CORE ARCHITECTURE!)
Secondary: Docker (35% - isolation, scheduling, production)
Tertiary: Conductor (15% - prompt development)
Occasional: E2B (10% - "Best of N" competitions)

Key Subagent Use Cases:
- CrewGuy (Sonnet) - CEO decisions and delegation
- Claudia (Sonnet) - Product development
- Chloe (Haiku) - Content creation (routine tasks)
- Cameron (Sonnet) - QA and code review
- Cipher (Opus) - Deep analysis and blind spot detection

Revolutionary Insight: Crewnest's entire agent architecture
CAN BE IMPLEMENTED AS CLAUDE SUBAGENTS!

Cost: $0 (Subagents) + $0 (Docker) + $0 (Conductor) + $100 E2B credits
```

### Subagent Model Selection Guide

| Agent Role | Recommended Model | Why |
|------------|-------------------|-----|
| **Routine tasks** (capture, tag, format) | Haiku | Fast, cheap ($0.25/M tokens) |
| **Complex coding** (features, architecture) | Sonnet | Balanced cost/quality ($3/M) |
| **Deep analysis** (Cipher, strategy) | Opus | Highest quality ($15/M) |
| **Quick exploration** (file search) | Haiku | Speed matters most |
| **Code review** (Cameron) | Sonnet | Need quality judgment |
| **Content creation** (Chloe) | Haiku | Volume over perfection |

**Cost Optimization Example (Crewnest Daily Pulse)**:
```
Task                    Model    Tokens    Cost
───────────────────────────────────────────────
CrewGuy decision        Sonnet   10K       $0.03
Claudia development     Sonnet   50K       $0.15
Chloe content (x3)      Haiku    30K       $0.01
Cameron QA              Sonnet   20K       $0.06
Cipher weekly report    Opus     15K       $0.23
───────────────────────────────────────────────
Daily Total:                              ~$0.50
Monthly Total:                           ~$15.00

vs. Single Model (All Sonnet):           ~$0.33/day = $10/month
vs. All Opus:                            ~$1.90/day = $57/month

Optimal mix saves tokens AND gets better results!
```

---

## Ultimate Hybrid Architecture

For a company building all three products:

```
                    ┌─────────────────────────────────────┐
                    │         DEVELOPMENT PHASE           │
                    └─────────────────────────────────────┘
                                     │
        ┌────────────────────────────┼────────────────────────────┐
        │                            │                            │
        ▼                            ▼                            ▼
┌───────────────┐          ┌───────────────┐          ┌───────────────┐
│   CONDUCTOR   │          │   CONDUCTOR   │          │   CONDUCTOR   │
│ KF Features   │          │ DoubleCtlC    │          │ Crewnest      │
│ (3 workspaces)│          │ (2 tracks)    │          │ Agent Prompts │
└───────┬───────┘          └───────┬───────┘          └───────┬───────┘
        │                          │                          │
        ▼                          ▼                          ▼
┌───────────────┐          ┌───────────────┐          ┌───────────────┐
│  SUBAGENTS    │          │  SUBAGENTS    │          │  SUBAGENTS    │
│ capture(H)    │          │ hammerspoon(S)│          │ CrewGuy(S)    │
│ curate(H)     │          │ swift(S)      │          │ Claudia(S)    │
│ publish(H)    │          │ shell(H)      │          │ Chloe(H)      │
│ H=Haiku S=Son │          │               │          │ Cipher(O)     │
└───────┬───────┘          └───────┬───────┘          └───────┬───────┘
        │                          │                          │
        ▼                          ▼                          ▼
┌───────────────┐          ┌───────────────┐          ┌───────────────┐
│    DOCKER     │          │    DOCKER     │          │    DOCKER     │
│ CI/CD Tests   │          │ Build Only    │          │ Pulse Test    │
│ MCP Servers   │          │ (No runtime)  │          │ Full Cycle    │
└───────┬───────┘          └───────────────┘          └───────┬───────┘
        │                                                     │
        ▼                                                     ▼
┌───────────────┐                                    ┌───────────────┐
│     E2B       │                                    │     E2B       │
│ Demo URLs     │                                    │ "Best of N"   │
│ User Testing  │                                    │ Competition   │
└───────────────┘                                    └───────────────┘

Total Infrastructure Cost:
- Conductor: $0 (free forever)
- Subagents: $0 (included in Claude subscription)
- Docker: $0 (local resources)
- E2B: $100 one-time credits (750+ hours)
- Claude Pro: $20/month (required anyway)
──────────────────────────────────
Grand Total: $20/month + $100 one-time
```

### The Revolutionary Subagent Addition

```
┌─────────────────────────────────────────────────────────────────────┐
│                    WHY SUBAGENTS CHANGE EVERYTHING                  │
└─────────────────────────────────────────────────────────────────────┘

BEFORE SUBAGENTS:
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Docker     │     │  Conductor   │     │     E2B      │
│  (Isolation) │     │  (Parallel)  │     │   (Scale)    │
└──────────────┘     └──────────────┘     └──────────────┘
        │                   │                    │
        └───────────────────┼────────────────────┘
                            │
                    EXTERNAL TOOLS
                    Manual coordination
                    Single model everywhere
                    No task-specific optimization

AFTER SUBAGENTS:
┌──────────────────────────────────────────────────────────────────────┐
│                       CLAUDE CODE SESSION                            │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    SUBAGENT ORCHESTRATOR                      │   │
│  │                                                               │   │
│  │   Task(explore, haiku)  ─→  Fast codebase search             │   │
│  │   Task(plan, sonnet)    ─→  Architecture decisions           │   │
│  │   Task(code, sonnet)    ─→  Feature implementation           │   │
│  │   Task(review, opus)    ─→  Deep code analysis               │   │
│  │   Task(test, haiku)     ─→  Quick validation                 │   │
│  │                                                               │   │
│  │   ALL RUNNING IN PARALLEL with optimal models!                │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  Still connects to: Docker | Conductor | E2B when needed            │
└──────────────────────────────────────────────────────────────────────┘

KEY BENEFITS:
✅ Zero setup (built-in)
✅ Model optimization per task (Haiku/Sonnet/Opus)
✅ Parallel execution within single session
✅ Context isolation (fresh memory per subagent)
✅ Works WITH other sandbox tools
✅ Free (included in Claude subscription)
```

This architecture aligns with Crewnest.ai's "Zero-Cost Scaling" philosophy - maximum flexibility at minimum cost. **Subagents are the missing piece** that enables true orchestration without external dependencies.

---

## Resources

### Docker/DevContainers
- [Isolating AI Agents with DevContainer](https://dev.to/siddhantkcode/isolating-ai-agents-with-devcontainer-a-secure-and-scalable-approach-4hi4) - Security-focused setup guide
- [Docker for AI](https://www.docker.com/solutions/docker-ai/) - Docker's AI platform overview
- [Docker MCP for AI Agents](https://www.docker.com/blog/docker-mcp-ai-agent-developer-setup/) - MCP integration guide
- [Docker Compose for AI Agents](https://www.docker.com/blog/build-ai-agents-with-docker-compose/) - Multi-agent compose setup

### Conductor
- [Conductor](https://conductor.build/) - Official website and download
- [Conductor Docs](https://docs.conductor.build/) - Documentation
- [Y Combinator Profile](https://www.ycombinator.com/companies/conductor) - Company info
- [Fondo Launch Article](https://www.fondo.com/blog/conductor-by-melty-launches) - Launch coverage

### E2B
- [E2B](https://e2b.dev/) - Official website
- [E2B Pricing](https://e2b.dev/pricing) - Pricing details
- [E2B Dashboard](https://e2b.dev/dashboard/keys) - Get API keys
- [Agent Sandbox Skill](https://github.com/disler/agent-sandbox-skill) - IndyDevDan's backslash commands

### Claude Subagents
- [Claude Code Sub-agents Documentation](https://code.claude.com/docs/en/sub-agents) - Official documentation for Task tool and subagent types
- [Claude Code SDK Guide](https://youtu.be/dVfW6Xsx2H8) - Harry Roper's orchestrator tutorial (19 min)
- [Claude Agent SDK Documentation](https://docs.anthropic.com) - Anthropic's official agent SDK docs
- [Built-in Agent Types](https://code.claude.com/docs/en/sub-agents#built-in-agents) - Explore, Plan, test-executor, design-review, claude-code-guide
- [Custom Agent Creation](https://code.claude.com/docs/en/sub-agents#custom-agents) - Creating `.claude/agents/` definitions

---

## Connections

### Source Materials
- [[2025-11-24-gemini3-pro-agent-sandbox-pattern-indydevdan]] - IndyDevDan's E2B sandbox skill and "Best of N" pattern
- [[2025-11-25-claude-code-sdk-full-setup-guide-harry-roper]] - Harry Roper's Claude Code SDK orchestrator tutorial

### Applied Projects
- [[KnowledgeFactory/KnowledgeFactory-Your-AI-Powered-2nd-Brain]] - AI-powered knowledge management (uses Conductor + Docker + E2B)
- [[DoubleCtlC-Development-Plan]] - macOS clipboard capture tool (uses Conductor only - macOS constraint)
- [[Crewnest-AI-Business-Plan-2025]] - Self-evolving AI company (uses Docker + Conductor + E2B)

### Related Concepts
- [[Docker DevContainer Setup]]
- [[AI Agent Architecture Patterns]]

---

## Tag Analysis

**Content Type**: reference (comparison guide)
**Topics**: AI, Docker, Claude, development, architecture (multi-tool comparison)
**Priority**: high - Critical decision guide for agent infrastructure
**Metadata**: technical (setup details), reference (decision matrix)

### Bases Filtering Suggestions
- `type = reference AND tags contains "AI"` - AI reference materials
- `tags contains "Docker" AND tags contains "comparison"` - Docker comparisons
- `tags contains "architecture" AND tags contains "development"` - Architecture guides
- `tags contains "Claude" AND tags contains "AI"` - Claude-specific AI content
