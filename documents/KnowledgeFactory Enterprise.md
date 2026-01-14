---
title: "KnowledgeFactory Enterprise"
tags: [product, AI, knowledge-management, projects, marketing, productivity, workflow, automation, lifecycle, enterprise]
date: 2026-01-14
type: product
status: active
priority: high
---

# KnowledgeFactory Enterprise

## Overview

**KnowledgeFactory Enterprise** is the complete technical specification and architecture documentation for the KnowledgeFactory ecosystem - an AI-powered knowledge manufacturing system that transforms raw information into refined, organized, and shareable insights.

This document serves as the **enterprise reference** for understanding all components, their relationships, and how to configure them for maximum productivity. For a user-focused introduction, see [[KnowledgeFactory/KnowledgeFactory-Your-AI-Powered-2nd-Brain]].

---

## System Architecture

### The Knowledge Factory Model

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    KNOWLEDGEFACTORY ENTERPRISE ARCHITECTURE                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────────────────┐    │
│  │  INPUT LAYER │     │ PROCESSING   │     │     OUTPUT LAYER          │    │
│  │              │────▶│    LAYER     │────▶│                          │    │
│  │  DoubleCopy  │     │              │     │  Sharehub (GitHub Pages) │    │
│  │  CLI Commands│     │ AI CLI Coder │     │  Share URLs              │    │
│  │  Mobile      │     │ MCP Servers  │     │  Obsidian Notes          │    │
│  └──────────────┘     │ Skills       │     └──────────────────────────┘    │
│                       └──────────────┘                                      │
│                              │                                              │
│                              ▼                                              │
│                    ┌──────────────────┐                                     │
│                    │  STORAGE LAYER   │                                     │
│                    │                  │                                     │
│                    │  Obsidian Vault  │                                     │
│                    │  Git Versioning  │                                     │
│                    │  Smart Search    │                                     │
│                    └──────────────────┘                                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Core Design Principles

1. **CLI-Agnostic**: Works with Claude Code, OpenCode, or other compatible AI CLIs
2. **Local-First**: All data stored locally in markdown files
3. **Plugin Architecture**: Extensible via MCP servers and skills
4. **Zero Vendor Lock-in**: Open standards (Markdown, Git, MCP protocol)
5. **Automation-First**: AI handles repetitive tasks at every stage

---

## Component Inventory

### Quick Reference: All Members

