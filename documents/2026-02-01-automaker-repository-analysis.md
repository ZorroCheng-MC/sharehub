---
date: 2026-02-01
title: "Automaker: Autonomous AI Development Studio"
tags:
  - repository
  - AI
  - Claude
  - coding
  - automation
  - development
  - tools
  - TypeScript
  - React
  - Electron
  - technical
  - deep-dive
url: https://github.com/AutoMaker-Org/automaker
date: 2026-02-01
type: repository
status: inbox
priority: high
owner: AutoMaker-Org
visibility: public
license: Automaker License Agreement
read: false
---

# [Automaker](https://github.com/AutoMaker-Org/automaker)

> **Stop typing code. Start directing AI agents.**

## 📖 Repository Overview

Automaker is an **autonomous AI development studio** that transforms how you build software. Instead of manually writing every line of code, you describe features on a Kanban board and watch as AI agents powered by **Claude Agent SDK** automatically implement them.

**Key Value Proposition**: Build entire applications in days, not weeks. Developers become architects directing AI agents rather than manual coders.

## 🎯 What Makes It Different

| Traditional Tools | Automaker |
|-------------------|-----------|
| Help you write code | Help you orchestrate AI agents |
| Manual coding | Agentic coding |
| Developer writes | AI implements |
| Task-focused | Project-wide |

### The Workflow

1. **Add Features** - Describe features with text, images, or screenshots
2. **Move to "In Progress"** - Automaker assigns an AI agent automatically
3. **Watch It Build** - Real-time progress as agent writes code, runs tests
4. **Review & Verify** - Review changes, run tests, approve when ready
5. **Ship Faster** - 10x faster development

## 📁 Repository Structure

```
automaker/
├── apps/
│   ├── ui/              # React + Vite + Electron frontend
│   └── server/          # Express + WebSocket backend
├── libs/                # Shared packages (monorepo)
│   ├── types/           # Core TypeScript definitions
│   ├── utils/           # Logging, errors, utilities
│   ├── prompts/         # AI prompt templates
│   ├── platform/        # Path management, security
│   ├── model-resolver/  # Claude model aliasing
│   ├── dependency-resolver/  # Feature dependency ordering
│   ├── git-utils/       # Git operations & worktree management
│   └── spec-parser/     # Spec parsing
├── docs/                # Documentation
├── scripts/             # Build and utility scripts
├── test/                # Tests
├── .claude/             # Claude Code configuration
├── Dockerfile           # Docker deployment
├── docker-compose.yml   # Docker orchestration
└── package.json         # v0.13.0
```

## 🛠️ Technical Stack

### Frontend
- **React 19** - UI framework
- **Vite 7** - Build tool
- **Electron 39** - Desktop app
- **TypeScript 5.9** - Type safety
- **TanStack Router** - File-based routing
- **Zustand 5** - State management
- **Tailwind CSS 4** - Styling (25+ themes)
- **Radix UI** - Accessible components
- **dnd-kit** - Drag and drop (Kanban)
- **@xyflow/react** - Graph visualization
- **xterm.js** - Terminal emulator
- **CodeMirror 6** - Code editor

### Backend
- **Node.js 22+** - Runtime
- **Express 5** - HTTP server
- **Claude Agent SDK** - AI agent integration
- **WebSocket (ws)** - Real-time streaming
- **node-pty** - Terminal sessions

### Testing & Quality
- **Playwright** - E2E testing
- **Vitest** - Unit testing
- **ESLint 9** - Linting
- **Prettier 3** - Formatting
- **Husky** - Git hooks

## ✨ Key Features

### Core Workflow
- 📋 **Kanban Board** - Drag-and-drop feature management
- 🤖 **AI Agent Integration** - Auto-assignment to implement features
- 🔀 **Git Worktree Isolation** - Protects main branch
- 📡 **Real-time Streaming** - Watch agents work live
- 🔄 **Follow-up Instructions** - Send instructions to running agents

### AI & Planning
- 🧠 **Multi-Model Support** - Opus, Sonnet, Haiku per feature
- 💭 **Extended Thinking** - None, medium, deep, ultra modes
- 📝 **Planning Modes** - Skip, lite, spec, full
- ✅ **Plan Approval** - Review before implementation
- 📊 **Multi-Agent Tasks** - Spec mode spawns dedicated agents

### Project Management
- 🔍 **Project Analysis** - AI-powered codebase understanding
- 💡 **Feature Suggestions** - AI-generated based on analysis
- 📁 **Context Management** - Add docs agents auto-reference
- 🔗 **Dependency Blocking** - Enforce execution order
- 🌳 **Graph View** - Visualize dependencies
- 📋 **GitHub Integration** - Import issues, validate, convert

