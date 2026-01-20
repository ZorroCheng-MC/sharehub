---
title: "Development Plan: /slide Command - NotebookLM Integration"
tags: [project, AI, Google, NotebookLM, API, automation, technical, KnowledgeFactory, development]
source: https://docs.cloud.google.com/gemini/enterprise/notebooklm-enterprise/docs/api-notebooks
date: 2026-01-20
type: project
status: planning
priority: high
read: false
---

# Development Plan: /slide Command - NotebookLM Integration

## Overview

Build a `/slide` command for KnowledgeFactory that sends Obsidian notes to NotebookLM Enterprise API, generates an infographic/presentation, and returns a shareable link.

**Goal:** Transform any vault note into a visual infographic via NotebookLM, enabling one-command visual content generation.

## Architecture

![/slide Command Architecture](/sharehub/images/slide-command-architecture.png)

**Asset Types Generated:**
- Infographic (visual summary)
- Slide Deck (presentation format)
- Study Guide (structured outline)
- Audio Overview (podcast)

## Implementation Plan

### Phase 1: Prerequisites & Setup

#### 1.1 Browser & Authentication Setup
- [ ] Claude Code installed with browser capability (`/chrome` command)
- [ ] Google account logged into Chrome browser
- [ ] NotebookLM already opened once in Chrome (to establish session)
- [ ] Claude Code has access to MCP Obsidian tools

#### 1.2 Environment Configuration
```bash
# Add to ~/.zshrc or environment
export NOTEBOOKLM_URL="https://notebooklm.cloud.google.com"
# No service accounts needed - uses your authenticated Chrome session
```

### Phase 2: Claude Agent Script (Within `/slide` Command)

#### 2.1 Slash Command Implementation
**Location:** `.claude/commands/slide.md`

The `/slide` command will be **pure Claude agent logic** that:
1. Uses `/chrome` to navigate and interact with NotebookLM
2. Uses MCP Obsidian tools to read vault files
3. Controls the browser through natural Claude instructions
4. Extracts results from page content

**Key insight:** Claude interactively controls Chrome via `/chrome` command, making decisions and taking actions based on what it sees in the browser.

```
/slide my-note.md --type infographic

Claude's workflow:
  1. Read the note content via MCP Obsidian tools
  2. Open /chrome and navigate to NotebookLM
  3. Click "Create notebook" button
  4. Fill in notebook title
  5. Paste note content into source field
  6. Click "Generate Infographic"
  7. Wait and monitor progress
  8. Extract the shareable link
  9. Return link to user
```

### Phase 3: Claude Code Slash Command

#### 3.1 Create `/slide` Command
**Location:** `.claude/commands/slide.md`

