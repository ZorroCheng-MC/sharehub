---
title: "Development Plan: /slide Command - NotebookLM MCP Integration"
tags: [project, AI, Google, NotebookLM, MCP, automation, technical, KnowledgeFactory, development]
source: https://youtu.be/HRDYY-qSDuU
date: 2026-01-25
type: project
status: planning
priority: high
read: false
---

# Development Plan: /slide Command - NotebookLM MCP Integration

## Overview

Build a `/slide` command for KnowledgeFactory that sends Obsidian notes to NotebookLM via MCP server, generates multimedia assets (infographics, slides, audio), and returns shareable links.

**Goal:** Transform any vault note into visual/audio content via NotebookLM MCP integration, enabling one-command content generation.

## MCP Integration Approach

Instead of browser automation, we use the **Model Context Protocol (MCP)** to directly control NotebookLM programmatically. This is cleaner, more reliable, and supports all NotebookLM features.

### Why MCP over Browser Automation

| Aspect      | Browser Automation        | MCP Integration          |
| ----------- | ------------------------- | ------------------------ |
| Reliability | UI-dependent, fragile     | API-like stability       |
| Speed       | Slow (screenshot loops)   | Fast direct calls        |
| Features    | Limited by UI visibility  | Full programmatic access |
| Maintenance | Breaks when UI changes    | Stable interface         |
| Setup       | Chrome session management | Simple MCP config        |

## The 5-Level Workflow

