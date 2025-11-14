---
title: "doublecmdc - Friction-Free Clipboard Capture"
tags: [idea, productivity, automation, knowledge-management, tools, inbox, actionable, technical]
date: 2025-11-14
type: idea
status: inbox
priority: high
---

# doublecmdc - Friction-Free Clipboard Capture

## 💡 Core Idea

**doublecmdc is the missing link between your browsing and your published knowledge.**

Double-tap Cmd+C anywhere on macOS → AI agent curates in your private vault → Single hotkey publishes to public sharehub.

This creates a complete friction-free pipeline:
- **Capture**: Double-tap Cmd+C on any content (text, links, YouTube, GitHub)
- **Curate**: AI agent automatically tags, formats, and organizes in your private vault
- **Share**: One more hotkey (e.g., Cmd+Shift+P) publishes curated note to public sharehub

**The complete workflow:**
```
Browser content → Double Cmd+C → AI processes in background →
  ↓
Private vault note (tagged, formatted, structured) →
  ↓
Cmd+Shift+P (or similar hotkey) → Published to GitHub Pages
  ↓
Shareable URL ready to send to anyone
```

## 🎯 Why This Matters: The Complete Knowledge Pipeline

Traditional knowledge management has **multiple breaking points**:

```
Traditional Workflow (5 breaking points):
1. See content → Switch to vault (FRICTION 1)
2. Create note → Manual formatting (FRICTION 2)
3. Add tags → Manual organization (FRICTION 3)
4. Save note → Context switch back (FRICTION 4)
5. Want to share → Copy to blog/Medium/docs (FRICTION 5)

Result: 90% of valuable content never captured
        Of the 10% captured, 90% never shared
        Total loss: 99% of knowledge value
```

**doublecmdc eliminates ALL friction points:**

```
doublecmdc Workflow (ZERO breaking points):
1. See content → Double Cmd+C (0 context switch)
2. AI curates → Auto-tagged, formatted (0 manual work)
3. Ready in vault → Continue browsing (0 interruption)
4. Want to share → Cmd+Shift+P (0 export work)
5. Published URL → Ready to share (0 copy/paste)

Result: 100% of valuable content captured
        100% instantly shareable
        Total value: Maximum knowledge leverage
```

### The Three-Stage Magic

**Stage 1: Friction-Free Capture (Double Cmd+C)**
- No context switching to vault
- No manual file creation
- No deciding where to save
- Works on ANY content, ANYWHERE on macOS

**Stage 2: AI-Powered Curation (Background Processing)**
- Auto-detects content type (YouTube/GitHub/article/idea)
- Routes to appropriate template
- AI generates 6-8 smart tags from taxonomy
- Formats with frontmatter and structure
- Saves to private vault
- All happens in < 3 seconds

**Stage 3: One-Command Publishing (Cmd+Shift+P)**
- Triggers `/publish` command on current note
- Copies images from vault → sharehub
- Converts image paths for web
- Git commits and pushes
- GitHub Pages deploys (60 sec)
- Shareable URL generated

**Total time: Capture (0.5s) + AI curation (3s) + Publish (5s) = 8.5 seconds**
From browser content to published URL shared with the world.

## 🏭 How It Fits Into KnowledgeFactory

doublecmdc completes the **KnowledgeFactory lifecycle automation** by adding zero-friction entry and exit points.

### The Complete 6-Stage Lifecycle with doublecmdc