```markdown
# /slide - Generate Infographic/Slides from Notes via NotebookLM UI

Browser-automated generation of infographics, slides, and study guides from vault notes using Claude's `/chrome` command.

## Usage
/slide <filename.md> [--type <asset-type>] [--share <emails>]

## Arguments
- `filename.md` - The note to convert (relative to vault root)
- `--type` - Asset type: infographic (default), slide-deck, study-guide, audio
- `--share` - Comma-separated emails to share notebook with

## Asset Types

| Type | Output | Use Case |
|------|--------|----------|
| `infographic` | Visual summary graphic | Quick visual reference |
| `slide-deck` | Presentation slides | Sharing findings with team |
| `study-guide` | Structured outline | Learning materials |
| `audio` | Audio podcast (MP3) | Listening on the go |

## How It Works

This command uses **Claude's natural interaction with your Chrome browser** via the `/chrome` tool:

1. **Read vault file** - Fetch note content from Obsidian using MCP Obsidian tools
2. **Open NotebookLM** - Use `/chrome` to navigate and control browser
3. **Create notebook** - Click UI buttons, fill forms as seen on screen
4. **Add source** - Paste note content into NotebookLM
5. **Trigger generation** - Click "Generate [Asset Type]"
6. **Monitor progress** - Watch for completion indicator
7. **Extract link** - Read shareable URL from screen
8. **Return to user** - Output the link and notebook URL

## Implementation Pattern

When you run `/slide`:

1. Claude reads your note from the vault
2. Claude opens `/chrome` and sees the NotebookLM UI
3. Claude sees "Create notebook" button and clicks it
4. Claude fills in title field and clicks Create
5. Claude sees "Add source" and pastes your note content
6. Claude clicks "Generate Infographic" button
7. Claude monitors the loading spinner
8. When done, Claude clicks Share and reads the link
9. Claude returns the link to you

## Output Format

\`\`\`
✓ infographic generated for: my-research-note.md

Link: https://notebooklm.cloud.google.com/.../infographic/share/...
Notebook: https://notebooklm.cloud.google.com/.../notebook/...

Ready to share! Anyone with the link can view the infographic.
\`\`\`

## Examples

\`\`\`bash
# Generate infographic (default)
/slide my-research-note.md

# Generate slide deck
/slide quarterly-report.md --type slide-deck

# Generate and share with team
/slide findings.md --type infographic --share alice@company.com,bob@company.com

# Generate audio overview
/slide interview-notes.md --type audio
\`\`\`

## Prerequisites

- [ ] Google account logged into Chrome
- [ ] NotebookLM accessed at least once (to establish session)
- [ ] MCP Obsidian tools available
- [ ] Claude Code `/chrome` command available

## How It's Different from API

| Aspect | NotebookLM API | `/chrome` UI Automation |
|--------|----------------|------------------------|
| **Infographics** | ❌ Not available | ✓ Full UI support |
| **Slides** | ❌ Not available | ✓ Full UI support |
| **Public sharing** | ⚠️ Requires GCP IAM | ✓ Direct public link |
| **Setup** | 🔴 Enterprise + service account | 🟢 Just logged-in Chrome |
| **Cost** | 🔴 Enterprise licensing | 🟢 Free (consumer NotebookLM) |
| **Reliability** | 🟢 API stability | ⚠️ UI-dependent |

## Notes

- Requires Chrome browser with active Google login (automatic if already logged in)
- Generation typically takes 20-40 seconds depending on content length
- Notebooks persist in NotebookLM for future access/editing
- Shareable links are public - no login required for viewers
- Best for notes < 5000 tokens (NotebookLM limit)
```

#### 3.2 Implementation Pattern in Command

When this command executes, Claude will:

```
STEP 1: Read vault file
├─ Use MCP Obsidian tools: get_vault_file("my-note.md")
└─ Extract content and metadata

STEP 2: Open /chrome
├─ Navigate to https://notebooklm.cloud.google.com
├─ Verify authentication (see if already logged in)
└─ Look for UI elements

STEP 3: Create Notebook
├─ Find "Create notebook" button
├─ Click it (see dialog appear)
├─ Fill title field with note filename
├─ Click "Create" button
└─ Wait for notebook to load

STEP 4: Add Source
├─ Find "Add source" or "Add material" button
├─ Click "Add text source" or similar
├─ Paste the note content
├─ Click "Add" or "Upload"
└─ Wait for source processing

STEP 5: Generate Asset
├─ Find generation buttons (Infographic, Slides, etc)
├─ Click appropriate button for asset type
├─ Monitor for loading spinner
└─ Wait for completion (max 2 min)

STEP 6: Extract Link
├─ Find Share button
├─ Click to reveal shareable link
├─ Read link from input field
└─ Copy to clipboard

STEP 7: Return Result
├─ Output link and notebook URL
└─ Success message
```

### Phase 4: Integration with KnowledgeFactory

#### 4.1 Update obsidian-vault-manager Skill
Add `/slide` command to SKILL.md:

```markdown
| `/slide` | Generate infographic/slides | `/slide note.md --type infographic` |
```

#### 4.2 Update Content Routing (Output Queue)
Add to `/capture` post-processing:

```
After capture complete:
→ Prompt: "Generate visual asset? (y/n) [infographic/slide-deck/study-guide/audio]"
→ If yes: Add to output queue → DoubleCopy processes via `/slide` command
```

#### 4.3 Output Queue File Format
**File:** `.github/data/queue/out-sld-{id}.json`

