---
title: "Claw Code Framework — Open-Source AI Coding Agent"
date: 2026-04-02
---
# Claw Code Framework — Open-Source AI Coding Agent

**Date:** 2026-04-01  
**Source:** https://claw-code.codes/  
**Status:** Open source, Production-ready

---

## What Is Claw Code?

**Claw Code** is an open-source AI coding agent framework — a clean-room rewrite of the Claude Code agent harness architecture, built from scratch in **Python (27.1%)** and **Rust (72.9%)**.

- **48k+ GitHub stars** (one of the fastest projects in GitHub history to reach 30k stars)
- **56k+ forks**, 335 watchers, 512k+ lines analyzed
- **No proprietary code** — independent reimplementation, legally vetted
- **Multi-LLM support** — Claude, OpenAI, local models (not locked to one provider)

---

## Origin Story: The March 2026 Source Leak

### The Leak
- **Date:** March 31, 2026
- **Discoverer:** Chaofan Shou (@shoucccc), security researcher
- **What:** Complete Claude Code source code accidentally published in npm v2.1.88
- **Size:** 59.8 MB JavaScript source map (.map file)
- **Exposure:** ~512,000 lines of TypeScript, 1,906 source files, full internal architecture

### The Response
- **Creator:** Sigrid Jin (@sigridjineth), Wall Street Journal profiled as one of the world's most active Claude Code power users (25 billion tokens consumed in past year)
- **Approach:** Clean-room rewrite using Python, orchestrated through oh-my-codex (OmX)
- **Result:** Claw Code achieved 30,000 GitHub stars within hours of publication

### Supply Chain Warning ⚠️
- During the leak window (00:21–03:29 UTC on March 31, 2026), a supply-chain attack distributed a malicious version of axios
- **Affected versions:** axios 1.14.1, 0.30.4 (contained a remote access trojan)
- **Impact:** npm installs during this window may have been compromised
- **Mitigation:** Anthropic migrated to native installer (`curl -fsSL https://claude.ai/install.sh | bash`)

---

## Core Architecture

### Directory Structure
```
claw-code/
├── src/                    # Python workspace
│   ├── commands.py        # CLI command registry & dispatch
│   ├── tools.py           # Plugin-based tool system (~40 tools)
│   ├── models.py          # LLM provider abstraction
│   ├── query_engine.py    # Core query engine (LLM calls, streaming, caching)
│   ├── task.py            # Task management & agent lifecycle
│   └── main.py            # Entry point
├── rust/                  # Rust core (performance-critical paths)
│   └── ...               # 6-crate workspace, high-performance runtime
└── tests/                # Integration tests
```

### Key Architectural Components

#### 1. **Tool System** (19 Built-in, Permission-Gated Tools)
A plugin-based architecture with granular permission controls:
- File reading/writing
- Bash execution
- Web scraping
- LSP integration
- Git operations
- Web fetch & search
- Agent spawning (sub-agents/"swarms")

Each tool is independently sandboxed with configurable access controls and full JSON schema definitions.

#### 2. **Query Engine**
Central intelligence managing:
- LLM API calls and streaming
- Response caching strategies
- Multi-step orchestration
- Provider-agnostic design
- Configurable turn limits & budget controls

#### 3. **Multi-Agent Orchestration**
- Spawn sub-agents ("swarms") to parallelize complex tasks
- Isolated execution contexts with shared memory
- Full subagent lifecycle control via the Agent tool
- Autonomous task decomposition

#### 4. **MCP Integration**
Full Model Context Protocol support with **6 transport types:**
- Stdio
- SSE (Server-Sent Events)
- HTTP
- WebSocket
- SDK
- ClaudeAiProxy

Features automatic name normalization, config hashing, and OAuth authentication.

#### 5. **LLM API Client**
Provider-agnostic implementation:
- Automatic retry logic
- SSE streaming support
- OAuth authentication
- Token usage tracking & cost estimation
- Multi-provider support (Claude, OpenAI, local models)

#### 6. **Memory & Session Management**
Multi-layer system including:
- Session persistence across conversations
- Transcript compaction for context management
- Context discovery mechanisms
- Append-only audit logs
- Automatic cleanup

---

## Core Capabilities

### 🔧 Plugin-Based Tool System
19 permission-gated tools covering:
- File I/O, shell execution, Git operations
- Web scraping, notebook editing
- Each independently sandboxed with configurable access

### 🧠 Autonomous Agent Loop
- Terminal-native operation
- Reads entire codebase
- Edits files, executes commands, runs tests
- Handles Git workflows
- Iterates autonomously until task completion

