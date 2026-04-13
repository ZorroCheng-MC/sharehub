---
title: "How kf-cli Captures Knowledge from YouTube, GitHub and the Web into Your Obsidian Vault"
date: 2026-04-13
type: article
status: draft
tags: [article, kf, obsidian, ai-tools]
summary: "kf-cli is a native CLI toolkit that captures knowledge from YouTube videos, GitHub repositories, and web articles directly into your Obsidian vault — no Docker or MCP required."
hero: images/kf-cli-knowledge-capture-obsidian-hero.jpg
created: 2026-04-13
---

# How kf-cli Captures Knowledge from YouTube, GitHub and the Web into Your Obsidian Vault

![](/images/kf-cli-knowledge-capture-obsidian-hero.jpg)

## The Problem: Knowledge Lives Everywhere

You watch a YouTube tutorial. You discover a GitHub repo. You read a blog post. Each one contains something worth keeping — but getting it into your knowledge base means copy-pasting, reformatting, tagging, and filing. Manually. Every time.

kf-cli solves this. It captures structured knowledge from any source and drops it directly into your Obsidian vault — formatted, tagged, and ready to link.

## What kf-cli Is

kf-cli is a Claude Code plugin that replaces the older Docker/MCP-based kf-claude with native CLI tools. No containers, no MCP overhead — just `yt-dlp`, `gh`, `curl`, and Claude doing the heavy lifting.

It runs entirely inside Claude Code via slash commands:

```
/kf-cli:capture    — smart router, detects input type automatically
/kf-cli:youtube-note  — YouTube video with full transcript
/kf-cli:gitingest     — GitHub repo analysis
/kf-cli:study-guide   — web article → structured study guide
/kf-cli:idea          — quick idea capture
/kf-cli:article       — long-form article with AI hero image
```

## How Each Capture Works

### YouTube Videos → `/kf-cli:youtube-note`

Drop in a YouTube URL. kf-cli:
1. Fetches video metadata via `yt-dlp` (title, channel, duration, thumbnail)
2. Pulls the full transcript via `uvx youtube-transcript-api`
3. Sends transcript to Claude for analysis — learning objectives, key insights, actionable points
4. Writes a structured note with proper frontmatter tags

The resulting note includes the thumbnail, a curriculum outline, main insights, and suggested related searches — all in under 30 seconds.

### GitHub Repos → `/kf-cli:gitingest`

Drop in a GitHub URL. kf-cli:
1. Fetches repo metadata via `gh api` (stars, language, description, commits)
2. Pulls the file tree and README
3. Claude analyses the architecture and key files
4. Writes a repository analysis note with tech stack, key files, and semantic search suggestions

### Web Articles → `/kf-cli:study-guide`

Drop in any URL. kf-cli:
1. Fetches the page via `WebFetch`
2. Claude extracts the core concepts and structures them as a study guide
3. Writes a note with key takeaways, quiz questions, and further reading

### Smart Routing → `/kf-cli:capture`

Don't want to think about which command to use? `/kf-cli:capture` detects the input type automatically:

| Input | Routes to |
|-------|-----------|
| `youtube.com/watch?v=...` | `youtube-note` |
| `github.com/owner/repo` | `gitingest` |
| Any other URL | `study-guide` |
| Plain text | `idea` |

## Where Notes Go

Every captured note lands in `notes/YYYY-MM-DD-slug.md` with:
- Frontmatter tags mapped to wiki topics (`kf`, `claude-code`, `obsidian`, etc.)
- Wikilinks to related notes
- Canonical topic tags that route it to the correct `wiki/[topic]/` file

The wiki is rebuilt with `python3 scripts/compile_wiki.py` — which reads all frontmatter tags and regenerates the topic indexes automatically.

## Why Native CLI vs Docker/MCP

| | kf-claude (old) | kf-cli (new) |
|---|---|---|
| File access | MCP Docker tool | Direct `Write(*)` |
| YouTube | MCP tool | `yt-dlp` |
| GitHub | MCP GitHub tools | `gh api` |
| Web fetch | `firecrawl_scrape` | `WebFetch` |
| Cold start | 2-5s Docker | 0s |
| Speed | ~500ms per op | ~1ms local I/O |

## Key Takeaways

- kf-cli turns any URL or idea into a structured Obsidian note in seconds
- Smart routing via `/kf-cli:capture` means you never choose the wrong command
- All notes use canonical frontmatter tags — no body hashtags, no routing failures
- Native CLI tools make it 100-500x faster than the Docker-based predecessor
- Publish any note instantly to GitHub Pages with `/kf-cli:publish`

---

*Created: 2026-04-13*
