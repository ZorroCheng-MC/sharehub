---
title: "KnowledgeFactory - Quick Setup Cheatsheet"
layout: default
tags: [product, setup, cheatsheet, reference, advanced]
date: 2025-11-11
type: reference
---

# KnowledgeFactory - Quick Setup Cheatsheet

> **For advanced users** - Assumes familiarity with all tools
>
> Full guide: [KnowledgeFactory-Your-AI-Powered-2nd-Brain.md](./KnowledgeFactory-Your-AI-Powered-2nd-Brain.md)

---

## Prerequisites

- [ ] Claude Pro/Max subscription ($20-50/mo) - [Sign up](https://claude.ai/upgrade)
- [ ] macOS/Linux/Windows with CLI access
- [ ] GitHub account - [Create account](https://github.com/signup)

**Time: ~20 minutes for experienced users**

---

## 1. Foundation Install

### System Dependencies

- [ ] Install system dependencies:
```bash
# macOS
brew install git jq

# Linux
sudo apt install git jq

# Windows
choco install git jq
```

- [ ] Verify installations:
```bash
git --version && jq --version
```

### Desktop Applications

- [ ] Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
  - [ ] Start Docker Desktop application
  - [ ] Verify Docker is running: `docker --version`

- [ ] Download and install [Obsidian](https://obsidian.md/download)
  - [ ] Launch Obsidian once
  - [ ] Close after initial launch

- [ ] Download and install [Claude Code](https://claude.ai/code)
  - [ ] Sign in with Claude Pro/Max account

---

## 2. Obsidian Setup

### Create Vault

- [ ] **Create new vault**: Obsidian → File → New vault
  - Name: `KnowledgeFactory`
  - Location: `~/Documents/Obsidian/KnowledgeFactory`

### Install Required Plugins

Open **Settings → Community plugins** → Turn off Safe Mode → Browse (search in catalog)

**Required (5):**
- [ ] **MCP Tools** - Core MCP integration (click "Install Server" button after enabling)
- [ ] **Local REST API** - Auto-installed by MCP Tools, verify it's enabled
- [ ] **Terminal** - Integrated terminal in Obsidian (by polyipseity), run Claude Code in side panel
- [ ] **Smart Connections** - Semantic search (required for `/semantic-search` command)
- [ ] **Templater** - Advanced templates (required for advanced `/capture` templates)

### Retrieve Local REST API Key (CRITICAL)

**IMPORTANT**: Get this API key BEFORE installing Docker MCP servers. The Obsidian MCP server requires this key for configuration.

- [ ] **Copy Obsidian API key**: Settings → Community plugins → Local REST API → Copy API Key
  - Save this key somewhere secure (you'll need it in Step 4)
  - Example format: `0de5ba07c38bd3250722a4978d463ab886dd507607482628794be7f8ee2bd66a`

### Configure Image Folder (REQUIRED)

- [ ] **Set image folder**: Settings → Files & Links → Attachment folder path: `images`
  - Under "Default location for new attachments": Select "In the folder specified below"

---

## 3. Docker MCP Tool Setup

**Now that you have the Obsidian Local REST API key**, install and configure MCP servers.

Open **Docker Desktop → MCP Toolkit → Catalog** (search for each server and install)

### Install MCP Servers

**Required (3):**
- [ ] **GitHub Official** (needs API key)
- [ ] **YouTube Transcripts** 
- [ ] **Obsidian** (needs API key from Step 2)

**Recommended (3):**
- [ ] **Firecrawl** (needs API key)
- [ ] **Context7** 
- [ ] **Fetch (Reference)** 

**Optional (2):**
- [ ] **Memory (Reference)** 
- [ ] **Perplexity** (needs API key)

### Configure API Keys

For each server that requires an API key:

- [ ] **GitHub**: Generate token at [github.com/settings/tokens](https://github.com/settings/tokens) (scopes: `repo`, `public_repo`)
  - MCP Toolkit → My servers → GitHub Official → Paste token
- [ ] **Obsidian**: MCP Toolkit → My servers → Obsidian → Add environment variable:
  - Key: `OBSIDIAN_LOCAL_REST_API_KEY`
  - Value: [API key from Step 2]
  - Save and restart server
- [ ] **Firecrawl** (recommended): Get key at [firecrawl.dev](https://firecrawl.dev) (1000 free/month)
  - MCP Toolkit → My servers → Firecrawl → Paste key
- [ ] **Perplexity** (optional): Get key at [perplexity.ai/settings/api](https://perplexity.ai/settings/api)
  - MCP Toolkit → My servers → Perplexity → Paste key

### Connect Claude Code

- [ ] **Connect to Claude Code**: MCP Toolkit → Clients → Claude Code → Connect
  - Verify all servers show green status

---

## 4. Document Vault GitHub Setup

### Private Vault (TIER 1)

- [ ] **Setup private vault git**:
```bash
cd ~/Documents/Obsidian/KnowledgeFactory
git init
cat > .gitignore << 'EOF'
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
.DS_Store
*.tmp
EOF
git add .
git commit -m "Initial commit: Private knowledge vault"
# Optional: Push to private GitHub repo
gh repo create my-knowledge-vault --private --source=. --push
```

### Public Sharehub (TIER 2 & 3)

- [ ] **Setup public sharehub**:
```bash
cd ~/Documents
git clone https://github.com/ZorroCheng-MC/sharehub.git
cd sharehub
rm -rf .git
git init
git add .
git commit -m "Initial commit: My document portal"
gh repo create my-sharehub --public --source=. --push
gh repo edit --enable-pages --pages-branch main
# Edit _config.yml: Set baseurl: "/my-sharehub"
git add _config.yml
git commit -m "Configure Jekyll baseurl"
git push
```

- [ ] **Verify deployment**: Wait ~60 seconds, visit `https://YOUR_USERNAME.github.io/my-sharehub`

---

## 5. Install obsidian-vault-manager Plugin

- [ ] **Add plugin marketplace**:
```bash
cd ~/Documents/Obsidian/KnowledgeFactory
claude

# In Claude Code CLI:
/plugin marketplace add ZorroCheng-MC/obsidian-vault-manager-plugin
```

- [ ] **Install the plugin**:
```bash
# Browse available plugins
/plugin

# Or install directly
/plugin install obsidian-vault-manager@ZorroCheng-MC/obsidian-vault-manager-plugin
```

The plugin will be available immediately with slash commands like `/capture`, `/publish`, etc.

---

## 6. Run Setup Wizard

- [ ] **Launch Claude Code and run setup**:
```bash
cd ~/Documents/Obsidian/KnowledgeFactory
claude
# In Claude Code CLI, run:
/setup
```
  - Provide: Vault path (auto-detected), Sharehub path, Sharehub repo, GitHub Pages URL
  - Creates: `.claude/config.sh` and `.claude/settings.local.json`

---

## 7. Verification

- [ ] **Test all commands**:
```bash
/capture Build a Chrome extension that auto-captures to Obsidian
/idea Knowledge graph visualization for Obsidian
/youtube-note https://youtube.com/watch?v=VIDEO_ID
/semantic-search "machine learning workflows"
/gitingest https://github.com/user/repo
/publish my-first-note.md
```

- [ ] **Verify MCP servers**: Check that `mcp__github__*`, `mcp__obsidian-mcp-tools__*`, `mcp__fetch__fetch`, `mcp__gitingest__*` commands are available

- [ ] **Check config paths**: View `~/.claude/skills/obsidian-vault-manager/.claude/config.sh` and verify paths are correct

---

## 8. Three-Tier Publishing

**TIER 1: Private** - Don't run `/publish`, note stays in vault

**TIER 2: Trusted Circle** - Add `access: private` to frontmatter, run `/publish`, requires password "maco"

**TIER 3: Public** - Run `/publish` without `access: private`, anyone can read

---

## Troubleshooting

**MCP Servers Not Working:**
- Check Docker Desktop → MCP Toolkit → My servers (all should be green)
- Verify API keys configured, restart server, or restart Docker Desktop

**`/publish` Command Fails:**
- Verify config exists: `cat ~/.claude/skills/obsidian-vault-manager/.claude/config.sh`
- Check sharehub is git repo: `cd ~/Documents/sharehub && git status`
- Verify remote: `git remote -v`

**Obsidian MCP Tools Not Working:**
- Settings → Community plugins → MCP Tools
- Verify "Install Server" was clicked, Local REST API enabled
- Restart Obsidian if needed

**API Rate Limits:**
- GitHub: 5000 requests/hour (authenticated)
- Firecrawl: 1000 requests/month (free tier)
- Perplexity: Depends on your plan

---

## Architecture Summary

```
┌─────────────────────────────────────────────────────────────┐
│                    KNOWLEDGEFACTORY                          │
├─────────────────────────────────────────────────────────────┤
│  Layer 1: Foundation                                         │
│  ├── Claude Code (AI assistant)                             │
│  ├── Obsidian (knowledge base)                              │
│  └── Docker Desktop (MCP infrastructure)                    │
├─────────────────────────────────────────────────────────────┤
│  Layer 2: MCP Servers                                        │
│  ├── GitHub Official (repo ops)                             │
│  ├── YouTube Transcripts (video data)                       │
│  ├── Firecrawl (web scraping)                               │
│  ├── Obsidian (vault ops)                                   │
│  ├── Context7 (docs)                                         │
│  ├── Fetch (web fetch)                                       │
│  ├── Memory (knowledge graph)                               │
│  └── Perplexity (AI search)                                 │
├─────────────────────────────────────────────────────────────┤
│  Layer 3: Plugin                                             │
│  └── obsidian-vault-manager                                 │
│      ├── /capture - Quick capture                           │
│      ├── /idea - AI-tagged ideas                            │
│      ├── /youtube-note - Video notes                        │
│      ├── /gitingest - Repo analysis                         │
│      ├── /study-guide - Content summaries                   │
│      ├── /semantic-search - Smart search                    │
│      ├── /bulk-auto-tag - Bulk tagging                      │
│      └── /publish - Publish to sharehub                     │
├─────────────────────────────────────────────────────────────┤
│  Layer 4: System Tools                                       │
│  ├── git (version control)                                  │
│  ├── jq (JSON processing)                                   │
│  ├── gh (GitHub CLI - optional)                             │
│  └── jekyll (via GitHub Actions)                            │
└─────────────────────────────────────────────────────────────┘

Two-Repository Model:
├── Vault (Private) - All notes, Tier 1 only
└── Sharehub (Public) - Selected notes, Tier 2 + 3
```

---

## Available Slash Commands

```bash
/capture [content]          # Quick capture with AI tagging
/idea [idea-text]          # Create idea file with tags
/youtube-note [url]        # Fetch video transcript
/study-guide [source]      # Generate study guide
/gitingest [repo-url]      # Analyze repository
/semantic-search [query]   # Semantic vault search
/bulk-auto-tag [pattern]   # Bulk AI tagging
/publish [filename]        # Publish to sharehub
```

---

## Quick Reference URLs

- **Claude Code**: [claude.ai/code](https://claude.ai/code)
- **Obsidian**: [obsidian.md](https://obsidian.md)
- **Docker Desktop**: [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
- **GitHub Tokens**: [github.com/settings/tokens](https://github.com/settings/tokens)
- **Firecrawl API**: [firecrawl.dev](https://firecrawl.dev)
- **Perplexity API**: [perplexity.ai/settings/api](https://perplexity.ai/settings/api)
- **Sharehub Template**: [github.com/ZorroCheng-MC/sharehub](https://github.com/ZorroCheng-MC/sharehub)
- **Vault Manager Plugin**: [github.com/ZorroCheng-MC/obsidian-vault-manager-plugin](https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin)

---

## Cost Breakdown

| Item | Cost | Required? |
|------|------|-----------|
| Claude Pro/Max | $20-50/mo | ✅ Required |
| Docker Desktop | Free | ✅ Required |
| Obsidian | Free | ✅ Required |
| GitHub | Free | ✅ Required |
| Firecrawl | Free (1000/mo) | ⭐ Recommended |
| Perplexity | Varies | ❌ Optional |

**Total minimum: $20/mo (Claude Pro only)**

---

*Setup time: ~20 minutes for advanced users | Full setup: ~60 minutes for beginners*
