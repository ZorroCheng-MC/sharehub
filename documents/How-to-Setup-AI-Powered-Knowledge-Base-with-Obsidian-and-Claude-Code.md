# How to Setup an AI-Powered Knowledge Base with Obsidian and Claude Code

A comprehensive guide to building an intelligent, automated knowledge management system that captures, analyzes, and publishes content to GitHub Pages.

> **🚀 Quick Start**: Fork the source repository at https://github.com/ZorroCheng-MC/sharehub to get started with a pre-configured setup!

## Overview

This setup combines Obsidian's powerful note-taking capabilities with Claude Code's AI assistance and GitHub Pages for public sharing. The system can automatically:

- Capture content from YouTube videos, GitHub repositories, web pages, and ideas
- Analyze and structure content using AI
- Organize notes with semantic search and tagging
- Publish selected content to GitHub Pages automatically

## Architecture

```
┌─────────────────────┐
│   Obsidian Vault    │
│   (Your Content)    │
└──────────┬──────────┘
           │
           ├─► Claude Code (AI Assistant)
           │   ├─► Custom Commands (/capture, /youtube-note, etc.)
           │   ├─► MCP Servers (obsidian, git, github, etc.)
           │   ├─► Hooks & Automation
           │   └─► Skills & Subagents
           │
           ├─► Smart Connections (Semantic Search)
           │   └─► Local REST API
           │
           └─► Git Repository
               └─► GitHub Actions
                   ├─► Auto Front Matter
                   └─► Jekyll Publishing
                       └─► GitHub Pages
```

## Prerequisites

- **macOS, Linux, or Windows** (this guide assumes macOS/Linux)
- **Obsidian** (latest version)
- **Claude Code CLI** installed
- **Git** installed
- **GitHub Account**
- **Node.js** and **npm** (for some MCP servers)
- **Python 3** (for youtube_transcript_api)

## Part 1: Obsidian Setup

### 1.1 Install Required Plugins

Install these Obsidian community plugins:

1. **Smart Connections** - AI-powered semantic search
2. **Obsidian Local REST API** - Enables external tool access
3. **Obsidian Git** - Git integration for auto-sync
4. **Templater** - Advanced template support
5. **MCP Tools** - Model Context Protocol integration
6. **Terminal** - Run commands from Obsidian

### 1.2 Configure Obsidian Local REST API

1. Install the plugin from Community Plugins
2. Enable it in Settings → Community Plugins
3. Generate an API key in plugin settings
4. Create `.env` file in your vault root:

```bash
LOCAL_REST_API_KEY=your_generated_api_key_here
```

### 1.3 Configure Smart Connections

1. Install Smart Connections plugin
2. Enable in Settings → Community Plugins
3. Configure:
   - Model: Choose your preferred embedding model
   - Search settings: Adjust limit and threshold
   - Enable "Show in sidebar" for quick access

### 1.4 Vault Structure

Organize your vault with this structure:

```
YourVault/
├── .claude/
│   ├── commands/
│   │   ├── capture.md
│   │   ├── idea.md
│   │   ├── youtube-note.md
│   │   ├── gitingest.md
│   │   ├── study-guide.md
│   │   └── semantic-search.md
│   └── agents/
│       ├── design-review-agent.md
│       └── test-executor.md
├── .obsidian/
│   └── plugins/
├── .env
└── (your content here)
```

## Part 2: Claude Code Configuration

### 2.1 Install Claude Code

```bash
# Install via npm (if not already installed)
npm install -g @anthropic-ai/claude-code

# Or via official installer
# Download from https://claude.ai/code
```

### 2.2 Configure Claude Settings

Create `~/.claude/settings.json`:

```json
{
  "env": {},
  "permissions": {
    "allow": [
      "WebFetch, Bash"
    ],
    "deny": []
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "echo '[RULE CHECK] Verifying file creation...' && true"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "echo '[RULE REMINDER] File operation completed.'"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo '[SESSION COMPLETE]'"
          },
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Glass.aiff 2>/dev/null || echo -e '\\a' || true"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Glass.aiff"
          }
        ]
      }
    ]
  },
  "statusLine": {
    "type": "command",
    "command": "input=$(cat); cwd=$(echo \"$input\" | jq -r '.workspace.current_dir'); model=$(echo \"$input\" | jq -r '.model.display_name'); printf \"\\033[2m%s | %s\\033[0m\" \"$(basename \"$cwd\")\" \"$model\""
  },
  "alwaysThinkingEnabled": true
}
```