```
┌─────────────────────────────────────────────────────────────────┐
│              KNOWLEDGEFACTORY WITH DOUBLECMDC                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Stage 1: 📥 CAPTURE (doublecmdc - NEW!)                       │
│    Tool: Double Cmd+C anywhere on macOS                        │
│    Input: Text, URLs, YouTube, GitHub, articles                │
│    Output: Clipboard content → AI processing queue             │
│    Time: 0.5 seconds                                            │
│    Friction: ZERO (no context switch)                          │
│                                                                 │
│  Stage 2: 🏷️ CURATE (AI automation - ENHANCED!)               │
│    Tool: Claude AI + obsidian-vault-manager templates          │
│    Process:                                                     │
│      • Content type detection (URL patterns, text analysis)    │
│      • Template routing (YouTube/GitHub/article/idea)          │
│      • AI tagging (6-8 smart tags from taxonomy)               │
│      • Frontmatter generation (YAML with metadata)             │
│      • Content formatting (markdown structure)                 │
│    Output: Organized note in private vault                     │
│    Time: 3 seconds                                              │
│    Friction: ZERO (fully automated)                            │
│                                                                 │
│  Stage 3: 🌱 NURTURE (Obsidian integration)                    │
│    Tool: Obsidian Graph View + Smart Connections               │
│    Process: Auto-linking, backlinks, knowledge graph           │
│    Output: Connected knowledge web                             │
│                                                                 │
│  Stage 4: 🔨 DEVELOP (Manual refinement)                       │
│    Tool: Obsidian editor + AI assistance                       │
│    Process: Human adds insights, structure, polish             │
│    Output: Refined knowledge asset                             │
│                                                                 │
│  Stage 5: 🔄 EVOLVE (Semantic search)                          │
│    Tool: Smart Connections / ChromaDB                          │
│    Process: Find related notes, discover patterns              │
│    Output: Enhanced understanding                              │
│                                                                 │
│  Stage 6: 🌐 SHARE (doublecmdc - NEW!)                         │
│    Tool: Cmd+Shift+P hotkey → /publish command                 │
│    Process:                                                     │
│      • Copy images vault → sharehub                            │
│      • Convert image paths for web                             │
│      • Git commit + push to sharehub repo                      │
│      • GitHub Pages deployment (60s)                           │
│      • Generate shareable URL                                  │
│    Output: Published URL ready to share                        │
│    Time: 5 seconds + 60s deploy                                │
│    Friction: ZERO (single hotkey)                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Before doublecmdc vs. After doublecmdc

**Before: Partial Automation (Stages 2-5)**
```
Manual entry → AI curation → Obsidian → Manual export → Share
    ↑                                         ↑
  FRICTION                                 FRICTION
  (5-30 sec)                              (2-5 min)
```

**After: Complete Automation (Stages 1-6)**
```
Double Cmd+C → AI curation → Obsidian → Cmd+Shift+P → Share
     ↓                                        ↓
  NO FRICTION                              NO FRICTION
  (0.5 sec)                                (5 sec)