```json
{
  "type": "output",
  "command": "slide",
  "sourceNote": "2026-01-20-my-note.md",
  "assetType": "infographic",
  "shareWith": ["alice@company.com"],
  "cleanup": true,
  "cleanupDays": 30,
  "addedAt": "2026-01-20T10:00:00Z"
}
```

## Browser Automation with Claude `/chrome`

### Technology Stack

| Component | Purpose | Tool |
|-----------|---------|------|
| **Browser Control** | Navigate & interact with NotebookLM UI | Claude `/chrome` command |
| **File Reading** | Access vault notes | MCP Obsidian Tools |
| **Visual Parsing** | Claude sees screenshots, understands UI | Claude vision + reasoning |
| **Natural Interaction** | Click buttons, fill forms, read results | Claude's decision-making |

### How Claude Interacts with `/chrome`

```
Claude sees the browser screenshot
        ↓
Claude identifies UI elements (buttons, forms, text)
        ↓
Claude decides what action to take next
        ↓
Claude issues command: "Click the 'Create notebook' button"
        ↓
/chrome executes the action
        ↓
New screenshot taken automatically
        ↓
Claude analyzes result and continues
```

### Asset Generation Flow

```
User: /slide my-note.md --type infographic
        ↓
Claude reads vault file
        ↓
Claude opens /chrome → sees NotebookLM homepage
        ↓
Claude: "Click 'Create notebook' button"
/chrome: ✓ Button clicked, dialog appeared
        ↓
Claude: "Type 'my-note' in title field"
/chrome: ✓ Title entered
        ↓
Claude: "Click 'Create' button"
/chrome: ✓ Notebook created, loaded
        ↓
Claude: "Paste the note content into source field"
/chrome: ✓ Content pasted
        ↓
Claude: "Click 'Generate Infographic'"
/chrome: ✓ Generation started, spinner visible
        ↓
Claude: [Wait and monitor] "Is generation done?"
/chrome: [Screenshot shows spinner still running]
Claude: [Wait 10 more seconds and check again]
        ↓
Claude: [Screenshot shows "Share" button]
Claude: "Click 'Share' to get link"
/chrome: ✓ Share dialog opened
        ↓
Claude reads the shareable link from screen
        ↓
Output to user:
✓ infographic generated
Link: https://notebooklm.cloud.google.com/...
```

### Shareable Link Format

NotebookLM generates links in this format:
```
https://notebooklm.cloud.google.com/global/notebook/{NOTEBOOK_ID}?share={SHARE_CODE}
```

**Key properties:**
- ✓ Publicly accessible (no login required)
- ✓ Shareable with anyone
- ✓ Specific to generated infographic/slide
- ✓ Persists for notebook lifetime

## Claude's Advantages with `/chrome`

### vs Playwright/Automation Scripts
- **Intelligent decision-making** - Claude adapts if UI changes
- **Natural language** - No complex selectors or waits needed
- **Visual understanding** - Claude sees exactly what you see
- **Error recovery** - Claude can retry or take alternate path
- **No maintenance** - Works if NotebookLM UI changes

### vs NotebookLM Enterprise API
- **Full feature access** - Infographics, slides, all UI features
- **Public sharing** - Direct shareable links, no GCP setup
- **Simple setup** - Just logged-in Chrome, no service accounts
- **Zero cost** - Consumer NotebookLM, no enterprise licensing

## Reliability & Error Handling

### Generation Timeouts
- Monitor for loading spinner (max 120 seconds)
- If timeout: Provide notebook URL for manual completion
- User can check back later and share when ready

### Browser State Issues
- Chrome session reuses authentication (stays logged in)
- If not authenticated: Claude sees login screen, can help
- If notebook fails to create: Claude retries from last stable state

### UI Changes
- If button labels change: Claude adapts based on what it sees
- If layout changes: Claude uses visual understanding to find elements
- Fallback: Return notebook URL if UI unrecognizable

## Integration Points

```
/slide command workflow:

CLI Input → .claude/commands/slide.md
        ↓
Claude reads vault (MCP Obsidian tools)
        ↓
Claude opens /chrome (browser control)
        ↓
Claude interacts with NotebookLM UI (visual + reasoning)
        ↓
Claude returns shareable link + notebook URL
        ↓
User shares link → Public infographic accessible
```

## Development Tasks