### 2.3 Create Custom Commands

#### Command: `/capture` - Smart Content Capture

Create `.claude/commands/capture.md`:

```markdown
---
description: Smart capture command with intelligent routing
argument-hint: [content to capture]
allowed-tools:
  - Write(*)
  - Read(*)
  - Bash(date:*)
---

## Context

- **Today's Date:** !`date "+%Y-%m-%d"`
- **User Input:** `$ARGUMENTS`

## Your Task

Route capture to appropriate command based on content type:

- **Ideas** → /idea
- **YouTube URLs** → /youtube-note  
- **Git repos** → /gitingest
- **Study content** → /study-guide
- **Default** → Create general note

Process accordingly and save to vault root.
```

#### Command: `/idea` - Quick Idea Capture

Create `.claude/commands/idea.md`:

```markdown
---
allowed-tools: Write(*), Bash(date +%Y-%m-%d)
description: Create an idea file and add to the base
---

# Context
The today's date is `!date +%Y-%m-%d`

# Task
1. Give the 3-5 words file name for the idea {idea-name}.
2. Create file `{date}-{idea-name}.md` with:

\```markdown
---
tags: [idea]
date: {date}
---
 
# Idea
{date}
{cleaned-idea}
\```
```

#### Command: `/youtube-note` - YouTube Video Analysis

Create `.claude/commands/youtube-note.md`:

```markdown
---
description: Fetch YouTube video transcript and create comprehensive material entry
argument-hint: [youtube-url-or-video-id]
allowed-tools:
  - Bash(uvx:*)
  - Write(*)
  - Read(*)
  - Bash(date:*)
---

## Context

- **Today's Date:** !`date "+%Y-%m-%d"`
- **YouTube URL/ID:** `$ARGUMENTS`

## Your Task

1. Extract video ID from URL
2. Fetch transcript: `uvx youtube_transcript_api VIDEO_ID`
3. Create structured note with:
   - Title and description
   - Thumbnail
   - Learning objectives
   - Curriculum/contents
   - Key takeaways section

Format: `[creator-name]-[descriptive-title].md`
```

#### Command: `/gitingest` - Repository Analysis

Create `.claude/commands/gitingest.md`:

```markdown
---
description: Analyze GitHub repositories efficiently using GitIngest
argument-hint: [repository-url] [focus:code|docs|config|all]
allowed-tools:
  - Bash(gitingest:*)
  - Write(*)
  - Read(*)
---

## Task

Use GitIngest MCP server to analyze repository:

1. Validate gitingest installation
2. Parse arguments (repo URL, focus type, max size)
3. Execute gitingest with smart filtering
4. Create analysis document

Focus modes:
- **code**: Source files only
- **docs**: Documentation only
- **config**: Configuration files
- **all**: Everything
```

#### Command: `/study-guide` - Learning Material Creator

Create `.claude/commands/study-guide.md`:

```markdown
---
description: Generate comprehensive study guide from content
argument-hint: [content-source]
allowed-tools:
  - Read(*)
  - Write(*)
  - Bash(curl:*)
---

## Task

Transform any content into structured study guide:

1. Ingest content (file/URL/text)
2. Analyze topics and complexity
3. Generate study guide with:
   - Learning objectives
   - Core content structure
   - Study strategies
   - Self-assessment
   - Additional resources

Format: `{date}-{topic-name}-study-guide.md`
```

#### Command: `/semantic-search` - Smart Vault Search

Create `.claude/commands/semantic-search.md`:

```markdown
---
description: Perform semantic search in Obsidian vault
allowed-tools:
  - Bash(curl:*)
---

## Task

Search vault using Smart Connections API:

\```bash
curl -k -X POST \\
  https://127.0.0.1:27124/search/smart \\
  -H "Authorization: Bearer $(grep LOCAL_REST_API_KEY .env | cut -d'=' -f2)" \\
  -H "Content-Type: text/plain" \\
  -d "{\"query\": \"$ARGUMENTS\", \"filter\": {\"limit\": 5}}" | jq .
\```
```

### 2.4 Configure MCP Servers

MCP servers extend Claude's capabilities. Key servers for this setup:

#### Essential MCP Servers:

1. **obsidian-mcp-tools** - Obsidian integration
2. **github** - GitHub API access
3. **git** - Git operations
4. **fetch** - Web content fetching
5. **gitingest** - Repository analysis
6. **sqlite** - Local database (optional)

Configure in your Claude desktop app or via environment.

### 2.5 Setup Hooks

