---
title: "Gemini CLI vs Claude Code Feature Parity Research"
date: 2025-12-30
tags:
  - idea
  - AI
  - Claude
  - tools
  - development
  - actionable
  - research
  - architecture
status: inbox
priority: high
---

# Gemini CLI vs Claude Code Feature Parity Research

## Core Idea

Investigate and document how closely Gemini CLI capabilities align with Claude Code's extensibility ecosystem, specifically examining:

- **Skills** - Custom capability packages
- **Commands** - Slash command definitions
- **MCP (Model Context Protocol)** - Tool integration layer
- **Templates** - Reusable content scaffolds
- **Scripts** - Automation helpers

The goal is to create portable, cross-compatible tooling that could be distributed through a plugin marketplace.

## Why This Matters

1. **Reduce vendor lock-in** - Skills/commands that work across AI CLI tools provide flexibility
2. **Maximize investment** - Time spent building Claude Code extensions could benefit Gemini users too
3. **Community benefit** - A unified plugin marketplace could accelerate AI-assisted development adoption
4. **Future-proofing** - As AI CLI tools evolve, understanding common patterns helps adaptation

## Research Questions

- [ ] Does Gemini CLI have an equivalent to Claude Code's `~/.claude/commands/` structure?
- [ ] How does Gemini handle MCP server integration?
- [ ] What's Gemini CLI's approach to skills/plugins?
- [ ] Are there template or scaffolding mechanisms in Gemini CLI?
- [ ] What scripting hooks exist in Gemini CLI?

## Feature Comparison Matrix (To Research)

| Feature | Claude Code | Gemini CLI | Portable? |
|---------|-------------|------------|-----------|
| Custom Commands | `.claude/commands/*.md` | ? | TBD |
| Skills | `~/.claude/skills/` | ? | TBD |
| MCP Integration | Native support | ? | TBD |
| Templates | Skill templates | ? | TBD |
| Scripts | Bash/Python helpers | ? | TBD |

## Potential Marketplace Concept

If feature parity exists, a unified marketplace could offer:
- Cross-platform skill packages
- Adapter layers for tool-specific differences
- Shared MCP server configurations
- Universal template libraries

## Related Concepts

- Claude Code Skills Architecture
- Gemini CLI Extensions
- MCP Protocol Specification
- AI Tool Interoperability
- Plugin Marketplace Design (npm, VS Code extensions model)

## Next Steps

1. Deep-dive into Gemini CLI documentation for extension capabilities
2. Compare MCP support between tools
3. Prototype a simple cross-compatible command
4. Research existing marketplace models (Raycast, VS Code, npm)
5. Draft specification for portable AI CLI plugin format

---

## Portability Test: obsidian-vault-manager-plugin

Testing whether the [obsidian-vault-manager-plugin](https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin) can be ported to Gemini CLI.

### Plugin Components to Test

| Component | Claude Code Location | Gemini CLI Equivalent | Test Status |
|-----------|---------------------|----------------------|-------------|
| Slash Commands | `.claude/commands/*.md` | ? | ⬜ Not tested |
| Skills | `~/.claude/skills/` | ? | ⬜ Not tested |
| MCP Docker Tools | `mcp__MCP_DOCKER__*` | ? | ⬜ Not tested |
| Bash Scripts | `scripts/core/*.sh` | Should work (universal) | ⬜ Not tested |
| Templates | `templates/*.md` | Should work (just text) | ⬜ Not tested |

### Action Items

- [ ] **Test 1: Gemini CLI Command Structure**
  - Check if Gemini CLI has a commands directory similar to `.claude/commands/`
  - Test if markdown-based command definitions work
  - Document: `gemini --help` and configuration paths

- [ ] **Test 2: MCP Support in Gemini CLI**
  - Verify if Gemini CLI supports MCP protocol
  - Check Docker MCP Toolkit compatibility with Gemini
  - Test: Can Gemini access `mcp__MCP_DOCKER__obsidian_*` tools?

- [ ] **Test 3: Port Simple Command**
  - Start with `/idea` command (simplest, no external dependencies)
  - Create Gemini-compatible version
  - Compare: execution flow, output format, success rate

- [ ] **Test 4: Port YouTube Capture**
  - Test `/youtube-note` which uses MCP Docker YouTube tools
  - Dependencies: `mcp__MCP_DOCKER__get_video_info`, `get_transcript`
  - Check if same MCP tools accessible from Gemini

- [ ] **Test 5: Skills/Plugins Architecture**
  - Research Gemini CLI extension/plugin system
  - Check for SKILL.md equivalent
  - Test if Gemini has tool allowlists like Claude Code

### Potential Compatibility Layers

```
┌─────────────────────────────────────┐
│     Universal Plugin Package        │
├─────────────────────────────────────┤
│  commands/     (markdown files)     │
│  scripts/      (bash - universal)   │
│  templates/    (text - universal)   │
├─────────────────────────────────────┤
│         Adapter Layer               │
├──────────────┬──────────────────────┤
│ Claude Code  │    Gemini CLI        │
│ Adapter      │    Adapter           │
└──────────────┴──────────────────────┘
```

### Key Questions to Answer

1. Does Gemini CLI have a `.gemini/` config directory?
2. Can Gemini CLI source bash scripts from a plugin?
3. Does Gemini support MCP servers (especially Docker MCP)?
4. How does Gemini handle tool permissions/allowlists?
5. Is there a Gemini equivalent to `marketplace.json`?

### Research Commands to Run

```bash
# Check Gemini CLI config locations
gemini --version
ls -la ~/.gemini/ 2>/dev/null || echo "No .gemini directory"
gemini config list

# Check MCP support
gemini mcp list  # If MCP supported
gemini tools list  # List available tools

# Test simple prompt with tool use
gemini -p "What tools do you have access to?"
```

---

*Captured: 2025-12-30*
*Updated: 2025-12-30 - Added obsidian-vault-manager portability test plan*