### MVP (Minimum Viable Product)
- [ ] Create `.claude/commands/slide.md` command definition
- [ ] Write Claude agent logic to:
  - [ ] Read vault file via MCP Obsidian tools
  - [ ] Open `/chrome` and navigate to NotebookLM
  - [ ] Create new notebook via UI interaction
  - [ ] Add note content as source
  - [ ] Trigger infographic generation
  - [ ] Monitor completion and extract link
- [ ] Test end-to-end with sample note
- [ ] Document in obsidian-vault-manager SKILL.md

### Enhanced Version (v1.1)
- [ ] Support multiple asset types (--type flag):
  - [ ] slide-deck
  - [ ] study-guide
  - [ ] audio-overview
- [ ] Implement --share flag (optional email sharing)
- [ ] Add status messages and progress tracking
- [ ] Error handling for timeout/failures
- [ ] Return both asset link AND notebook URL

### Future Enhancements (v2.0+)
- [ ] Auto-cleanup (delete notebooks after X days)
- [ ] Integrate with DoubleCopy output queue
- [ ] Batch processing multiple notes
- [ ] Auto-trigger from `/capture` workflow
- [ ] Cache generated links to avoid re-generation
- [ ] Mobile trigger via DoubleCopy Mobile app
- [ ] Download options (PNG/PDF)

## Testing Checklist

- [ ] Chrome browser with Google auth works
- [ ] Single note → infographic generation succeeds
- [ ] Markdown formatting preserved in notebook
- [ ] Links and references handled correctly
- [ ] Generated shareable link is publicly accessible
- [ ] Multiple asset types (slides, study guide) work
- [ ] Timeout handling (> 2 min) returns notebook URL
- [ ] Claude can adapt to NotebookLM UI changes
- [ ] Error messages are clear
- [ ] Full process completes in < 2 minutes
- [ ] Works across multiple runs without issues

## Comparison: API vs Browser Automation

| Aspect | NotebookLM Enterprise API | Browser Automation (Recommended) |
|--------|--------------------------|--------------------------------|
| **Public sharing** | Requires GCP IAM roles | Public shareable links ✓ |
| **Asset types** | Limited (podcast only) | Full UI support ✓ |
| **Infographics** | Not available | Available via UI ✓ |
| **Slide decks** | Not available | Available via UI ✓ |
| **Setup complexity** | GCP account, service account | Just Google account ✓ |
| **Cost** | Enterprise licensing | Free (consumer NotebookLM) ✓ |
| **Maintenance** | API may change | UI-dependent (fragile) |

**Chosen approach: Browser Automation** - Leverages free consumer NotebookLM UI to access full feature set including infographics and public sharing.

## Architecture Diagram

```
User's Obsidian Vault
        │
        │ /slide my-note.md --type infographic
        ▼
Claude Code (/slide command)
        │
        ├─ Read vault file (MCP Obsidian Tools)
        │
        ├─ Launch Playwright browser automation
        │
        ▼
Browser (Playwright)
        │
        ├─ Navigate to https://notebooklm.cloud.google.com
        │
        ├─ Create new notebook (click UI)
        │
        ├─ Paste note content into sources
        │
        ├─ Click "Generate Infographic"
        │
        ├─ Poll for completion (max 2 min)
        │
        ├─ Extract shareable link
        │
        └─ Optional: Share with team emails
        │
        ▼
Return to user
        │
        ├─ Infographic link: https://notebooklm.cloud.google.com/...
        │
        ├─ Notebook link: https://notebooklm.cloud.google.com/...
        │
        └─ Success message with asset preview
```

## Related Documentation

- [[KnowledgeFactory Enterprise]] - Parent system documentation (Phase 5: Output Commands Expansion)
- [NotebookLM Consumer UI](https://notebooklm.cloud.google.com) - User interface
- [Playwright Documentation](https://playwright.dev) - Browser automation library
- obsidian-vault-manager skill - Integration target
- [[KnowledgeFactory Enterprise#DoubleCopy Mobile]] - Future mobile integration

---

**Created**: 2026-01-20
**Status**: Planning → Ready for Implementation
**Next Action**: Build Playwright automation script and test with sample note
