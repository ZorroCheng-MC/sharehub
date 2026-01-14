---
title: "Gemini CLI vs Claude Code Feature Parity Research"
date: 2026-01-14
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

## DoubleCopy Multi-Agent Testing Plan

**Related**: [DoubleCopy Development Plan](DoubleCopy-Development-Plan.md) | [DoubleCopy Source Code](doublecopy/)

DoubleCopy v1.0.34 currently uses Claude Code CLI exclusively. The roadmap includes multi-agent support for Gemini CLI, OpenAI Codex, and Qwen Code. This section documents the testing plan to validate Gemini CLI compatibility.

### Current DoubleCopy Architecture

```
DoubleCopy.app
    ↓
capture.sh (generated)
    ↓
claude -p "/capture $CONTENT"    ← Agent-specific
    ↓
/capture command → MCP tools → Obsidian note
```

**Key Discovery**: DoubleCopy uses slash command invocation (`claude -p "/capture $CONTENT"`) which properly follows templates. For Gemini CLI to work, it needs:

1. Non-interactive prompt mode (`gemini -p "..."` or equivalent)
2. Access to same MCP tools (obsidian, youtube-transcript, etc.)
3. Either slash command support OR direct prompt execution

### Gemini CLI Compatibility Tests

#### Test 1: Basic Non-Interactive Execution

**Objective**: Verify Gemini CLI can execute prompts non-interactively

```bash
# Test 1a: Check if Gemini has non-interactive mode
gemini --help | grep -i "prompt\|non-interactive\|-p"

# Test 1b: Test simple non-interactive execution
gemini -p "Hello, respond with just 'OK'" 2>&1

# Test 1c: Test with longer prompt
gemini -p "Create a simple markdown note with title 'Test' and one bullet point"
```

**Expected Result**: Gemini should execute and return output without interactive shell

**Status**: ⬜ Not tested

---

#### Test 2: MCP Server Access

**Objective**: Verify Gemini CLI can access Docker MCP servers

```bash
# Test 2a: Check if Gemini supports MCP
gemini mcp list 2>&1 || echo "MCP not supported or different command"

# Test 2b: List available tools
gemini tools list 2>&1

# Test 2c: Test MCP tool execution
gemini -p "Use the obsidian tool to list files in the vault root directory"
```

**Key Tools Needed for DoubleCopy**:
- `mcp__MCP_DOCKER__obsidian_append_content` - Create notes
- `mcp__MCP_DOCKER__get_video_info` - YouTube metadata
- `mcp__MCP_DOCKER__get_transcript` - YouTube transcript
- `mcp__MCP_DOCKER__firecrawl_scrape` - Web article fetching

**Status**: ⬜ Not tested

---

#### Test 3: Simple Capture Test (Idea Note)

**Objective**: Test Gemini creating a simple Obsidian note

```bash
# Test 3a: Direct prompt capture (no slash command)
CONTENT="Test idea for Gemini CLI integration"
DATE=$(date "+%Y-%m-%d")

gemini -p "Create a markdown note in Obsidian vault with:
- Filename: ${DATE}-gemini-test-idea.md
- Frontmatter with tags: idea, AI, testing
- Title: 'Gemini CLI Test Idea'
- Content: ${CONTENT}
Use the obsidian_append_content MCP tool to create the file."

# Test 3b: Verify note created
ls -la ~/Documents/Obsidian/Claudecode/${DATE}-gemini-*.md
```

**Status**: ⬜ Not tested

---

#### Test 4: YouTube Capture Test

**Objective**: Test Gemini capturing a YouTube video with transcript

```bash
# Test 4a: YouTube capture with MCP tools
VIDEO_URL="https://www.youtube.com/watch?v=dQw4w9WgXcQ"

gemini -p "Capture this YouTube video to Obsidian:
URL: ${VIDEO_URL}

1. Use get_video_info to get title, channel, duration
2. Use get_transcript to get the transcript
3. Create a structured note with:
   - Thumbnail image
   - Video metadata
   - Key points from transcript
4. Use obsidian_append_content to save the note"

# Test 4b: Check if note was created with thumbnail
grep -l "maxresdefault.jpg" ~/Documents/Obsidian/Claudecode/*.md | tail -1
```

