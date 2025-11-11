---
title: "KnowledgeFactory - Quick Setup Cheatsheet"
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

- ✅ Claude Pro/Max subscription ($20-50/mo)
- ✅ macOS/Linux/Windows with CLI access
- ✅ GitHub account

**Time: ~20 minutes for experienced users**

---

## 1. Foundation Install

```bash
# System dependencies
brew install git jq  # macOS
# sudo apt install git jq  # Linux
# choco install git jq  # Windows

# Verify
git --version && jq --version
```

**Desktop Apps:**
1. Docker Desktop → [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Obsidian → [obsidian.md](https://obsidian.md/)
3. Claude Code → [claude.ai/code](https://claude.ai/code)

---

## 2. Obsidian Setup

```bash
# 1. Create vault
#    Obsidian → File → New vault → Name: "KnowledgeFactory"
#    Location: ~/Documents/Obsidian/KnowledgeFactory
```

### Install Required Plugins

**Settings → Community plugins → Turn off Safe Mode → Browse**

**Required:**
1. **MCP Tools** - Core MCP integration
   - Install + Enable
   - Click "Install Server" button (installs MCP Server v0.2.27+)
   - This installs Local REST API server
   - Required for MCP Obsidian server to work

**Dependencies (installed automatically or manually):**
2. **Local REST API** - REST API for vault operations
   - Auto-installed by MCP Tools
   - Verify: MCP Tools settings shows "Local REST API is installed"

**Recommended (enhances functionality):**
3. **Smart Connections** - Semantic search and AI linking
   - Required for `/semantic-search` command
   - Enables AI-powered note connections

4. **Templater** - Advanced template system
   - Required for advanced `/capture` templates
   - Enables dynamic note generation

5. **Terminal** - Integrated terminal in Obsidian (by polyipseity)
   - Run Claude Code in side panel
   - Creates unified workspace (edit notes + run AI commands)
   - No context switching between apps
   - Configuration: Set shell path in plugin settings, then run `claude` in terminal

**Verification Checklist:**
```bash
# Settings → Community plugins → Installed plugins
# Should see:
✅ MCP Tools (enabled)
✅ Local REST API (enabled)
✅ Smart Connections (enabled) - recommended
✅ Templater (enabled) - recommended
✅ Terminal (enabled) - recommended for integrated UI
```

### Configure Image Folder (REQUIRED)

**Settings → Files & Links**
```bash
# Default location for new attachments: "In the folder specified below"
# Attachment folder path: images

# Alternative (CLI):
mkdir -p "$VAULT_PATH/images"
jq '.attachmentFolderPath = "images"' "$VAULT_PATH/.obsidian/app.json" > "$VAULT_PATH/.obsidian/app.json.tmp" && mv "$VAULT_PATH/.obsidian/app.json.tmp" "$VAULT_PATH/.obsidian/app.json"

# Verify:
cat "$VAULT_PATH/.obsidian/app.json" | jq '.attachmentFolderPath'
# Should output: "images"
```

**Why:** Ensures images are organized and `/publish` command works correctly.

---

## 3. Retrieve Local REST API Key (CRITICAL)

**IMPORTANT**: Get this API key BEFORE installing Docker MCP servers. The Obsidian MCP server requires this key for configuration.

**Method 1: Via Obsidian UI**
```bash
# Settings → Community plugins → Local REST API → Copy API Key
# Example: 0de5ba07c38bd3250722a4978d463ab886dd507607482628794be7f8ee2bd66a
```

**Method 2: Via Command Line**
```bash
VAULT_PATH="$HOME/Documents/Obsidian/KnowledgeFactory"
cat "$VAULT_PATH/.obsidian/plugins/obsidian-local-rest-api/data.json" | jq -r '.apiKey'
```

**Save this key** - you'll use it in Step 4 to configure the Docker MCP Obsidian server.

---

## 4. MCP Infrastructure

**Now that you have the Obsidian Local REST API key**, install and configure MCP servers.

### Install MCP Servers (Docker Desktop)
**Docker Desktop → MCP Toolkit → Catalog**

```
Required (4):
├── GitHub Official      [API key required]
├── YouTube Transcripts  [no key]
├── Firecrawl           [API key required]
└── Obsidian            [Requires API key from Step 3]

Recommended (2):
├── Context7            [no key]
└── Fetch (Reference)   [no key]

Optional (2):
├── Memory (Reference)  [no key]
└── Perplexity         [API key required]
```

### Configure API Keys

**GitHub:**
```bash
# Generate: github.com/settings/tokens
# Scopes: repo, public_repo
# Docker Desktop → MCP Toolkit → My servers → GitHub Official → Paste token
```

**Firecrawl:**
```bash
# Get key: firecrawl.dev (1000 free/month)
# Docker Desktop → MCP Toolkit → My servers → Firecrawl → Paste key
```

**Obsidian (Use API key from Step 3):**
```bash
# Docker Desktop → MCP Toolkit → My servers → Obsidian
# Add environment variable:
#   Key: OBSIDIAN_LOCAL_REST_API_KEY
#   Value: [paste API key from Step 3]
# Click Save → Restart server
```

**Perplexity (Optional):**
```bash
# Get key: perplexity.ai/settings/api
# Docker Desktop → MCP Toolkit → My servers → Perplexity → Paste key
```

### Connect Claude Code
```bash
# Docker Desktop → MCP Toolkit → Clients → Claude Code → Connect
# Verify: All servers show green status
```

---

## 5. Two-Repository Architecture

### Private Vault (TIER 1)

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

```bash
cd ~/Documents
git clone https://github.com/ZorroCheng-MC/sharehub.git
cd sharehub

rm -rf .git
git init
git add .
git commit -m "Initial commit: My document portal"

# Create your public GitHub repo
gh repo create my-sharehub --public --source=. --push

# Enable GitHub Pages
gh repo edit --enable-pages --pages-branch main

# Configure Jekyll baseurl
# Edit _config.yml: baseurl: "/my-sharehub"
git add _config.yml
git commit -m "Configure Jekyll baseurl"
git push
```

**Verify deployment:**
```bash
# Wait ~60 seconds, then visit:
# https://YOUR_USERNAME.github.io/my-sharehub
```

---

## 6. Install obsidian-vault-manager Plugin

```bash
# Clone plugin repository
cd ~/.claude/skills
git clone https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin.git obsidian-vault-manager

# Verify installation
ls -la ~/.claude/skills/obsidian-vault-manager
```

---

## 7. Run Setup Wizard

```bash
cd ~/Documents/Obsidian/KnowledgeFactory
claude

# In Claude Code CLI:
/setup
```

**Provide when prompted:**
```
Vault path:       ~/Documents/Obsidian/KnowledgeFactory (auto-detected)
Sharehub path:    ~/Documents/sharehub
Sharehub repo:    YOUR_USERNAME/my-sharehub
GitHub Pages URL: https://YOUR_USERNAME.github.io/my-sharehub
```

**Creates:**
- `.claude/config.sh` - Path configuration
- `.claude/settings.local.json` - Claude Code settings

---

## 8. Verification

### Test Commands

```bash
# Capture quick note
/capture Build a Chrome extension that auto-captures to Obsidian

# Create idea with AI tagging
/idea Knowledge graph visualization for Obsidian

# Get YouTube transcript
/youtube-note https://youtube.com/watch?v=VIDEO_ID

# Semantic search vault
/semantic-search "machine learning workflows"

# Analyze git repository
/gitingest https://github.com/user/repo

# Publish note to sharehub
/publish my-first-note.md
```

### Verify MCP Servers

```bash
# In Claude Code, all MCP tools should be available:
mcp__github__*
mcp__obsidian-mcp-tools__*
mcp__fetch__fetch
mcp__gitingest__*
```

### Check Paths

```bash
cat ~/.claude/skills/obsidian-vault-manager/.claude/config.sh
```

Should show:
```bash
VAULT_PATH="/Users/YOU/Documents/Obsidian/KnowledgeFactory"
SHAREHUB_PATH="/Users/YOU/Documents/sharehub"
SHAREHUB_REPO="YOUR_USERNAME/my-sharehub"
GITHUB_PAGES_URL="https://YOUR_USERNAME.github.io/my-sharehub"
```

---

## 9. Three-Tier Publishing

### TIER 1: Private (Never Publish)
```bash
# Simply don't run /publish command
# Note stays in vault forever
```

### TIER 2: Trusted Circle (Password Protected)
```yaml
---
title: "Internal Team Strategy"
access: private
---
Your content here...
```
```bash
/publish internal-strategy.md
# Requires password "maco" to view
```

### TIER 3: Public (No Password)
```yaml
---
title: "How to Build a Knowledge Base"
---
Your content here...
```
```bash
/publish knowledge-base-guide.md
# Anyone can read
```

---

## Troubleshooting

### MCP Servers Not Working
```bash
# Docker Desktop → MCP Toolkit → My servers
# Check all servers show green status
# Restart Docker Desktop if needed
```

### `/publish` Command Fails
```bash
# Verify config exists
cat ~/.claude/skills/obsidian-vault-manager/.claude/config.sh

# Check sharehub is git repository
cd ~/Documents/sharehub && git status

# Verify GitHub remote
git remote -v
```

### Obsidian MCP Tools Not Working
```bash
# Settings → Community plugins → MCP Tools
# Verify "Install Server" was clicked
# Check Local REST API is running
```

### API Rate Limits
```bash
# GitHub: 5000 requests/hour (authenticated)
# Firecrawl: 1000 requests/month (free tier)
# Perplexity: Depends on plan
```

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
- **Obsidian**: [obsidian.md](https://obsidian.md/)
- **Docker Desktop**: [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
- **GitHub Tokens**: [github.com/settings/tokens](https://github.com/settings/tokens)
- **Firecrawl API**: [firecrawl.dev](https://www.firecrawl.dev/)
- **Sharehub Template**: [github.com/ZorroCheng-MC/sharehub](https://github.com/ZorroCheng-MC/sharehub)
- **Plugin Repo**: [github.com/ZorroCheng-MC/obsidian-vault-manager-plugin](https://github.com/ZorroCheng-MC/obsidian-vault-manager-plugin)

---

## Cost Breakdown

| Item | Cost | Required? |
|------|------|-----------|
| Claude Pro | $20/mo | ✅ Yes (or Max) |
| Claude Max | $50/mo | Alternative |
| Docker Desktop | Free | ✅ Yes |
| Obsidian | Free | ✅ Yes |
| GitHub | Free | ✅ Yes (free tier) |
| Firecrawl | Free | ✅ Yes (1000/mo) |
| Perplexity | Varies | ❌ Optional |

**Total minimum: $20/mo (Claude Pro only)**

---

*Setup time: ~20 minutes for advanced users | Full setup: ~60 minutes for beginners*
