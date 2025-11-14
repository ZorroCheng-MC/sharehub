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

doublecmdc adds friction-free clipboard capture to KnowledgeFactory. Double-tap Cmd+C anywhere on macOS to send clipboard content (text, links, YouTube URLs) directly to your Obsidian vault with AI auto-tagging—capture happens in the background while you continue working.

This creates a seamless workflow where knowledge capture becomes nearly invisible. Instead of context-switching to your vault, manually creating notes, and organizing content, you simply double-tap Cmd+C on any selected content and it's automatically captured, tagged, and routed to the appropriate template based on content type.

## 🎯 Why This Matters

This eliminates the primary friction point in knowledge management: the overhead of capturing information. By reducing capture to a single gesture (double Cmd+C), it removes the mental barrier that prevents people from saving valuable insights they encounter throughout their day. Combined with AI auto-tagging, it ensures captured content is immediately organized and searchable without manual categorization work.

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

## 📝 Next Steps

1. **Implement core doublecmdc script**
   - Monitor for double Cmd+C keyboard events using Hammerspoon
   - Detect content type (URL pattern matching, plain text fallback)
   - Route to appropriate capture workflow (YouTube/GitHub/article/idea)

2. **Integrate with obsidian-vault-manager skill**
   - Use existing MCP tools for vault operations
   - Leverage bundled templates and tag taxonomy
   - Reuse AI analysis logic for smart tagging

3. **Add notification feedback**
   - Visual confirmation when capture succeeds
   - Show filename and applied tags
   - Error handling with user-friendly messages

4. **Test edge cases**
   - Multiple clipboard formats (rich text, images, code)
   - Very long content (pagination, truncation)
   - Rate limiting (prevent accidental rapid captures)
   - Offline mode (queue captures when vault unavailable)

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