**Status**: ⬜ Not tested

---

#### Test 5: capture.sh Modification for Multi-Agent

**Objective**: Design capture.sh to support agent switching

**Current capture.sh (Claude only)**:
```bash
claude -p "/capture $CONTENT"
```

**Proposed Multi-Agent capture.sh**:
```bash
#!/bin/bash
CONTENT="$1"
AGENT="${DOUBLECOPY_AGENT:-claude}"  # Default to claude

case "$AGENT" in
    claude)
        # Slash command invocation (works with Claude)
        claude -p "/capture $CONTENT"
        ;;
    gemini)
        # Direct prompt (if Gemini doesn't support slash commands)
        # Load capture prompt from template
        PROMPT=$(cat ~/.doublecopy/prompts/capture-gemini.md)
        PROMPT="${PROMPT//\$CONTENT/$CONTENT}"
        gemini -p "$PROMPT"
        ;;
    *)
        echo "Unknown agent: $AGENT"
        exit 1
        ;;
esac
```

**Action Items**:
- [ ] Create `~/.doublecopy/prompts/capture-gemini.md` template
- [ ] Test agent switching via environment variable
- [ ] Compare output quality between Claude and Gemini

**Status**: ⬜ Not implemented

---

#### Test 6: DoubleCopy App Modification

**Objective**: Add Gemini CLI support to DoubleCopy.app

**Swift Changes Required** (in `main.swift`):

```swift
// 1. Add Gemini to CLIAgent enum
enum CLIAgent: String, CaseIterable {
    case claude = "Claude Code"
    case gemini = "Gemini CLI"
}

// 2. Add Gemini check function
func checkGeminiCLI() -> Bool {
    let result = shell("which gemini || ls ~/.gemini/bin/gemini 2>/dev/null")
    return !result.isEmpty
}

// 3. Modify capture.sh generation
func installCaptureScript() {
    let agent = selectedAgent // From preferences
    let captureCommand: String

    switch agent {
    case .claude:
        captureCommand = "claude -p \"/capture $CONTENT\""
    case .gemini:
        captureCommand = "gemini -p \"$(cat ~/.doublecopy/prompts/capture.md)\""
    }
    // ... rest of script generation
}
```

**Status**: ⬜ Not implemented

---

### Testing Checklist Summary

| Test | Description | Status |
|------|-------------|--------|
| 1 | Non-interactive execution | ⬜ |
| 2 | MCP server access | ⬜ |
| 3 | Simple idea capture | ⬜ |
| 4 | YouTube capture with transcript | ⬜ |
| 5 | Multi-agent capture.sh | ⬜ |
| 6 | DoubleCopy app modification | ⬜ |

### Success Criteria

For Gemini CLI to be considered "DoubleCopy Compatible":

1. ✅ Can execute prompts non-interactively
2. ✅ Can access MCP Docker tools (obsidian, youtube, etc.)
3. ✅ Can create properly formatted Obsidian notes
4. ✅ Output quality comparable to Claude Code
5. ✅ Response time acceptable (<30 seconds for simple capture)

### Fallback Strategy

If Gemini CLI lacks certain capabilities:

| Missing Feature | Workaround |
|-----------------|------------|
| No MCP support | Use REST API calls directly in prompt |
| No non-interactive mode | Use expect/pexpect wrapper |
| No tool access | Fall back to basic text processing |
| Poor output quality | Use Claude as primary, Gemini as backup |

---

*Captured: 2025-12-30*
*Updated: 2025-12-30 - Added obsidian-vault-manager portability test plan*
*Updated: 2025-12-30 - Added DoubleCopy multi-agent testing plan with Gemini CLI action items*