Hooks automate workflows. Configure in `~/.claude/settings.json`:

- **PreToolUse**: Validate operations before execution
- **PostToolUse**: Confirm operations after completion
- **Stop**: Cleanup and notifications on session end
- **Notification**: Audio/visual feedback

## Part 3: GitHub Setup

### 3.1 Create GitHub Repository

```bash
# Create new repository on GitHub
# Name it something like: knowledge-base or sharehub

# Clone to your machine
git clone https://github.com/yourusername/your-repo.git ~/path/to/repo
```

### 3.2 Setup GitHub Pages

1. Go to repository **Settings** → **Pages**
2. Source: **GitHub Actions**
3. Select **Jekyll** as the static site generator

### 3.3 Configure Repository Settings

1. **Enable Actions**: Settings → Actions → General → Allow all actions
2. **Generate Personal Access Token** (if needed):
   - Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Generate new token with `repo` and `workflow` scopes
   - Save token securely

### 3.4 Create GitHub Actions Workflows

#### Workflow 1: Jekyll Deployment

Create `.github/workflows/jekyll.yml`:

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true
      
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### Workflow 2: Auto Front Matter

Create `.github/workflows/add-frontmatter.yml`:

```yaml
name: Auto-Add Front Matter

on:
  push:
    paths:
      - '**/*.md'
  pull_request:
    paths:
      - '**/*.md'

jobs:
  add-frontmatter:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          fetch-depth: 0

      - name: Check and add front matter
        run: |
          #!/bin/bash
          modified_files=()
          
          while IFS= read -r -d '' file; do
            if ! head -1 "$file" | grep -q "^---$"; then
              echo "📝 Adding front matter to: $file"
              
              title=$(grep -m 1 "^# " "$file" | sed 's/^# //')
              if [ -z "$title" ]; then
                title=$(basename "$file" .md | sed 's/[-_]/ /g')
              fi
              
              {
                echo "---"
                echo "title: \"$title\""
                echo "---"
                echo ""
                cat "$file"
              } > "$file.tmp"
              
              mv "$file.tmp" "$file"
              modified_files+=("$file")
            fi
          done < <(find . -name "*.md" -type f -print0)
          
          echo "modified=${#modified_files[@]}" >> $GITHUB_ENV

      - name: Commit and push if changes
        if: env.modified != '0'
        run: |
          git config --local user.email "github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"
          git add .
          git commit -m "🤖 Auto-add front matter"
          git push
```

### 3.5 Jekyll Configuration

Create `_config.yml` in repository root:

```yaml
title: Your Knowledge Base
description: AI-powered knowledge management system
baseurl: ""
url: "https://yourusername.github.io"

theme: minima  # or your preferred theme

markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge

plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap

exclude:
  - .obsidian/
  - .claude/
  - .env
  - node_modules/
  - README.md
```

Create `Gemfile`:

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.3"
gem "minima", "~> 2.5"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag"
  gem "jekyll-sitemap"
end
```

## Part 4: Integration & Workflow

### 4.1 Mount Repository as Obsidian Vault

```bash
# Option 1: Open existing folder as vault
# In Obsidian: Open folder as vault → Select your git repository

# Option 2: Create symbolic link
ln -s ~/path/to/git-repo ~/Documents/Obsidian/YourVaultName
```

### 4.2 Daily Workflow

1. **Capture Content**:
   ```bash
   # In Claude Code (in your vault directory)
   /capture https://youtube.com/watch?v=VIDEO_ID
   /capture "Quick idea about AI workflows"
   /capture https://github.com/user/repo
   ```

2. **Organize & Review**:
   - Use Smart Connections to find related notes
   - Tag and categorize using AI assistance
   - Review in Obsidian graph view

3. **Publish Selected Content**:
   ```bash
   # Move files you want to publish
   mv draft-note.md documents/public/
   
   # Commit and push
   git add .
   git commit -m "Add new article"
   git push origin main
   ```

4. **Auto-Publishing**:
   - GitHub Actions automatically adds front matter
   - Jekyll builds and deploys to GitHub Pages
   - Content live at `https://yourusername.github.io`

### 4.3 AI-Assisted Content Creation

Use Claude Code interactively:

```bash
# Start Claude in your vault
cd ~/path/to/vault
claude

# Example interactions:
> Create a study guide from this URL: https://example.com/article
> Analyze this repository and create summary: https://github.com/user/repo
> Search my notes for information about "machine learning"
> Create an idea note about "automated workflows"
```

### 4.4 Semantic Search Integration

