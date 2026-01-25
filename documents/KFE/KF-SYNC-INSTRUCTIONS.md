---
title: "KnowledgeFactory Sync Instructions"
tags: [product, development, GitHub, KnowledgeFactory, setup, instructions]
date: 2026-01-25
type: reference
status: active
priority: high
---

# KnowledgeFactory Sync Instructions

> How to sync the KnowledgeFactory setup to another Mac after the kf-claude migration.

**Document Version**: 1.1
**Last Updated**: 2026-01-25

---

## Prerequisites

Before syncing, ensure you have:

- [ ] macOS with Homebrew installed
- [ ] Git configured with GitHub access
- [ ] Claude Code CLI installed (`npm install -g @anthropic-ai/claude-code`)
- [ ] Obsidian installed

---

## Overview

The KnowledgeFactory setup consists of four repositories:

| Repository           | Purpose                              | Location                                     |
| -------------------- | ------------------------------------ | -------------------------------------------- |
| **mymatrix** (Vault) | Obsidian vault with notes & commands | `~/Documents/Obsidian/Claudecode`            |
| **kf-claude**        | Claude Code plugin with templates    | `~/.claude/plugins/marketplaces/kf-claude`   |
| **doublecopy**       | Desktop clipboard capture app        | `~/Documents/Obsidian/Claudecode/doublecopy` |
| **sharehub**         | GitHub Pages for publishing          | `~/Dev/sharehub`                             |

> **Note**: The `doublecopy` folder in the vault is a **separate git repository** (nested). Changes to DoubleCopy must be pushed to its own remote.

---

## Step 1: Sync the Vault (mymatrix)

```bash
# Navigate to your vault
cd ~/Documents/Obsidian/Claudecode

# Stash any local changes
git stash

# Pull latest changes
git pull --rebase origin main

# Restore stashed changes
git stash pop
```

### Handling Conflicts

If you see merge conflicts:

```bash
# Check which files have conflicts
git status

# Edit each conflicted file, look for:
# <<<<<<< HEAD
# (your changes)
# =======
# (remote changes)
# >>>>>>> origin/main

# After resolving, mark as resolved
git add <resolved-file>

# Continue the rebase
git rebase --continue
```

### What You Get After Pull

```
.claude/
├── commands/           # Short commands (synced)
│   ├── capture.md
│   ├── youtube-note.md
│   ├── idea.md
│   ├── gitingest.md
│   ├── study-guide.md
│   ├── publish.md
│   ├── semantic-search.md
│   └── share.md
└── config.local.json   # Vault configuration

KFE/                    # Migration planning docs
├── KF-MASTER-PLAN.md
├── KF-MIGRATION-CHECKLIST.md
├── KF-GITHUB-STRUCTURE.md
├── KF-README-TEMPLATES.md
└── KF-LANDING-PAGE-SPEC.md

doublecopy/             # Desktop capture app
└── process-prs.sh      # Updated for kf-claude
```

---

## Step 2: Install kf-claude Plugin

The plugin contains templates and scripts that the short commands depend on.

```bash
# Create plugin directory if needed
mkdir -p ~/.claude/plugins/marketplaces

# Clone kf-claude plugin
cd ~/.claude/plugins/marketplaces
git clone https://github.com/ZorroCheng-MC/kf-claude.git

# Verify installation
ls kf-claude/commands/
```

### Plugin Structure

```
~/.claude/plugins/marketplaces/kf-claude/
├── .claude-plugin/
│   └── marketplace.json    # Plugin registration
├── commands/               # Command definitions
│   ├── capture.md
│   ├── youtube-note.md
│   ├── idea.md
│   ├── gitingest.md
│   ├── study-guide.md
│   ├── publish.md
│   ├── semantic-search.md
│   ├── share.md
│   ├── setup.md
│   └── bulk-auto-tag.md
└── kf-claude/
    ├── templates/          # Note templates
    └── scripts/core/       # Shell scripts
```