### Views Available
| View | Shortcut | Description |
|------|----------|-------------|
| Board | `K` | Kanban workflow |
| Agent | `A` | Chat with AI |
| Spec | `D` | Project specification |
| Context | `C` | Manage context files |
| Settings | `S` | Configure everything |
| Terminal | `T` | Integrated terminal |
| Graph | `H` | Dependency visualization |
| Ideation | `I` | Brainstorm with AI |
| Memory | `Y` | Agent memory |
| GitHub Issues | `G` | Import issues |
| GitHub PRs | `R` | Manage PRs |

## 🚀 Quick Start

```bash
# Prerequisites: Node.js 22+, Claude Code CLI authenticated

# Clone and install
git clone https://github.com/AutoMaker-Org/automaker.git
cd automaker
npm install

# Start (choose web or desktop)
npm run dev
```

**Authentication**: Uses your authenticated Claude Code CLI credentials automatically.

## 🔐 Security Model

- **Git Worktrees** - Isolated execution per feature
- **Path Sandboxing** - Optional `ALLOWED_ROOT_DIRECTORY`
- **Docker Isolation** - Recommended deployment
- **Plan Approval** - Review before implementation

**Warning**: This software has full OS access. Recommended to run in Docker or VM.

## 📊 Data Storage

Per-project in `{projectPath}/.automaker/`:
- `features/` - Feature JSON and images
- `context/` - Context files for agents
- `worktrees/` - Git worktree metadata
- `settings.json` - Project settings
- `app_spec.txt` - Project specification

## 🔗 Related Links

- **Discord**: [Agentic Jumpstart](https://discord.gg/jjem7aEDKU)
- **Course**: [Agentic Jumpstart](https://agenticjumpstart.com/?utm=automaker-gh)
- **Claude Agent SDK**: [@anthropic-ai/claude-agent-sdk](https://www.npmjs.com/package/@anthropic-ai/claude-agent-sdk)

## 📺 Video Overview

[![Automaker Video](https://img.youtube.com/vi/MHWX4OF-HiM/maxresdefault.jpg)](https://youtu.be/MHWX4OF-HiM)

**[Automaker: This AI Coding Agent STUDIO is KINDA COOL!](https://youtu.be/MHWX4OF-HiM)**
- **Channel**: AICodeKing
- **Duration**: 8 minutes
- **Published**: 2026-01-09

### Video Key Takeaways

- 🤖 Fully autonomous AI studio that plans, creates tasks, and writes code for you
- 🖥️ Native Electron desktop app for dedicated environment
- ⚡ **Auto Mode** - Agents pick up tasks from Kanban and execute without intervention
- 📝 **App Spec** - Turn a text prompt into full project roadmap in seconds
- 🧠 Upload custom context files (API docs, coding standards) for agent guidance
- 🔒 Runs locally with your own API keys - full control over code and privacy
- 🔄 Shifts workflow from "developer drives AI" to "product management while AI drives code"

### Demo in Video
- Builds a **Movie Tracker** app from scratch using Auto Mode
- Compares to other AI coding tools

## 📜 License

**Automaker License Agreement** - Custom license:
- ✅ **Allowed**: Build anything, internal use, modify for internal use
- ❌ **Restricted**: No resale, no SaaS hosting, no monetizing mods
- ⚠️ **Liability**: Use at own risk, AI can break things

## 🏷️ Tags Analysis

**Content Analysis:**
- **Type**: `repository` (GitHub project)
- **Topics**: AI agents, autonomous coding, Claude SDK, desktop app
- **Complexity**: Advanced (full-stack monorepo, Electron, real-time)
- **Priority**: High - practical tool for agentic coding

**Why These Tags:**
- `AI`, `Claude` - Core AI integration with Claude Agent SDK
- `automation` - Autonomous agent execution
- `coding`, `development` - Developer tooling focus
- `TypeScript`, `React`, `Electron` - Primary tech stack
- `tools` - Practical development tool

**Suggested Bases Filters:**
- Find similar: `type = repository AND tags contains "Claude"`
- Find AI tools: `tags contains "AI" AND tags contains "automation"`
- Find by stack: `tags contains "TypeScript" AND tags contains "React"`

## 🔍 Related Searches

- `/semantic-search "Claude Agent SDK agentic coding"`
- `/semantic-search "AI coding agents autonomous"`
- `/semantic-search "Kanban AI development workflow"`

---

**Captured**: 2026-02-01
**Source**: https://github.com/AutoMaker-Org/automaker
**Version**: 0.13.0