```bash
# Search your vault semantically
/semantic-search "How to setup CI/CD"

# Results show most relevant notes based on meaning, not just keywords
```

## Part 5: Advanced Features

### 5.1 Custom Skills

Create custom skills in `~/.claude/skills/`:

```markdown
# my-custom-skill.md
---
description: Your custom skill
---

## Task
Define what this skill does...
```

### 5.2 Subagents

Configure specialized subagents in `.claude/agents/`:

- **design-review-agent.md** - Review UI/UX changes
- **test-executor.md** - Run and validate tests
- **content-analyzer.md** - Analyze content quality

### 5.3 Automation Scripts

Create helper scripts:

```bash
# publish.sh - Quick publish script
#!/bin/bash
echo "Publishing to GitHub Pages..."
git add .
git commit -m "Update content"
git push origin main
echo "✅ Published! Check https://yourusername.github.io"
```

### 5.4 Obsidian Templates

Use Templater for consistent note structure:

```markdown
---
title: {{title}}
date: {{date:YYYY-MM-DD}}
tags: []
---

# {{title}}

## Summary

## Details

## References
```

## Part 6: Maintenance & Best Practices

### 6.1 Regular Maintenance

- **Weekly**: Review and organize captured content
- **Monthly**: Check GitHub Actions status
- **Quarterly**: Update plugins and dependencies

### 6.2 Backup Strategy

```bash
# Automatic Git backup via Obsidian Git plugin
# Configure in plugin settings:
# - Auto-pull: every 10 minutes
# - Auto-commit: every 30 minutes
# - Auto-push: every hour
```

### 6.3 Security Considerations

- Never commit `.env` files (add to `.gitignore`)
- Use GitHub secrets for tokens
- Regularly rotate API keys
- Review public content before publishing

### 6.4 Performance Optimization

- Use `.gitignore` to exclude large files
- Configure Smart Connections to index selectively
- Optimize Jekyll build with excludes
- Use shallow git clones when possible

## Part 7: Forking & Customization

### 7.1 Fork This Setup

**Source Repository**: https://github.com/ZorroCheng-MC/sharehub

```bash
# 1. Fork the repository on GitHub
# Visit: https://github.com/ZorroCheng-MC/sharehub
# Click "Fork" button in top-right

# 2. Clone your fork
git clone https://github.com/yourusername/sharehub.git
cd sharehub

# 3. Install dependencies
bundle install  # for Jekyll
pip install youtube_transcript_api  # for YouTube transcript fetching

# 4. Configure your environment
cp .env.example .env
# Edit .env with your API keys
```

### 7.2 Customize for Your Needs

1. **Modify Commands**: Edit `.claude/commands/*.md`
2. **Adjust Workflows**: Update `.github/workflows/*.yml`
3. **Change Theme**: Edit `_config.yml`
4. **Add Features**: Create new MCP servers or skills

### 7.3 Environment Variables

Create `.env.example`:

```bash
# Copy this to .env and fill in your values
LOCAL_REST_API_KEY=your_key_here
GITHUB_TOKEN=your_token_here
OBSIDIAN_VAULT_PATH=/path/to/vault
```

## Troubleshooting

### Common Issues

**Issue**: Smart Connections not working
- **Solution**: Check Local REST API is running, verify API key in `.env`

**Issue**: GitHub Actions failing
- **Solution**: Check workflow logs, verify permissions, ensure Jekyll dependencies installed

**Issue**: Claude Code commands not found
- **Solution**: Verify `.claude/commands/` directory exists, check command frontmatter syntax

**Issue**: YouTube transcript fails
- **Solution**: Install youtube_transcript_api: `pip install youtube_transcript_api`

## Resources

### This Setup
- **Source Repository**: https://github.com/ZorroCheng-MC/sharehub (Fork this to get started!)

### Tools & Documentation
- **Obsidian**: https://obsidian.md
- **Claude Code**: https://claude.ai/code
- **Smart Connections**: https://github.com/brianpetro/obsidian-smart-connections
- **Jekyll**: https://jekyllrb.com
- **GitHub Pages**: https://pages.github.com
- **MCP Protocol**: https://modelcontextprotocol.io

## Contributing

If you improve this setup:

1. Fork the repository
2. Make your changes
3. Submit a pull request
4. Share your improvements!

## License

MIT License - Feel free to use and modify this setup for your needs.

---

**Built with** 🧠 Obsidian + 🤖 Claude Code + 🚀 GitHub Pages

*Last updated: 2025-10-22*