```

### The Integration Points

**doublecmdc leverages existing KnowledgeFactory infrastructure:**

1. **Hammerspoon** (macOS automation)
   - Monitors keyboard events for double Cmd+C
   - Reads clipboard content
   - Triggers Claude Code command

2. **Claude Code** (AI processing)
   - Receives clipboard content via Hammerspoon
   - Runs `/capture` or `/youtube-note` or `/idea` based on content type
   - Uses existing MCP servers (YouTube, GitHub, Firecrawl, Obsidian)

3. **obsidian-vault-manager-plugin** (templates & automation)
   - Content type detection logic (already exists)
   - AI tagging with taxonomy (already exists)
   - Template routing (already exists)
   - `/publish` command (already exists)

4. **Sharehub** (publishing platform)
   - Two-repo architecture (vault → sharehub)
   - Image handling (copy + path conversion)
   - GitHub Pages deployment
   - Password protection support

**doublecmdc adds:**
- ✅ Global keyboard shortcut (double Cmd+C for capture)
- ✅ Background processing (no terminal required)
- ✅ Visual notifications (capture success/failure)
- ✅ Publish hotkey (Cmd+Shift+P or similar)
- ✅ Error handling (offline mode, rate limiting)

### Real-World Example: From Tweet to Published Knowledge

**Scenario**: You see a valuable tweet thread about AI architecture

**Without doublecmdc (7 steps, 5+ minutes):**
1. Copy tweet URL
2. Switch to terminal
3. Run `claude`
4. Run `/capture <paste-url>`
5. Wait for processing
6. Switch to Obsidian to verify
7. Manually export to blog/Medium
8. Format for publishing
9. Share URL

**With doublecmdc (2 gestures, 68 seconds):**
1. Select tweet URL → Double Cmd+C (0.5s)
   - *Notification: "Captured! Article note created with tags: [AI, architecture, reference]"*
2. Note appears in vault (3s AI processing)
3. Review note in Obsidian (optional)
4. Cmd+Shift+P to publish (5s + 60s deploy)
   - *Notification: "Published! https://yourname.github.io/sharehub/documents/ai-architecture-thread.html"*

**Result**: From tweet to shareable knowledge in 68 seconds, with 2 simple gestures.

## 🔗 Related Concepts

- **Capture habit formation** - Lower friction = higher adoption and consistency
- **Context preservation** - Capturing without leaving current work maintains flow state
- **Intelligent routing** - Content-aware templates (YouTube → video note, GitHub → repository analysis, plain text → idea)
- **AI-powered organization** - Auto-tagging from predefined taxonomy eliminates manual categorization
- **Background processing** - Async capture doesn't interrupt current task
- **Universal inbox pattern** - Single entry point for all knowledge types
- **Bases filtering** - Structured tags enable powerful queries and views
- **Hammerspoon automation** - macOS keyboard event monitoring and clipboard access
- **Modal interaction design** - Double-tap gesture as semantic trigger
- **Two-repo architecture** - Private vault + public sharehub separation
- **Complete lifecycle** - Entry (capture) → processing → exit (publish)

## 📝 Implementation Plan

### Phase 1: Capture Automation (Week 1-2)

**Goal**: Replace manual `/capture` with double Cmd+C hotkey

**Components:**
1. **Hammerspoon capture script** (`doublecmdc-capture.lua`)
   ```lua
   -- Monitor for double Cmd+C keyboard event
   -- Read clipboard content
   -- Detect content type (URL patterns, plain text)
   -- Trigger Claude Code command via CLI
   ```

2. **Claude Code integration**
   - Receives clipboard content as argument
   - Routes to `/capture`, `/youtube-note`, `/gitingest`, or `/idea`
   - Uses existing MCP servers and templates

3. **Visual feedback**
   - macOS notification: "✅ Captured! [content-type] note created"
   - Show generated filename and tags
   - Error notifications with user-friendly messages

**Testing:**
- YouTube URL → `/youtube-note`
- GitHub URL → `/gitingest`
- Web article URL → `/capture`
- Plain text → `/idea`
- Empty clipboard → Error notification

---

### Phase 2: Publishing Automation (Week 3-4)

**Goal**: Replace manual `/publish` command with Cmd+Shift+P hotkey

**Components:**
1. **Hammerspoon publish script** (`doublecmdc-publish.lua`)
   ```lua
   -- Detect active Obsidian note
   -- Get note filename
   -- Trigger /publish command via Claude Code CLI
   ```

2. **Publishing workflow**
   - Read current note path from Obsidian
   - Run `/publish <note-filename>.md`
   - Track git push status
   - Wait for GitHub Pages deployment (~60s)

3. **Visual feedback**
   - Notification: "🚀 Publishing [note-title]..."
   - Progress indicator during GitHub Pages deploy
   - Final notification: "✅ Published! [shareable-url]"
   - Copy URL to clipboard automatically

**Testing:**
- Public note (`access: public`) → Published without password
- Private note (`access: private`) → Published with password protection
- Note with images → Images copied + paths converted
- Invalid frontmatter → Error with helpful message

---

### Phase 3: Background Processing (Week 5-6)

**Goal**: Run capture/publish without requiring terminal or Claude Code to be open

**Components:**
1. **Background daemon** (`doublecmdc-daemon`)
   - Runs Claude Code in background
   - Listens for Hammerspoon triggers
   - Processes capture/publish queue
   - Manages MCP server connections

2. **Queue system**
   - Offline mode: Queue captures when vault unavailable
   - Rate limiting: Prevent accidental rapid captures (max 1/sec)
   - Retry logic: Auto-retry failed operations
   - Error recovery: Graceful degradation

3. **System integration**
   - Launch daemon on macOS login (launchd)
   - Monitor resource usage (CPU, memory)
   - Automatic restart on crashes
   - Clean shutdown on system sleep

**Testing:**
- Capture while offline → Queued and processed when online
- Multiple rapid captures → Rate limited to 1/sec
- Daemon crash → Automatic restart
- System sleep/wake → Queue preserved

---

### Phase 4: Advanced Features (Week 7-8)

**Goal**: Polish user experience and add power-user features

**Features:**
1. **Smart content detection**
   - Image clipboard → OCR + create image note
   - Code snippet → Detect language + create code note
   - PDF link → Extract text + create document note
   - Multiple URLs → Batch capture

2. **Customization**
   - Configurable hotkeys (not just double Cmd+C)
   - Custom templates for different content types
   - Tag preferences (which tags to auto-apply)
   - Notification preferences (verbose, minimal, silent)

3. **Advanced publishing**
   - Bulk publish: Select multiple notes + Cmd+Shift+P
   - Scheduled publishing: Set publish date/time
   - Draft mode: Publish to staging branch first
   - Unpublish: Remove from sharehub with one command

4. **Analytics & insights**
   - Capture statistics (daily/weekly counts)
   - Most-captured content types
   - Publishing frequency
   - Sharehub traffic (if enabled)

---

### Phase 5: Documentation & Distribution (Week 9-10)

**Goal**: Make doublecmdc easy to install and use for others

**Deliverables:**
1. **Installation wizard**
   - One-command setup: `bash <(curl -s https://...)`
   - Detects Hammerspoon, Claude Code, Obsidian
   - Auto-configures settings
   - Validates MCP server connections

2. **Documentation**
   - README with GIFs/videos showing workflow
   - Configuration guide (hotkeys, templates, preferences)
   - Troubleshooting guide (common errors, solutions)
   - Architecture diagram (how components interact)

3. **Distribution**
   - GitHub repository: `doublecmdc` plugin
   - Claude Code plugin marketplace
   - Hammerspoon Spoons directory
   - obsidian-vault-manager integration

4. **Community**
   - Examples gallery (showcase real-world usage)
   - User testimonials
   - Discord/forum for support
   - Video tutorial series

---

### Success Metrics

**User Experience:**
- ✅ Capture time: < 1 second (from double Cmd+C to notification)
- ✅ AI processing: < 5 seconds (content type detection + tagging)
- ✅ Publishing time: < 10 seconds (from hotkey to git push complete)
- ✅ Error rate: < 1% (failed captures/publishes)
- ✅ User satisfaction: "This changed how I manage knowledge"

**Technical:**
- ✅ CPU usage: < 5% average (background daemon)
- ✅ Memory usage: < 100 MB (background daemon)
- ✅ Reliability: 99.9% uptime (daemon restarts)
- ✅ Offline support: Queue preserves 100% of captures

**Adoption:**
- ✅ Daily active users using doublecmdc
- ✅ Capture frequency increases 10x vs. manual
- ✅ Publishing frequency increases 5x vs. manual
- ✅ Knowledge shared publicly increases 20x

---

## 💎 The Value Proposition

### What Makes doublecmdc Revolutionary

**It's not just a clipboard manager. It's the completion of the KnowledgeFactory vision.**

**The Promise:**
> "From any content, anywhere, to your curated private vault to published shareable knowledge—with just two gestures and zero context switching."

### The Three Impossibilities It Makes Possible

**1. Capture Without Interruption**
- **Before**: Capturing means stopping what you're doing
- **After**: Double Cmd+C, keep working, note appears in vault

**2. Organization Without Effort**
- **Before**: Manual tagging is exhausting at scale
- **After**: AI tags everything perfectly in 3 seconds

**3. Sharing Without Friction**
- **Before**: Exporting to blog/Medium takes 5+ minutes
- **After**: Cmd+Shift+P, URL ready in 68 seconds

### The Compounding Effect

**Week 1 with doublecmdc:**
- 50 captures (vs. 5 manual captures)
- 10 published notes (vs. 0 manual exports)
- Knowledge shared: 10x increase

**Month 1 with doublecmdc:**
- 200+ captures (vs. 20 manual captures)
- 40+ published notes (vs. 2 manual exports)
- Knowledge shared: 20x increase
- Network effects: Your published knowledge gets discovered, shared, linked

**Year 1 with doublecmdc:**
- 2,400+ captures (vs. 240 manual captures)
- 480+ published notes (vs. 24 manual exports)
- Knowledge shared: Your entire vault becomes a searchable, shareable knowledge base
- Impact: Your ideas reach thousands instead of sitting unused in notes

### Why This Matters for KnowledgeFactory

**doublecmdc transforms KnowledgeFactory from a "tool" into a "habit":**

**Without doublecmdc:**
- KnowledgeFactory is powerful but requires conscious effort
- You have to "remember" to capture and publish
- Friction prevents habit formation

**With doublecmdc:**
- Capturing becomes unconscious (like copying text)
- Publishing becomes effortless (like saving a file)
- Habits form naturally because there's no friction

**Result**: KnowledgeFactory becomes your default knowledge workflow, not a special tool you "decide" to use.

---

## 🚀 Vision: The Ultimate Knowledge Workflow

**Imagine your workday with doublecmdc:**

**Morning (9:00 AM - 12:00 PM):**
- Reading articles, seeing tweets, watching YouTube
- Every valuable insight: Double Cmd+C
- 15 captures in 3 hours (0 context switches)

**Lunch break:**
- Open Obsidian, review morning captures
- AI already tagged and organized everything
- Refine 2-3 notes with insights
- Cmd+Shift+P to publish the best one

**Afternoon (1:00 PM - 5:00 PM):**
- More captures (research, GitHub repos, code examples)
- Another 10 captures (still 0 context switches)

**End of day:**
- 25 captures total (vs. 2-3 manual captures)
- 3 published notes (vs. 0 manual exports)
- Your knowledge vault grew 10x faster
- Your public knowledge base is live and shareable

**Result over time:**
- Private vault: Comprehensive knowledge archive (everything you learned)
- Public sharehub: Curated expertise (best insights shared with world)
- Zero organizational debt (AI handles all tagging/formatting)
- Maximum knowledge leverage (capture everything, share the best)

## 🏷️ Tags Analysis

**Content Analysis:**
- **Type**: `idea` (Captured thought/concept for a productivity tool)
- **Topics**: `productivity` (workflow optimization), `automation` (keyboard shortcuts, background processing), `knowledge-management` (Obsidian integration, capture workflow), `tools` (macOS utility)
- **Characteristics**: `actionable` (has concrete implementation steps), `technical` (requires Hammerspoon scripting, MCP integration, clipboard handling)
- **Priority**: `high` - This directly addresses the core friction point in knowledge capture workflows. Implementing this would significantly improve the KnowledgeFactory user experience and adoption.

**Why These Tags:**
- `productivity` + `automation` + `knowledge-management` + `tools` cover the four key aspects of this solution
- `actionable` because there are clear next steps for implementation
- `technical` because it requires scripting, API integration, and understanding of macOS automation
- `high` priority because reducing capture friction is a known high-leverage improvement

**Suggested Bases Filters:**
- Find similar ideas: `type = idea AND tags contains "productivity"`
- Find actionable items: `type = idea AND tags contains "actionable" AND status = inbox`
- Find by priority: `priority = high AND status = inbox`
- Find by topic combination: `tags contains "productivity" AND tags contains "automation"`

## 🔍 Related Searches

- `/semantic-search "productivity automation"`
- `/semantic-search "friction-free capture workflow"`

---

**Captured**: 2025-11-14
**Status**: inbox (needs processing)
**Next Action**: Review and categorize, add to relevant project if actionable