Based on the [AntiGravity + NotebookLM tutorial](https://youtu.be/HRDYY-qSDuU):

| Level | Capability | Description |
|-------|------------|-------------|
| 1. Connection | MCP Setup | Link Claude/AntiGravity to NotebookLM via MCP server |
| 2. Research | Deep Research | Automate notebook creation and research tasks |
| 3. Building | Dashboards | Create interactive HTML dashboards from notebook data |
| 4. Multimedia | Audio & Slides | Programmatically generate Audio Overviews and Slide Decks |
| 5. Ingestion | Bulk Upload | Upload local files (PDFs/Text) into NotebookLM |

## MCP Server Options

### Option A: Python Version (Recommended for features)

**Repository:** [jacob-bd/notebooklm-mcp](https://github.com/jacob-bd/notebooklm-mcp)

**Best for:** Advanced users who want full control, including Audio Overviews, Slide Decks, and Deep Research mode.

**Installation:**
```bash
uv tool install notebooklm-mcp-server
```

**Features:**
- Create and manage notebooks
- Add sources (text, URLs, files)
- Generate Audio Overviews
- Generate Slide Decks
- Deep Research mode
- Full CRUD operations

### Option B: Node.js Version (Recommended for speed)

**Repository:** [PleasePrompto/notebooklm-mcp](https://github.com/PleasePrompto/notebooklm-mcp)

**Best for:** Quick setup and using NotebookLM primarily for retrieving answers with citations to prevent AI hallucinations.

**Installation:**
```bash
npx notebooklm-mcp@latest
```

**Features:**
- Fast notebook operations
- Citation-backed responses
- Lightweight setup
- Quick query/response cycle

## Implementation Plan

### Phase 1: MCP Server Setup

#### 1.1 Install NotebookLM MCP Server

```bash
# Option A: Python (full features)
uv tool install notebooklm-mcp-server

# Option B: Node.js (quick setup)
npx notebooklm-mcp@latest
```

#### 1.2 Configure Claude Code MCP Settings

Add to `.claude/settings.json` or global MCP config:

```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "notebooklm-mcp-server",
      "args": []
    }
  }
}
```

#### 1.3 Authentication Setup

- Google account authentication (one-time browser auth)
- Credentials cached for subsequent API calls

### Phase 2: /slide Command Implementation

#### 2.1 Slash Command Definition

**Location:** `.claude/commands/slide.md`

```markdown
# /slide - Generate Multimedia from Notes via NotebookLM MCP

Generate infographics, slides, audio overviews, and study guides from vault notes using NotebookLM MCP integration.

## Usage

/slide <filename.md> [--type <asset-type>]

## Arguments

- `filename.md` - The note to convert (relative to vault root)
- `--type` - Asset type: slide-deck (default), audio, study-guide, research

## Asset Types

| Type | Output | Use Case |
|------|--------|----------|
| slide-deck | Presentation slides | Visual presentations |
| audio | Audio Overview (MP3) | Podcast-style listening |
| study-guide | Structured outline | Learning materials |
| research | Deep Research report | Comprehensive analysis |

## Workflow

1. Read vault file via MCP Obsidian tools
2. Create NotebookLM notebook via MCP
3. Add note content as source
4. Trigger asset generation (slides/audio/etc)
5. Return shareable link

## Examples

# Generate slide deck (default)
/slide my-research-note.md

# Generate audio overview
/slide quarterly-report.md --type audio

# Generate deep research
/slide findings.md --type research
```

#### 2.2 MCP Tool Flow

```
STEP 1: Read vault file
├─ Use MCP Obsidian tools: get_vault_file("my-note.md")
└─ Extract content and metadata

STEP 2: Create NotebookLM Notebook
├─ Call: notebooklm_create_notebook(title)
└─ Returns: notebook_id

STEP 3: Add Source Content
├─ Call: notebooklm_add_source(notebook_id, content)
└─ Wait for processing

STEP 4: Generate Asset
├─ Call: notebooklm_generate_slides(notebook_id)
│   OR: notebooklm_generate_audio(notebook_id)
│   OR: notebooklm_deep_research(notebook_id, query)
└─ Returns: asset_url

STEP 5: Return Result
├─ Output shareable link
└─ Success message with notebook URL
```

### Phase 3: Integration with KnowledgeFactory

#### 3.1 Update obsidian-vault-manager Skill

Add `/slide` command to SKILL.md:

```markdown
| /slide | Generate slides/audio from notes | /slide note.md --type slide-deck |
```

#### 3.2 Output Queue Integration

**File:** `.github/data/queue/out-sld-{id}.json`

```json
{
  "type": "output",
  "command": "slide",
  "sourceNote": "2026-01-20-my-note.md",
  "assetType": "slide-deck",
  "cleanup": true,
  "cleanupDays": 30,
  "addedAt": "2026-01-20T10:00:00Z"
}
```

## Architecture Diagram

```
User's Obsidian Vault
        │
        │ /slide my-note.md --type slide-deck
        ▼
Claude Code (/slide command)
        │
        ├─ Read vault file (MCP Obsidian Tools)
        │
        ├─ Call NotebookLM MCP Server
        │
        ▼
NotebookLM MCP Server
        │
        ├─ Create notebook
        │
        ├─ Add note content as source
        │
        ├─ Generate slide deck / audio / research
        │
        └─ Return shareable link
        │
        ▼
Return to user
        │
        ├─ Asset link: https://notebooklm.google.com/...
        │
        ├─ Notebook link: https://notebooklm.google.com/...
        │
        └─ Success message
```

## Development Tasks

### MVP (Minimum Viable Product)

- [ ] Install and configure NotebookLM MCP server (Python version)
- [ ] Test MCP connection with simple notebook creation
- [ ] Create `.claude/commands/slide.md` command definition
- [ ] Implement slide-deck generation workflow
- [ ] Test end-to-end with sample note
- [ ] Document in obsidian-vault-manager SKILL.md

### Enhanced Version (v1.1)

- [ ] Support all asset types:
  - [ ] slide-deck
  - [ ] audio (Audio Overview)
  - [ ] study-guide
  - [ ] research (Deep Research)
- [ ] Add progress/status messages
- [ ] Error handling for generation failures
- [ ] Return both asset link AND notebook URL

### Future Enhancements (v2.0+)

- [ ] Bulk file ingestion (Level 5)
- [ ] Interactive dashboard generation (Level 3)
- [ ] Auto-cleanup old notebooks
- [ ] Integrate with DoubleCopy output queue
- [ ] Mobile trigger via DoubleCopy Mobile app
- [ ] Citation extraction for fact-checking

## Testing Checklist

- [ ] MCP server connects successfully
- [ ] Notebook creation works
- [ ] Source content uploads correctly
- [ ] Slide deck generation completes
- [ ] Audio overview generation completes
- [ ] Deep research mode works
- [ ] Shareable links are accessible
- [ ] Error handling works for failures
- [ ] Full process completes in reasonable time

## Resources

### Tutorial Video

- [Build a NotebookLM App with AntiGravity](https://youtu.be/HRDYY-qSDuU) - Step-by-step setup guide

### MCP Server Repositories

- [jacob-bd/notebooklm-mcp](https://github.com/jacob-bd/notebooklm-mcp) - Python version (full features)
- [PleasePrompto/notebooklm-mcp](https://github.com/PleasePrompto/notebooklm-mcp) - Node.js version (speed)

### Related Documentation

- [[KnowledgeFactory Enterprise]] - Parent system documentation
- [NotebookLM](https://notebooklm.google.com) - Google NotebookLM
- [Model Context Protocol](https://modelcontextprotocol.io) - MCP specification
- obsidian-vault-manager skill - Integration target

---

**Created**: 2026-01-20
**Updated**: 2026-01-25
**Status**: Planning → Ready for MCP Implementation
**Next Action**: Install NotebookLM MCP server and test basic notebook creation