| Component | Purpose | Required? | Source |
|-----------|---------|-----------|--------|
| **Obsidian** | Knowledge vault & editor | Required | [obsidian.md](https://obsidian.md) |
| **AI CLI Coder** | AI command interface | Required (choose one) | See CLI section |
| **Docker MCP Toolkit** | MCP server management | Required | Docker Desktop |
| **obsidian-vault-manager** | Skills & automation | Required | GitHub |
| **Sharehub** | Publishing platform | Required | GitHub |
| **DoubleCopy** | Clipboard capture | Optional | Local build |

---

## 1. Obsidian (Knowledge Vault)

### Purpose
Local-first markdown knowledge base with graph visualization, plugin ecosystem, and powerful search.

### Required Plugins

| Plugin | Purpose | Installation |
|--------|---------|-------------|
| **MCP Tools** | MCP server integration | Community Plugins → "MCP Tools" |
| **Local REST API** | API access for automation | Auto-installed with MCP Tools |
| **Templater** | Advanced templates | Community Plugins → "Templater" |
| **Smart Connections** | Semantic search & AI linking | Community Plugins → "Smart Connections" |
| **Terminal** | Integrated CLI in Obsidian | Community Plugins → "Terminal" |

### Configuration Requirements

**1. MCP Tools Plugin Setup:**
- Install from Community Plugins
- Click "Install Server" button (installs MCP Server v0.2.27+)
- Verify Local REST API is running

**2. Get Local REST API Key:**
```bash
# Via Settings:
# Obsidian → Settings → Community Plugins → Local REST API → API Key

# Via terminal:
cat "$VAULT_PATH/.obsidian/plugins/obsidian-local-rest-api/data.json" | jq -r '.apiKey'
```

**3. Image Folder Configuration:**
- Settings → Files & Links → Default location for new attachments
- Select: "In the folder specified below"
- Set folder: `images`

---

## 2. AI CLI Coder (Interchangeable)

### Overview

KnowledgeFactory is designed to work with **any MCP-compatible AI CLI**. The skill system is portable across different AI coding assistants.

### Supported CLIs

| CLI | Provider | Best For | MCP Support |
|-----|----------|----------|-------------|
| **Claude Code** | Anthropic | Full-featured, production use | Native |
| **OpenCode** | SST (Open Source) | Multi-provider flexibility | Native |
| **Gemini CLI** | Google | Gemini model users | Partial |
| **Codex** | OpenAI | GPT-4 users | Partial |

### Primary: Claude Code

**Best for**: Users with Claude Pro/Max subscription who want the most polished experience.

**Installation:**
```bash
# macOS/Linux
curl -fsSL https://claude.ai/install.sh | bash

# Verify
claude --version
```

**Configuration Files:**
- `CLAUDE.md` - Project instructions
- `.claude/commands/*.md` - Slash commands
- `.claude/skills/*/SKILL.md` - Skills
- `.claude/settings.local.json` - Local settings

### Alternative: OpenCode (Recommended for Non-Claude Users)

**Best for**: Users who want provider flexibility (75+ LLM providers) or don't have Claude subscription.

**Why OpenCode as Default Alternative:**
- **Native Claude Code compatibility** - Reads `.claude/skills/` directory directly
- **Open source** - No vendor lock-in
- **Multi-provider** - Works with 75+ LLM providers (Anthropic, OpenAI, Google, local models)
- **Similar UX** - TUI interface similar to Claude Code
- **MCP native support** - Full Model Context Protocol integration

**Installation:**
```bash
# macOS
brew install sst/tap/opencode

# Linux/macOS (curl)
curl -fsSL https://opencode.ai/install | bash

# Verify
opencode --version
```

**Configuration Files:**
- `AGENTS.md` - Project instructions (equivalent to CLAUDE.md)
- `.opencode/command/*.md` - Slash commands
- Both `.opencode/skill/` AND `.claude/skills/` - Skills (reads both!)
- `opencode.json` - Configuration

### Compatibility Matrix

| Feature | Claude Code | OpenCode | Notes |
|---------|------------|----------|-------|
| **Skills Location** | `.claude/skills/` | `.claude/skills/` ✅ | Native compatibility |
| **Instructions File** | `CLAUDE.md` | `AGENTS.md` | Different names |
| **Commands Location** | `.claude/commands/` | `.opencode/command/` | Requires duplication |
| **MCP Servers** | Docker Desktop | `opencode.json` | Different config |
| **Subagents** | `Task` tool | `@mention` system | Different syntax |
| **Todo Tracking** | `TodoWrite` | `update_plan` | Different tool name |

### Skills Portability

**The obsidian-vault-manager skill works with both CLIs without modification** because OpenCode reads from `.claude/skills/` natively.

```
~/.claude/skills/obsidian-vault-manager/
├── SKILL.md          ← Read by BOTH Claude Code AND OpenCode
├── templates/
├── scripts/
└── ...
```

---

## 3. Docker MCP Toolkit

### Purpose
Container platform for running MCP (Model Context Protocol) servers that provide specialized capabilities.

### Required MCP Servers

| Server | Purpose | API Key Required |
|--------|---------|-----------------|
| **Obsidian** | Vault file operations | No (uses REST API key) |
| **GitHub Official** | Repository operations | Yes |
| **YouTube Transcripts** | Video transcript extraction | No |
| **Firecrawl** | Web content scraping | Yes |

### Recommended MCP Servers

| Server | Purpose | API Key Required |
|--------|---------|-----------------|
| **Context7** | Library documentation | No |
| **Fetch (Reference)** | General web fetching | No |
| **Memory (Reference)** | Knowledge graph | No |
| **Perplexity** | AI-powered search | Yes |

### Installation (Docker Desktop)

```
1. Docker Desktop → MCP Toolkit (left sidebar)
2. Catalog tab → Search server name → Click "Add"
3. My servers tab → Configure API keys
4. Clients tab → Click "Connect" on your CLI
```

### API Key Configuration

**GitHub Official:**
1. Go to: https://github.com/settings/tokens
2. Generate new token (classic)
3. Scopes: `repo`, `public_repo`
4. Add to MCP Toolkit → My servers → GitHub Official

**Firecrawl:**
1. Sign up: https://firecrawl.dev
2. Copy API key from dashboard
3. Add to MCP Toolkit → My servers → Firecrawl

**Obsidian Server:**
1. Get API key from Obsidian REST API plugin (see above)
2. Add to MCP Toolkit → My servers → Obsidian
3. Key name: `OBSIDIAN_LOCAL_REST_API_KEY`

### OpenCode MCP Configuration

For OpenCode users, create `opencode.json` in project root:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "instructions": ["AGENTS.md"],
  "mcp": {
    "obsidian-mcp-tools": {
      "type": "local",
      "command": ["docker", "run", "-i", "--rm", "mcp/obsidian"],
      "enabled": true,
      "environment": {
        "OBSIDIAN_LOCAL_REST_API_KEY": "your-api-key-here"
      }
    },
    "github": {
      "type": "local",
      "command": ["docker", "run", "-i", "--rm", "mcp/github"],
      "enabled": true,
      "environment": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token-here"
      }
    },
    "youtube-transcript": {
      "type": "local",
      "command": ["docker", "run", "-i", "--rm", "mcp/youtube-transcript"],
      "enabled": true
    },
    "firecrawl": {
      "type": "local",
      "command": ["docker", "run", "-i", "--rm", "mcp/firecrawl"],
      "enabled": true,
      "environment": {
        "FIRECRAWL_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

---

## 4. Skill System (obsidian-vault-manager)

### Purpose
Provides slash commands, templates, and automation workflows for the complete knowledge lifecycle.

### Location
```
~/.claude/skills/obsidian-vault-manager/
├── SKILL.md                    # Main skill definition (766 lines)
├── README.md                   # Documentation
├── MCP_ARCHITECTURE.md         # MCP tool reference
├── scripts/
│   ├── core/
│   │   ├── fetch-youtube-transcript.sh
│   │   └── publish.sh
│   └── validation/
│       ├── validate-frontmatter.py
│       └── validate-mcp-tools.sh
└── templates/
    ├── youtube-note-template.md
    ├── idea-template.md
    └── study-guide-template.md
```

### Available Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `/capture` | Universal content capture (auto-routes) | `/capture https://youtube.com/...` |
| `/youtube-note` | YouTube video with transcript | `/youtube-note https://youtube.com/watch?v=...` |
| `/idea` | Quick idea with AI tagging | `/idea Build a browser extension` |
| `/gitingest` | GitHub repository analysis | `/gitingest https://github.com/user/repo` |
| `/publish` | Publish to GitHub Pages | `/publish my-note.md` |
| `/share` | Generate shareable URL (Plannotator variant) | `/share my-note.md` |
| `/study-guide` | Generate study materials | `/study-guide topic-name` |
| `/semantic-search` | Search by meaning | `/semantic-search AI automation` |
| `/bulk-auto-tag` | Batch tag existing notes | `/bulk-auto-tag "*.md"` |

### Content Routing Logic

The `/capture` command auto-detects content type:

```
Input Type              → Routed To
─────────────────────────────────────
youtube.com URL         → /youtube-note
youtu.be URL            → /youtube-note
github.com URL          → /gitingest
Other HTTP/HTTPS URL    → Article capture
Plain text              → /idea
```

### Tag Taxonomy

All notes use this standardized taxonomy for Obsidian Bases compatibility:

**Content Type** (1 required):
`video`, `idea`, `article`, `study-guide`, `repository`, `reference`, `project`

**Topics** (2-4 recommended):
`AI`, `productivity`, `knowledge-management`, `development`, `learning`, `research`, `writing`, `tools`, `business`, `design`, `automation`, `data-science`, `web-development`, `personal-growth`, `finance`

**Status** (1 required):
`inbox`, `processing`, `evergreen`, `published`, `archived`, `needs-review`

**Metadata** (0-2 optional):
`high-priority`, `quick-read`, `deep-dive`, `technical`, `conceptual`, `actionable`, `tutorial`, `inspiration`

### Skill Installation

**For Claude Code:**
```bash
# Clone to skills directory
git clone https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin.git \
  ~/.claude/skills/obsidian-vault-manager
```

**For OpenCode:**
```bash
# Same location - OpenCode reads .claude/skills/ natively
git clone https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin.git \
  ~/.claude/skills/obsidian-vault-manager
```

---

## 5. Sharehub (Publishing Platform)

### Purpose
Jekyll-based GitHub Pages publishing platform with tag-based access control.

### Repository
- **Source**: https://github.com/ZorroCheng-MC/sharehub
- **Deployment**: https://zorrocheng-mc.github.io/sharehub
- **Local Path**: `~/Dev/sharehub`

### Directory Structure
```
~/Dev/sharehub/
├── _config.yml              # Jekyll configuration
├── _layouts/
│   └── universal.html       # Layout with password protection
├── .github/workflows/
│   ├── jekyll.yml          # Auto-deploy to Pages
│   └── add-frontmatter.yml # Auto-add YAML headers
├── documents/              # Published documents
├── images/                 # Image assets
├── index.html              # Document listing
└── share.html              # URL-based sharing
```

### Access Control Model

**Three-Tier Sensitivity:**

```
TIER 1: PRIVATE (Vault Only)
├── Notes stay in Obsidian vault
├── Never published
└── Access: Only you

TIER 2: TRUSTED CIRCLE
├── Published to Sharehub with access: private
├── Password protected (password: "maco")
└── Share URL with trusted team

TIER 3: PUBLIC
├── Published to Sharehub (no access tag)
├── No password required
└── Anyone with link can read
```

**Frontmatter Configuration:**

```yaml
# TIER 2: Password protected
---
title: "Internal Document"
access: private
---

# TIER 3: Public
---
title: "Public Article"
---
```

### Publishing Workflow

```bash
# In Obsidian vault with Claude Code/OpenCode
/publish my-note.md

# Script performs:
# 1. Read note from vault
# 2. Copy images to sharehub/images/
# 3. Convert image paths: ./images/ → /sharehub/images/
# 4. Write note to sharehub/documents/
# 5. Git commit + push
# 6. GitHub Pages deploys (~1-2 minutes)

# Result URL:
# https://username.github.io/sharehub/documents/my-note.html
```

---

## 6. DoubleCopy (Capture Utility)

### Purpose
macOS menu bar application enabling **permission-free clipboard capture**. Double-copy any content to automatically create a note in Obsidian.

### Source Location
```
/Users/zorro/Documents/Obsidian/Claudecode/doublecopy/
```

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    DOUBLECOPY ARCHITECTURE                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐   │
│  │   Clipboard │     │  Detection  │     │   Capture   │   │
│  │   Monitor   │────▶│   Engine    │────▶│   Script    │   │
│  │             │     │             │     │             │   │
│  │ NSPasteboard│     │ Same hash + │     │ claude -p   │   │
│  │ changeCount │     │ time < 0.8s │     │ "/capture"  │   │
│  └─────────────┘     └─────────────┘     └─────────────┘   │
│         │                                       │           │
│         ▼                                       ▼           │
│  ┌─────────────┐                       ┌─────────────┐      │
│  │  Menu Bar   │                       │  Obsidian   │      │
│  │   Status    │                       │   Vault     │      │
│  └─────────────┘                       └─────────────┘      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Key Innovation

**No Permissions Required!**

DoubleCopy uses `NSPasteboard.changeCount` which increments on every Cmd+C, even for identical content. By detecting:
- Same content hash
- Within 0.8 seconds

...it identifies intentional "double copy" gestures without Accessibility permissions.

### Technical Specs

| Attribute | Value |
|-----------|-------|
| Language | Swift 5.x |
| Framework | Cocoa (macOS native) |
| Size | ~4,500 lines |
| Version | 1.0.34 |
| Min macOS | 11.0 (Big Sur) |
| Detection Interval | 0.8s (configurable) |
| Polling Interval | 0.3s |

### Multi-CLI Support (Planned)

DoubleCopy is designed to support multiple AI CLIs:

```swift
enum CLIAgent: String {
    case claude = "Claude Code"    // ✅ Implemented
    case opencode = "OpenCode"     // 🔜 Planned (default alternative)
    case gemini = "Gemini CLI"     // 🔜 Planned
    case codex = "OpenAI Codex"    // 🔜 Planned
}
```

### Build & Install

```bash
cd doublecopy

# Build application
./build-app.sh

# Build DMG for distribution
./build-dmg.sh

# Output: dist/DoubleCopy-1.0.34.dmg
```

---

## Installation Guide

### Prerequisites Checklist

- [ ] **macOS, Linux, or Windows** computer
- [ ] **Claude Pro/Max** subscription (for Claude Code) OR any LLM API key (for OpenCode)
- [ ] **GitHub account** (free tier sufficient)
- [ ] **Docker Desktop** installed
- [ ] **Git** and **jq** installed

### Phase 1: Foundation (15 minutes)

**1.1 Install Docker Desktop**
```bash
# Download from: https://docker.com/products/docker-desktop
# Launch and wait for green "Running" indicator
```

**1.2 Install Obsidian**
```bash
# Download from: https://obsidian.md
# Create vault: File → New vault → Name: "KnowledgeFactory"
# Remember path: ~/Documents/Obsidian/KnowledgeFactory
```

**1.3 Install System Dependencies**
```bash
# macOS
brew install git jq

# Linux
sudo apt install git jq

# Verify
git --version && jq --version
```

### Phase 2: Choose Your CLI (10 minutes)

**Option A: Claude Code** (Recommended if you have Claude subscription)
```bash
# Install
curl -fsSL https://claude.ai/install.sh | bash

# Authenticate
claude auth

# Verify
claude --version
```

**Option B: OpenCode** (Recommended alternative for non-Claude users)
```bash
# Install
brew install sst/tap/opencode  # macOS
# OR
curl -fsSL https://opencode.ai/install | bash

# Configure model in opencode.json (see MCP section)

# Verify
opencode --version
```

### Phase 3: Configure Obsidian Plugins (10 minutes)

**3.1 Install MCP Tools Plugin**
1. Obsidian → Settings → Community plugins → Browse
2. Search: "MCP Tools" → Install → Enable
3. Click "Install Server" in plugin settings
4. Verify: MCP Server v0.2.27+ installed

**3.2 Get REST API Key**
1. Settings → Community Plugins → Local REST API
2. Copy API Key (long hexadecimal string)
3. Save for Phase 4

**3.3 Configure Image Folder**
1. Settings → Files & Links
2. Default location: "In the folder specified below"
3. Folder path: `images`

### Phase 4: Configure MCP Servers (15 minutes)

**4.1 Install Required Servers (Docker Desktop)**
1. MCP Toolkit → Catalog
2. Add: GitHub Official, YouTube Transcripts, Firecrawl, Obsidian

**4.2 Configure API Keys**
- GitHub: Token from github.com/settings/tokens
- Firecrawl: Key from firecrawl.dev
- Obsidian: REST API key from Phase 3

**4.3 Connect CLI**
- MCP Toolkit → Clients → Connect (Claude Code or configure opencode.json)

### Phase 5: Install Skills (5 minutes)

```bash
# Clone skill to standard location (works for both CLIs)
git clone https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin.git \
  ~/.claude/skills/obsidian-vault-manager
```

### Phase 6: Set Up Sharehub (10 minutes)

```bash
# Clone and configure
cd ~/Dev
git clone https://github.com/ZorroCheng-MC/sharehub.git
cd sharehub

# Initialize as your repo
rm -rf .git && git init && git add . && git commit -m "Initial"

# Create GitHub repo
gh repo create my-sharehub --public --source=. --push
gh repo edit --enable-pages --pages-branch main

# Update baseurl in _config.yml
```

### Phase 7: Test Installation (5 minutes)

```bash
# Navigate to vault
cd ~/Documents/Obsidian/KnowledgeFactory

# Start CLI
claude  # or: opencode

# Test capture
/idea Test note for KnowledgeFactory installation

# Verify note created in Obsidian
```

---

## CLI Migration Guide: Claude Code → OpenCode

### What Works Automatically

| Component | Migration Required? |
|-----------|-------------------|
| Skills in `~/.claude/skills/` | **None** - OpenCode reads natively |
| Templates | **None** - Part of skill |
| Scripts | **None** - Part of skill |
| MCP server concepts | **None** - Same protocol |

### What Needs Manual Setup

#### 1. Instructions File

**Claude Code:** `CLAUDE.md`
**OpenCode:** `AGENTS.md`

**Migration:**
```bash
# Copy and rename
cp CLAUDE.md AGENTS.md

# Or symlink
ln -s CLAUDE.md AGENTS.md
```

#### 2. Slash Commands

**Claude Code:** `.claude/commands/*.md`
**OpenCode:** `.opencode/command/*.md`

**Migration:**
```bash
# Create OpenCode command directory
mkdir -p .opencode/command

# Copy commands
cp .claude/commands/*.md .opencode/command/

# Or symlink entire directory
ln -s ../.claude/commands .opencode/command
```

#### 3. MCP Server Configuration

**Claude Code:** Docker Desktop → MCP Toolkit → Clients
**OpenCode:** `opencode.json` file

**Migration:**
Create `opencode.json` with MCP server definitions (see Docker MCP section above).

#### 4. Tool Name Translations

If your custom commands reference Claude-specific tools:

| Claude Code | OpenCode | Notes |
|------------|----------|-------|
| `TodoWrite` | `update_plan` | Different tool name |
| `Task` tool | `@agent-name` | Different subagent syntax |
| `Skill` tool | `use_skill` | Similar concept |

---

## Daily Workflows

### Morning Capture Session

```bash
# Start CLI in vault
cd ~/Documents/Obsidian/KnowledgeFactory
claude  # or: opencode

# Capture YouTube video
/youtube-note https://youtube.com/watch?v=abc123

# Capture GitHub repo
/gitingest https://github.com/user/interesting-project

# Quick idea
/idea Use AI to summarize daily Slack messages
```

### Mobile Capture (via DoubleCopy)

```
1. See interesting content anywhere
2. Copy once (content in clipboard)
3. Copy again within 0.8s (double-copy gesture)
4. DoubleCopy triggers /capture command
5. Note appears in Obsidian
```

### Publishing Workflow

```bash
# Publish to sharehub
/publish my-article.md

# Result: https://username.github.io/sharehub/documents/my-article.html
```

---

## Troubleshooting

### MCP Server Connection Issues

**Symptom**: Commands fail with "MCP server not available"

**Fix:**
1. Docker Desktop → MCP Toolkit → My servers
2. Verify servers show green status
3. Restart server if needed
4. Check API keys are configured

### Skills Not Found

**Symptom**: `/capture` command not recognized

**Fix:**
```bash
# Verify skill exists
ls ~/.claude/skills/obsidian-vault-manager/SKILL.md

# Re-clone if missing
git clone https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin.git \
  ~/.claude/skills/obsidian-vault-manager
```

### OpenCode Not Reading Claude Skills

**Symptom**: OpenCode doesn't see skills in `.claude/skills/`

**Fix:**
```bash
# Verify OpenCode version (needs recent version)
opencode --version

# Check skill path in opencode.json
{
  "instructions": ["AGENTS.md", ".claude/skills/*/SKILL.md"]
}
```

### DoubleCopy Not Capturing

**Symptom**: Double-copy gesture not detected

**Fix:**
1. Check DoubleCopy is running (menu bar icon visible)
2. Verify vault path in Preferences
3. Check sensitivity setting (try "Relaxed" = 1.2s)
4. Review logs: View Logs in menu

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0.0 | 2025-01-14 | Full rewrite with OpenCode compatibility, comprehensive architecture documentation |
| 1.0.0 | 2025-01-01 | Initial outline of KnowledgeFactory Enterprise members |

---

## Related Documentation

- [[KnowledgeFactory/KnowledgeFactory-Your-AI-Powered-2nd-Brain]] - User-focused introduction
- [obsidian-vault-manager-plugin](https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin) - Skill source
- [sharehub](https://github.com/ZorroCheng-MC/sharehub) - Publishing platform
- [OpenCode Docs](https://opencode.ai/docs/) - OpenCode official documentation
- [Claude Code Docs](https://claude.ai/code) - Claude Code official documentation

---

*KnowledgeFactory Enterprise v2.0.0 | CLI-Agnostic Knowledge Manufacturing System*