---

## Step 3: Update Vault Configuration

Check if the config file exists and has correct paths:

```bash
cd ~/Documents/Obsidian/Claudecode
cat .claude/config.local.json
```

Expected content:

```json
{
  "vault_path": "/Users/YOUR_USERNAME/Documents/Obsidian/Claudecode",
  "sharehub_url": "https://zorrocheng-mc.github.io/sharehub",
  "sharehub_repo": "~/Dev/sharehub",
  "enable_short_commands": true
}
```

### If Config Needs Updating

Edit the file to match your Mac's paths:

```bash
# Update vault_path with your actual username
nano .claude/config.local.json
```

Or run the setup wizard:

```bash
cd ~/Documents/Obsidian/Claudecode
claude
# Then run:
/kf-claude:setup
```

---

## Step 4: Sync DoubleCopy (Nested Repository)

The `doublecopy` folder inside the vault is a **separate git repository** with its own remote.

```bash
cd ~/Documents/Obsidian/Claudecode/doublecopy

# Check current status
git status
git remote -v
```

### First Time Setup (Clone)

If the doublecopy folder exists but isn't a git repo:

```bash
cd ~/Documents/Obsidian/Claudecode

# Remove the folder (it came from vault pull)
rm -rf doublecopy

# Clone the DoubleCopy repo
git clone https://github.com/ZorroCheng-MC/doublecopy.git
```

### Sync Existing DoubleCopy

```bash
cd ~/Documents/Obsidian/Claudecode/doublecopy

# Stash local changes if any
git stash

# Pull latest
git pull --rebase origin main

# Restore stashed changes
git stash pop
```

### Recent Changes to Sync

| File | Change |
|------|--------|
| `process-prs.sh` | Updated to use `/kf-claude:capture` |
| `DoubleCopyApp/main.swift` | v1.2.0 with kf-claude integration |
| `CLAUDE.md` | Updated documentation |

### Push Local Changes (If You Made Changes)

```bash
cd ~/Documents/Obsidian/Claudecode/doublecopy

git add -A
git commit -m "Your commit message"
git push origin main
```

---

## Step 5: Clone/Sync Sharehub (for Publishing)

```bash
# If sharehub doesn't exist
mkdir -p ~/Dev
cd ~/Dev
git clone https://github.com/ZorroCheng-MC/sharehub.git

# If it exists, just pull
cd ~/Dev/sharehub
git pull origin main
```

---

## Step 6: Install Dependencies

```bash
# Required
brew install jq git

# Optional (for YouTube transcripts)
brew install uv
```

---

## Step 7: Verify Everything Works

### Test Short Commands

```bash
cd ~/Documents/Obsidian/Claudecode
claude

# Test idea capture
/idea Test sync from new Mac

# Test semantic search (requires Obsidian running with Local REST API)
/semantic-search KnowledgeFactory
```

### Test Plugin Commands

```bash
# These should also work
/kf-claude:capture Test plugin command
/kf-claude:publish KFE/KF-MASTER-PLAN.md
```

### Available Commands

| Short Command | Plugin Command | Purpose |
|---------------|----------------|---------|
| `/capture` | `/kf-claude:capture` | Universal content capture |
| `/youtube-note` | `/kf-claude:youtube-note` | YouTube video with transcript |
| `/idea` | `/kf-claude:idea` | Quick idea capture |
| `/gitingest` | `/kf-claude:gitingest` | GitHub repository analysis |
| `/study-guide` | `/kf-claude:study-guide` | Generate study materials |
| `/publish` | `/kf-claude:publish` | Publish to GitHub Pages |
| `/share` | `/kf-claude:share` | Generate shareable URL |
| `/semantic-search` | `/kf-claude:semantic-search` | Search vault by meaning |
| - | `/kf-claude:setup` | Configuration wizard |

---

## Step 8: Build/Install DoubleCopy (Optional)

After syncing DoubleCopy in Step 4, build or download the app:

### Build from Source

```bash
cd ~/Documents/Obsidian/Claudecode/doublecopy
./build-app.sh

# The built app will be in dist/
open dist/DoubleCopy.app
```

### Or Download Release

Download latest DMG from:
https://github.com/ZorroCheng-MC/doublecopy/releases

---

## Ongoing Sync Workflow

### Daily Sync (Each Mac)

```bash
cd ~/Documents/Obsidian/Claudecode

# Before starting work
git pull --rebase origin main

# After making changes
git add -A
git commit -m "Your commit message"
git push origin main
```

### Sync DoubleCopy (Nested Repo)

```bash
cd ~/Documents/Obsidian/Claudecode/doublecopy

# Pull latest
git pull --rebase origin main

# If you made changes, push them
git add -A
git commit -m "Your commit message"
git push origin main
```

### Sync kf-claude Plugin Updates

```bash
cd ~/.claude/plugins/marketplaces/kf-claude
git pull origin master
```

### Sync Sharehub (After Publishing)

```bash
cd ~/Dev/sharehub
git pull origin main
```

---

## Troubleshooting

### "Command not found" for short commands

1. Verify commands exist in vault:
   ```bash
   ls .claude/commands/
   ```

2. Restart Claude Code to reload commands

### "Template not found" errors

1. Verify kf-claude plugin is installed:
   ```bash
   ls ~/.claude/plugins/marketplaces/kf-claude/
   ```

2. Re-clone if missing:
   ```bash
   cd ~/.claude/plugins/marketplaces
   git clone https://github.com/ZorroCheng-MC/kf-claude.git
   ```

### Publish fails with "sharehub_repo not configured"

1. Check config:
   ```bash
   cat .claude/config.local.json
   ```

2. Run setup to fix:
   ```bash
   /kf-claude:setup
   ```

### Semantic search fails

1. Ensure Obsidian is running
2. Verify Local REST API plugin is installed and enabled
3. Check API key exists:
   ```bash
   cat .obsidian/plugins/obsidian-local-rest-api/data.json | jq '.apiKey'
   ```

### Git conflicts on notes

For note content conflicts, usually keep the newer version:

```bash
# Accept remote version
git checkout --theirs path/to/file.md

# Or accept local version
git checkout --ours path/to/file.md

# Then continue
git add path/to/file.md
git rebase --continue
```

---

## Quick Reference

### Repository URLs

| Repo | URL |
|------|-----|
| Vault | https://github.com/ZorroCheng-MC/mymatrix |
| kf-claude | https://github.com/ZorroCheng-MC/kf-claude |
| DoubleCopy | https://github.com/ZorroCheng-MC/doublecopy |
| Sharehub | https://github.com/ZorroCheng-MC/sharehub |

### Key Paths

| Item | Path |
|------|------|
| Vault | `~/Documents/Obsidian/Claudecode` |
| Plugin | `~/.claude/plugins/marketplaces/kf-claude` |
| DoubleCopy | `~/Documents/Obsidian/Claudecode/doublecopy` (nested repo) |
| Sharehub | `~/Dev/sharehub` |
| Config | `{vault}/.claude/config.local.json` |
| Commands | `{vault}/.claude/commands/` |

### Useful Commands

```bash
# Check git status across all repos
cd ~/Documents/Obsidian/Claudecode && git status
cd ~/Documents/Obsidian/Claudecode/doublecopy && git status
cd ~/.claude/plugins/marketplaces/kf-claude && git status
cd ~/Dev/sharehub && git status

# View recent commits
git log --oneline -5

# Check unpushed commits
git log origin/main..HEAD --oneline
```

---

## Related Documents

- [[KF-MASTER-PLAN]] - Strategic overview
- [[KF-MIGRATION-CHECKLIST]] - Migration progress tracking
- [[KF-GITHUB-STRUCTURE]] - Repository structure details

---

*KnowledgeFactory Sync Instructions v1.1 | 2026-01-25*