### 🌐 15 Slash Commands
Interactive command system:
- `/compact` — transcript compression
- `/model` — switch LLM provider
- `/permissions` — granular access control
- `/cost` — budget tracking
- `/session` — resume support
- Command graph categorization
- REPL integration

### 💾 Session & Memory
- Multi-layer memory system
- Session persistence
- Transcript compaction
- Context discovery
- Persistent knowledge across conversations

### ⚡ Rust Performance Core
- 6-crate workspace with 16 runtime modules
- Performance-critical paths
- Zero-dependency JSON parser
- OAuth PKCE flow
- Syntax-highlighted terminal rendering

### 🔌 Multi-Provider LLM Support
- Not locked to Claude
- OpenAI support
- Local model integration
- Automatic API client abstraction

---

## Advanced Features Revealed by Source Leak

### KAIROS Mode
Continuously-running proactive assistant mode. Rather than waiting for user input, actively observes the development environment and takes autonomous action. Backed by independent append-only log system.

### ULTRAPLAN Mode
Offloads complex planning to remote cloud container running more capable models (Opus-class). Allocates up to 30 minutes of dedicated reasoning with browser-based approval workflows for human oversight.

### autoDream Service
Background memory consolidation engine running as forked sub-agent process. Performs idle-time analysis:
- Reorganizes learned patterns
- Prunes stale context
- Strengthens relevant memory associations
- Continuous optimization without user interaction

### Feature Flags
44 compiled feature flags discovered (20 remain disabled for external users). Indicates Anthropic's internal feature pipeline is substantially ahead of public releases.

---

## Permission System

**Three Permission Modes:**
1. **Whitelist** — only explicitly allowed tools
2. **Blacklist** — all except denied tools
3. **Interactive** — prompt user per-action

Per-tool policy engine with:
- Deny lists for sensitive operations
- Interactive prompting workflows
- Granular permission boundaries

---

## Claw Code vs Claude Code

| Aspect | Claude Code (Anthropic) | Claw Code (Open Source) |
|--------|----------------------|----------------------|
| **Type** | Official proprietary CLI agent | Open-source clean-room rewrite |
| **Language** | TypeScript | Python + Rust |
| **Access** | Terminal, VS Code, Web (requires Claude subscription) | Terminal (supports multiple LLM providers) |
| **Cost** | Requires Claude Pro/Max or Enterprise | Free and open source |
| **Architecture** | Monolithic TS bundle with source maps | Modular Python + Rust core |
| **Tool System** | ~40 built-in tools, 29k lines | Plugin-based, extensible architecture |
| **LLM Support** | Claude models only | Provider-agnostic (Claude, OpenAI, local) |
| **Extensibility** | Closed | Open for contributions |

---

## Getting Started

### Installation
```bash
git clone https://github.com/instructkr/claw-code.git
cd claw-code
pip install -r requirements.txt
python src/main.py
```

### Environment Variables
- Standard Python package management
- Multi-LLM provider configuration
- API key setup for chosen providers

---

## Related Ecosystem Projects

### Official & Mirrors
- **Kuberwastaken/claude-code** — Earliest GitHub mirror + Rust rewrite
- **Ringmast4r/claw-cli-source** — Source snapshot archive (v2.1.88)

### Tools & Clients
- **raullenchai/claw** — Mobile remote-control interface (tmux-based)
- **jamesrochabrun/Claw** — Native macOS desktop GUI client

### Experimental Frameworks
- **GreenSheep01201/claw-empire** — Multi-agent virtual company simulator
- **0xKarl-dev/claw-codes** — Python/Rust hybrid agent runtime framework

---

## Repository Stats

| Metric | Value |
|--------|-------|
| **Stars** | 48,000+ |
| **Forks** | 56,000+ |
| **Watchers** | 335 |
| **Open Issues** | 2,100+ |
| **Lines Analyzed** | 512,000+ |
| **Primary Language** | Python (27.1%), Rust (72.9%) |

---

## Key Takeaways

1. **Clean-Room Implementation** — No Anthropic proprietary code; legally independent architecture-based rewrite
2. **Production Ready** — 48k+ stars, active community, battle-tested by power users
3. **Provider Agnostic** — Works with Claude, OpenAI, local models (unlike Claude Code's locked ecosystem)
4. **Modular & Extensible** — Plugin-based tool system, easy to customize for your use case
5. **Performance-Focused** — Rust core for critical paths, Python for agent orchestration
6. **Self-Hosted** — Full control over deployment, no vendor lock-in
7. **Active Development** — Ongoing Rust migration, community contributions welcome

---

## Links & Resources

- **Official Repository:** https://github.com/instructkr/claw-code
- **Project Website:** https://claw-code.codes/
- **Creator:** Sigrid Jin (@sigridjineth)

---

*Captured April 1, 2026 via kf-claude:capture*
