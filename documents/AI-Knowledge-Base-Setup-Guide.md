---
title: "AI-Powered Knowledge Base: Capture, Organize, and Share Your Ideas"
tags: [guide, AI, knowledge-management, tools, obsidian, evergreen, tutorial, actionable]
date: 2025-09-11
type: guide
status: evergreen
---

# AI-Powered Knowledge Base: Capture, Organize, and Share Your Ideas

> **🚀 Quick Start**: Fork the source repository at https://github.com/ZorroCheng-MC/sharehub to get started!

---

## The Problem: Information Overload and Scattered Knowledge

Do you struggle with any of these challenges?

- 💭 **Random thoughts disappear** - Great ideas pop into your head but vanish before you can act on them
- 📺 **YouTube overwhelm** - You watch educational videos but can't remember the key points later
- 📚 **Scattered reading notes** - Articles and documents you've read are forgotten or buried in browser bookmarks
- 🔍 **Can't find what you learned** - You know you learned something about a topic, but can't locate your notes
- 🤝 **Hard to share knowledge** - You want to share your insights publicly, but organizing them is too much work
- 🔗 **Missing connections** - Related ideas and concepts remain isolated, preventing deeper understanding
- ⏰ **Too much manual work** - Organizing, formatting, and structuring notes takes time away from learning

**The core problem**: We live in an age of information abundance but knowledge scarcity. We consume vast amounts of content but struggle to transform it into organized, actionable knowledge.

---

## The Solution: An AI-Powered Knowledge Management System

This setup combines three powerful tools to solve these problems:

```
📝 Obsidian → 🤖 Claude Code → 🌐 GitHub Pages
(Capture)      (Organize)       (Share)
```

### What It Does

1. **Captures everything instantly** - Ideas, YouTube videos, articles, repositories, and study materials with a single command
2. **AI analyzes and structures** - Claude Code automatically extracts key points, creates summaries, and organizes content
3. **Finds hidden connections** - Semantic search discovers relationships between your notes you didn't know existed
4. **Organizes automatically** - AI-powered tagging and categorization keep your knowledge base clean
5. **Publishes selectively** - Share polished content publicly on GitHub Pages with automated formatting

### Key Benefits

✅ **Zero friction capture** - Capture any content in seconds, not minutes
✅ **AI-powered intelligence** - Let AI do the heavy lifting of analysis and organization
✅ **Never lose an idea** - Everything is stored, searchable, and connected
✅ **Discover insights** - Find relationships between ideas you didn't see before
✅ **Professional sharing** - Publish polished content with automatic formatting
✅ **Complete control** - Your knowledge base lives on your machine and your GitHub

---

## AI-Powered Smart Tagging: Automatic Organization

One of the most powerful features of this system is **AI-powered smart tagging** that automatically organizes your knowledge base without manual effort.

### The Problem with Traditional Tagging

Most note-taking systems require you to:
- Manually assign tags to every note
- Remember your tag taxonomy
- Ensure consistency across hundreds of notes
- Spend time organizing instead of learning

**This system does all of that automatically.**

### How AI Smart Tagging Works

When you capture content, Claude analyzes it and automatically assigns:

1. **Content Type Tags** - What kind of content is this?
   - `idea`, `video`, `article`, `study-guide`, `repository`, `reference`, `project`

2. **Topic Tags** - What subjects does it cover? (2-4 tags)
   - `AI`, `productivity`, `knowledge-management`, `development`, `learning`, `research`, `writing`, `tools`, `business`, `design`, `automation`, `data-science`, `web-development`, `personal-growth`, `finance`

3. **Status Tags** - Where is it in your workflow?
   - `inbox` (new captures), `processing` (active work), `evergreen` (permanent knowledge), `published` (shared publicly)

4. **Metadata Tags** - What are its characteristics?
   - `high-priority`, `quick-read`, `deep-dive`, `technical`, `conceptual`, `actionable`, `tutorial`, `inspiration`

5. **Additional Properties**
   - `priority` (high/medium/low)
   - `type` (matches content type)
   - `status` (matches status tag)
   - `date` (capture date)

### Example: Automatic Tagging in Action

```bash
# You capture a YouTube video:
/capture https://youtube.com/watch?v=example

# AI analyzes the content and creates:
```

```yaml
---
title: "Andrew Huberman - Focus Toolkit"
tags: [video, productivity, learning, personal-growth, inbox, tutorial, deep-dive]
url: https://youtube.com/watch?v=example
date: 2025-10-23
type: video
status: inbox
priority: high
duration: 2h 15m
---
```

**Without you doing anything**, the note is now:
- Categorized as a video
- Tagged with relevant topics (productivity, learning, personal-growth)
- Marked as inbox for processing
- Labeled as a tutorial and deep-dive
- Assigned high priority based on content analysis
- Ready for Obsidian Bases filtering

### Powerful Filtering with Bases

The consistent AI tagging enables powerful queries in Obsidian Bases:

```
Find all high-priority videos about AI that are tutorials
→ type = video AND tags contains "AI" AND priority = high AND tags contains "tutorial"

Find all unprocessed ideas
→ type = idea AND status = inbox

Find all actionable content for learning
→ tags contains "learning" AND tags contains "actionable"

Find all deep-dive technical content
→ tags contains "technical" AND tags contains "deep-dive"
```

### Bulk Tagging Existing Notes

Already have notes without tags? Use the `/bulk-auto-tag` command:

```bash
# Tag all existing markdown files:
/bulk-auto-tag .

# Tag specific folder:
/bulk-auto-tag documents/

# Tag files matching pattern:
/bulk-auto-tag "*.md"
```

AI analyzes each file and adds comprehensive tags automatically.

### Tag Quality Guarantees

- **Consistent**: Same tag taxonomy across all notes
- **Accurate**: AI understands context, not just keywords
- **Complete**: 5-8 tags per note (optimal for filtering)
- **Useful**: Tags enable powerful Bases queries
- **Automatic**: Zero manual tagging required

> 📖 **Want to learn more?** See the [AI Smart Tagging Strategy Guide](AI-Smart-Tagging-Strategy.md) for advanced usage, custom taxonomies, and migration strategies.

---

## How It Works: From Chaos to Clarity

### Architecture Overview

```
┌─────────────────────┐
│   Obsidian Vault    │  ← Your centralized knowledge base
│   (Your Content)    │
└──────────┬──────────┘
           │
           ├─► 🤖 Claude Code
           │   ├─► Smart Capture (/capture)
           │   ├─► Content Analysis
           │   └─► Automated Organization
           │
           ├─► 🔍 Smart Connections
           │   └─► Semantic Search & Discovery
           │
           └─► 🚀 GitHub Pages
               └─► Automated Publishing
```

---

## Capturing Everything: Your Second Brain

### 1. Random Ideas and Thoughts 💡

**The Problem**: Fleeting thoughts that could become great ideas are lost forever.

**The Solution**: Instant capture with AI cleanup.

```bash
# Simply type:
/capture "AI-powered note-taking could revolutionize how students learn by providing instant summaries and connections"

# AI automatically:
# - Cleans up your rough thought
# - Creates a properly formatted note
# - Adds relevant tags
# - Saves with today's date
```

**Result**: A clean, organized idea note with AI-generated tags:
```yaml
---
title: "AI-Powered Student Learning System"
tags: [idea, AI, learning, education, inbox, actionable, conceptual]
date: 2025-10-23
type: idea
status: inbox
priority: medium
---

## 💡 Core Idea
AI-powered note-taking could revolutionize student learning by providing instant summaries and connections to existing knowledge.

## 🎯 Why This Matters
Reduces cognitive load for students, allowing them to focus on understanding rather than note organization.

## 📝 Next Steps
- Research existing AI note-taking tools
- Interview students about pain points
- Design prototype workflow
```

### 2. YouTube Videos and Lectures 📺

**The Problem**: You watch a 45-minute video but only remember fragments days later.

**The Solution**: Automatic transcript extraction and structured analysis.

```bash
# Just paste the URL:
/capture https://youtube.com/watch?v=dQw4w9WgXcQ

# AI automatically:
# - Fetches the full transcript
# - Extracts the title and creator
# - Summarizes key points
# - Creates a learning structure
# - Embeds the thumbnail
# - Generates learning objectives
```

**Result**: A comprehensive video note with AI-generated structure:

```yaml
---
title: "Building Better APIs - Fireship Tutorial"
tags: [video, development, web-development, API, inbox, tutorial, technical]
url: https://youtube.com/watch?v=dQw4w9WgXcQ
cover: https://img.youtube.com/vi/dQw4w9WgXcQ/maxresdefault.jpg
date: 2025-10-23
type: video
status: inbox
priority: high
duration: 12m 30s
---

## 📖 Description
Comprehensive guide to REST API design principles covering authentication, versioning, and best practices for scalable web services.

![Video Thumbnail](https://img.youtube.com/vi/dQw4w9WgXcQ/maxresdefault.jpg)

## 🎯 Learning Objectives
- Understand RESTful API design principles
- Implement proper authentication patterns
- Structure endpoints for scalability
- Handle versioning and deprecation

## 📋 Curriculum/Contents
- [ ] **Introduction to REST** (0:00-2:30)
- [ ] **HTTP Methods & Status Codes** (2:30-5:00)
- [ ] **Authentication Strategies** (5:00-8:00)
- [ ] **Versioning & Documentation** (8:00-12:30)

## 📝 Notes & Key Takeaways
(Your notes here after watching)
```

Complete with:
- AI-generated tags and metadata
- Automatic thumbnail embedding
- Structured learning objectives
- Timestamped curriculum
- Priority assignment based on content

### 3. Online Articles and Documents 📄

**The Problem**: You read great articles but forget the insights within days.

**The Solution**: AI-generated study guides from any content.

```bash
# From a URL:
/study-guide https://example.com/article-about-machine-learning

# From a file:
/study-guide ~/Downloads/important-paper.pdf

# AI automatically:
# - Analyzes the content depth
# - Identifies main topics and concepts
# - Creates learning objectives
# - Structures the material for retention
# - Adds self-assessment questions
# - Suggests related resources
```

**Result**: A comprehensive study guide with AI-powered organization:

```yaml
---
title: "Study Guide: Machine Learning Fundamentals"
tags: [study-guide, AI, data-science, learning, processing, technical, deep-dive]
source: https://example.com/ml-article
date: 2025-10-23
type: study-guide
status: processing
difficulty: intermediate
estimated-time: 40 hours
priority: high
---

## 📚 Overview
**Subject:** Machine Learning Fundamentals
**Source:** Introduction to ML Algorithms by Example Corp
**Generated:** 2025-10-23

### 🎯 Learning Objectives
By the end of this study session, you will be able to:
- [ ] Explain the difference between supervised and unsupervised learning
- [ ] Implement basic ML algorithms from scratch
- [ ] Evaluate model performance using appropriate metrics
- [ ] Select the right algorithm for different problem types

### ⏱️ Study Plan
- **Total Estimated Time:** 40 hours
- **Difficulty Level:** Intermediate
- **Prerequisites:** Python, basic statistics
- **Recommended Study Method:** Practice-based with code examples

## 📋 Content Structure

### Week 1: Fundamentals (10 hours)
- **Supervised Learning Basics**
- **Key Algorithms: Linear Regression, Logistic Regression**
- **Practice Exercises:** 5 coding challenges

### Week 2-4: Advanced Topics (30 hours)
- **Neural Networks & Deep Learning**
- **Model Evaluation & Optimization**
- **Real-world Project Implementation**

## 💡 Study Strategies
- Focus on implementation over theory
- Build projects with each concept
- Use spaced repetition for formulas
- Join study groups for discussion

## 🧠 Self-Assessment
- [ ] Can you explain overfitting vs underfitting?
- [ ] Can you implement gradient descent?
- [ ] Can you choose the right model for a new dataset?
```

Complete with:
- AI-assigned difficulty level and time estimates
- Structured weekly breakdown
- Customized study strategies
- Self-assessment checkpoints
- Progress tracking

### 4. GitHub Repositories and Code 💻

**The Problem**: Understanding large codebases is time-consuming and overwhelming.

**The Solution**: AI-powered repository analysis without cloning.

```bash
# Analyze any repository:
/capture https://github.com/anthropics/anthropic-sdk-python

# AI automatically:
# - Analyzes the code structure
# - Extracts documentation
# - Identifies key components
# - Creates a digestible summary
# - Highlights important patterns
```

**Result**: A structured analysis perfect for understanding the codebase quickly.

### 5. Study Materials and Courses 📚

**The Problem**: Course materials are scattered across platforms and hard to review.

**The Solution**: Consolidated, AI-structured study materials.

```bash
# Create a study guide from any source:
/study-guide course-lecture-notes.md

# AI creates:
# - Comprehensive overview
# - Learning objectives
# - Study plan with time estimates
# - Core content structure
# - Practice questions
# - Knowledge connections
```

---

## Organizing and Consolidating: From Information to Knowledge

### Smart Capture Routes Content Automatically

The `/capture` command is intelligent - it detects what type of content you're capturing and routes it appropriately:

- **YouTube URL?** → Creates a video note with transcript analysis
- **GitHub repo?** → Analyzes the codebase structure
- **Just text?** → Saves as a quick idea
- **Web article?** → Can create a study guide

**You don't think about organization - the AI does it for you.**

### Semantic Search: Find Connections You Didn't Know Existed

Traditional search finds exact words. **Semantic search finds meaning.**

```bash
# Search for concepts, not just keywords:
/semantic-search "How do I automate my learning workflow?"

# Returns related notes about:
# - Automation tools
# - Learning systems
# - Workflow optimization
# - Personal knowledge management
#
# Even if none of those notes mention "automate" or "workflow"!
```

**The Power**: Discover connections between:
- An idea you had 3 months ago
- A video you watched last week
- An article you're reading today

These connections spark new insights and deepen understanding.

### Visual Knowledge Graph

Obsidian's graph view shows how your knowledge connects:

- **Nodes** = Your notes
- **Links** = Relationships between ideas
- **Clusters** = Topics that naturally group together

**See your knowledge grow** and discover unexpected patterns.

### AI-Powered Organization

- **Automatic tagging** - AI suggests relevant tags based on content
- **Smart categorization** - Notes are organized by topic and theme
- **Relationship detection** - AI identifies which notes should link together
- **Duplicate detection** - Alerts you when you're capturing similar ideas

---

## Publishing When Ready: Share Your Knowledge

### Selective Publishing to GitHub Pages

Not everything in your knowledge base is meant for public eyes. **You control what gets shared.**

```bash
# Move notes you want to publish:
mv my-great-article.md documents/

# Commit and push:
git add .
git commit -m "Publish article on AI workflows"
git push origin main

# GitHub Actions automatically:
# 1. Adds proper formatting
# 2. Builds a beautiful static site
# 3. Deploys to GitHub Pages
#
# Your article is live in ~60 seconds!
```

### Automatic Professional Formatting

- **Front matter** - Added automatically for Jekyll processing
- **Responsive layout** - Looks great on mobile and desktop
- **Clean typography** - Professional, readable design
- **SEO optimized** - Proper meta tags and descriptions

### Your Personal Publishing Platform

Your published site becomes:
- 📚 **A portfolio** of your thinking and learning
- 🎓 **A teaching resource** for others
- 🔗 **A shareable reference** for discussions
- 💼 **A demonstration** of your knowledge and skills

---

## Real-World Workflows

### Morning Capture Routine (5 minutes)

```bash
# Open Claude Code in your vault
cd ~/knowledge-base
claude

# Capture overnight thoughts:
> /capture "Idea for improving onboarding flow with progressive disclosure"

# Capture yesterday's video:
> /capture https://youtube.com/watch?v=example

# Quick note from article:
> /capture https://blog.example.com/great-article
```

**Result**: 3 new notes, all structured and ready for review.

### Weekly Review and Organization (30 minutes)

```bash
# Search for related content:
> /semantic-search "onboarding UX patterns"

# Review results and link related notes
# Use Obsidian's graph view to see connections
# Tag and categorize as needed
```

**Result**: Your knowledge base becomes more interconnected and valuable.

### Publishing Flow (2 minutes)

```bash
# Polish a note you want to share
# Move to publish folder
mv polished-article.md documents/

# Push to GitHub
git add . && git commit -m "New article" && git push

# ✅ Live in 60 seconds!
```

**Result**: Professional content published with zero friction.

### Study Session Workflow

```bash
# Create study guide from course material:
> /study-guide lecture-notes.md

# AI generates comprehensive guide with:
# - Learning objectives
# - Structured content
# - Self-assessment
# - Study strategies

# Add your own insights as you study
# Use semantic search to connect to related notes
```

**Result**: Deep, connected learning instead of isolated facts.

---

## Features in Action

### `/capture` - The Universal Inbox

**Single command, intelligent routing**:

```bash
/capture "Random thought"           → Idea note
/capture https://youtube.com/...    → Video analysis
/capture https://github.com/...     → Code analysis
/capture https://article.com/...    → Article note
```

**Stop thinking about structure. Just capture.**

### `/youtube-note` - Never Forget a Video

**Input**: Video URL
**Output**:
- 📺 Full transcript
- 📖 AI-generated summary
- 🎯 Learning objectives
- 📋 Curriculum structure
- 🖼️ Embedded thumbnail

**Value**: Transform passive watching into active learning.

### `/study-guide` - Learn Anything Faster

**Input**: Article, document, or video content
**Output**:
- 🎯 Clear objectives
- 📚 Structured breakdown
- 💡 Study strategies
- 🧠 Self-assessment
- ⏱️ Time estimates

**Value**: Structured learning instead of aimless reading.

### `/gitingest` - Understand Code Instantly

**Input**: GitHub repository URL
**Output**:
- 🏗️ Architecture overview
- 📁 Key files and components
- 📝 Documentation summary
- 🔍 Important patterns

**Value**: Hours of code reading reduced to minutes of AI-processed insights.

### `/semantic-search` - Discovery Engine

**Input**: A question or concept
**Output**: Related notes based on **meaning**, not just keywords

**Example**:
```bash
/semantic-search "How can I be more productive?"

# Returns notes about:
# - Time management systems
# - Focus techniques
# - Automation tools
# - Learning methods
#
# Even if they never mention "productive"!
```

**Value**: Discover insights hiding in your existing knowledge.

### `/bulk-auto-tag` - Organize Existing Notes

**Input**: Folder path or file pattern
**Output**: AI-analyzed tags added to all existing notes

**Example**:
```bash
# Tag all markdown files in current directory:
/bulk-auto-tag .

# Tag specific folder:
/bulk-auto-tag documents/

# Tag files matching pattern:
/bulk-auto-tag "articles/*.md"

# AI analyzes each file and adds:
# - Content type tag (idea, video, article, etc.)
# - 2-4 topic tags from predefined taxonomy
# - Status tag (inbox, processing, evergreen, etc.)
# - Metadata tags (technical, tutorial, quick-read, etc.)
# - Priority level (high/medium/low)
# - Additional properties (type, status, date)
```

**Result**:
```
Processing: 25 markdown files found

✅ Tagged 25 files successfully
   - Average 8 tags per note
   - Consistent taxonomy applied
   - Ready for Bases filtering

📊 Tag Distribution:
   - Content types: 8 ideas, 10 videos, 5 articles, 2 repositories
   - Top topics: AI (15), productivity (12), development (8)
   - Status: 20 inbox, 3 processing, 2 evergreen
   - Priority: 7 high, 15 medium, 3 low
```

**Value**:
- Instantly organize hundreds of existing notes
- No manual tagging required
- Consistent tag taxonomy across entire vault
- Enables powerful Bases filtering retroactively
- Preserves existing content and metadata

**Use Cases**:
- Migrating from another note-taking system
- Organizing an existing untagged vault
- Standardizing tags across all notes
- Preparing notes for Bases filtering
- Annual knowledge base cleanup

---

## Why This System Works

### 1. Minimal Friction = Consistent Use

Most knowledge systems fail because they're too complicated. **This system has one command: `/capture`**

The lower the friction, the more you'll use it. The more you use it, the more valuable it becomes.

### 2. AI Does the Heavy Lifting

You don't organize - **AI organizes for you**:
- Extracts key points
- Suggests tags
- Creates structure
- Finds connections

**Your job**: Capture and think.
**AI's job**: Structure and organize.

### 3. It Gets Smarter Over Time

As you add more notes:
- Semantic search finds better connections
- The graph view reveals patterns
- AI has more context for suggestions
- Your knowledge compounds

**The system becomes more valuable the more you use it.**

### 4. You Own Your Data

Unlike SaaS note-taking apps:
- ✅ Files are plain markdown on your computer
- ✅ No vendor lock-in
- ✅ Works offline
- ✅ Fully searchable and portable
- ✅ Your GitHub repository is your backup

**Your knowledge, your control.**

---

## Getting Started: Three Options

### Option 1: Fork and Use Immediately (Recommended)

1. **Fork the repository**: https://github.com/ZorroCheng-MC/sharehub
2. **Clone to your machine**
3. **Open in Obsidian**
4. **Start capturing with `/capture`**

**Time to first capture**: ~10 minutes

### Option 2: Try It First with Examples

The forked repository includes example notes showing:
- How ideas are captured
- What YouTube notes look like
- Study guide structure
- Repository analysis format

**Explore before committing.**

### Option 3: Build From Scratch

Want to understand every component? See the [Technical Setup Guide](#technical-setup-guide) below.

---

## Daily Usage: Your Workflow

Once you have the system set up, here's how you actually use it day-to-day.

### Your Workspace Setup

The recommended setup uses **two side-by-side windows**:

![Workspace Setup - Obsidian and Claude Code](/sharehub/images/workspace-setup.jpg)

**The ideal layout**:
- **Obsidian (left)** - Browse your notes, see the knowledge graph, organize files
- **Terminal (right)** - Run Claude Code commands to capture and process content

**Why this layout works**:
- You can see notes appear in real-time as Claude creates them
- Easy to switch between capturing (Terminal) and organizing (Obsidian)
- Visual feedback loop helps you understand what the system is doing
- Graph view shows connections as your knowledge base grows

### Step 1: Open Your Vault in Obsidian

```bash
# Open Obsidian and select your vault
# OR use command line:
open -a Obsidian ~/path/to/your-vault
```

Your Obsidian interface shows:
- **File explorer** (left sidebar) - All your notes and folders
- **Editor** (center) - View and edit notes
- **Graph view** (icon) - Visualize connections between notes

### Step 2: Start Claude Code in Terminal

Open a terminal and navigate to your vault:

```bash
cd ~/path/to/your-vault
claude
```

You'll see the Claude Code prompt:

```
Terminal: 📚 Knowledge Management System
>
```

**Pro Tip**: Use Obsidian's Terminal plugin to run Claude directly inside Obsidian! This gives you a true single-window experience.

### Step 3: Capture Content with Commands

Now you can use any capture command. Just type them at the Claude prompt:

#### Capture a Random Idea

```bash
> /capture "AI could help students by creating personalized study plans"
```

**What happens**:
1. Claude processes your idea
2. Creates a formatted note with tags
3. Saves it to your vault
4. You see it appear in Obsidian immediately!

#### Capture a YouTube Video

```bash
> /capture https://youtube.com/watch?v=dQw4w9WgXcQ
```

**What happens**:
1. Claude fetches the video transcript
2. Analyzes the content
3. Creates a structured note with:
   - Title and description
   - Learning objectives
   - Key takeaways
   - Thumbnail image
4. Saves to your vault

#### Create a Study Guide

```bash
> /study-guide https://example.com/article
```

**What happens**:
1. Claude fetches the article content
2. Analyzes the structure and complexity
3. Creates a comprehensive study guide
4. Saves with learning objectives and self-assessment

#### Search Your Knowledge Base

```bash
> /semantic-search "How do I improve my learning workflow?"
```

**What happens**:
1. Claude searches using semantic meaning (not just keywords)
2. Finds related notes across your entire vault
3. Shows you connections you might have missed

### Step 4: Review and Organize in Obsidian

After capturing content, switch to Obsidian to:

1. **Read the generated notes** - Claude has structured everything for you
2. **Add your own thoughts** - Append personal insights and reflections
3. **Create links** - Connect related notes manually if desired
4. **Use graph view** - Visualize how your knowledge is connecting
5. **Tag and categorize** - Add additional tags if needed (AI already added some!)

### Step 5: Publish When Ready

When you have a note you want to share publicly:

#### Method 1: Using Claude (Easiest)

```bash
> Please move my-article.md to the documents folder and publish it
```

Claude will:
1. Move the file to the `documents/` folder
2. Commit to git
3. Push to GitHub
4. Your article goes live in ~60 seconds!

#### Method 2: Manual Commands

```bash
# In terminal (not Claude):
mv my-article.md documents/
git add documents/my-article.md
git commit -m "Publish article about AI learning"
git push origin main
```

GitHub Actions automatically:
1. Adds front matter (if missing)
2. Builds with Jekyll
3. Deploys to GitHub Pages

**Your article is live at**: `https://yourusername.github.io/sharehub/documents/my-article`

### Common Workflows

#### Morning Routine (5 minutes)

```bash
cd ~/knowledge-vault
claude

# Capture overnight thoughts
> /capture "Idea: Use AI for automated meeting summaries"

# Capture yesterday's video
> /capture https://youtube.com/watch?v=example

# Quick search to refresh memory
> /semantic-search "productivity techniques"
```

#### Deep Work Session (30 minutes)

```bash
# Create study guide from course material
> /study-guide lecture-notes.md

# Work through the material in Obsidian
# Add your own notes and highlights

# Search for related concepts
> /semantic-search "related to [current topic]"

# Link related notes in Obsidian
```

#### Publishing Routine (2 minutes)

```bash
# Polish an article in Obsidian
# Then in Claude:
> Move article-name.md to documents/ and publish it

# Or manually:
# mv article-name.md documents/
# git add . && git commit -m "New article" && git push
```

### Tips for Power Users

1. **Keep Claude running** - Leave the terminal open all day for instant capture
2. **Use aliases** - Create shell aliases for common commands
3. **Keyboard shortcuts** - Set up Obsidian hotkeys to switch between windows
4. **Template notes** - Create templates in Obsidian for different note types
5. **Regular reviews** - Weekly review in Obsidian to organize and link notes

### Troubleshooting Common Issues

**Q: Claude command not found?**
```bash
# Make sure Claude Code is installed:
npm install -g @anthropic-ai/claude-code
# Or reinstall if needed
```

**Q: Commands not working?**
```bash
# Check you're in your vault directory:
pwd
# Should show your vault path

# Check .claude/commands/ folder exists:
ls .claude/commands/
```

**Q: Notes not appearing in Obsidian?**
- Obsidian auto-refreshes, but you can force it: View → Force Reload
- Check the file was created: `ls -la` in your vault

**Q: How do I see what was published?**
```bash
# Visit your GitHub Pages site:
https://yourusername.github.io/sharehub/

# Or check GitHub Actions status:
gh run list --limit 5
```

---

## Who Is This For?

This system is perfect if you:

- 📚 **Students** - Capture lectures, create study guides, organize research
- 💼 **Professionals** - Document learning, track projects, share expertise
- ✍️ **Writers** - Collect ideas, organize research, build content library
- 👨‍💻 **Developers** - Analyze codebases, document learnings, build knowledge
- 🎓 **Lifelong learners** - Organize courses, track progress, connect ideas
- 🧠 **Researchers** - Manage papers, notes, and discoveries

**Anyone who learns deserves a system this powerful.**

---

## Success Stories: What Others Are Building

### The Developer's Code Library

*"I analyze interesting GitHub repos with `/gitingest`, then search semantically when I need solutions. I've built a personal Stack Overflow that actually understands my problems."*

### The Student's Study System

*"Every lecture video goes through `/youtube-note`. Before exams, I use `/semantic-search` to find all related concepts. My grades improved because I can finally see how everything connects."*

### The Writer's Idea Factory

*"Random thoughts get captured instantly. When I'm ready to write, `/semantic-search` shows me which ideas connect. I've published 3x more because the ideas are already organized."*

### The Professional's Learning Journal

*"I publish polished articles to GitHub Pages while keeping rough notes private. It's become my professional portfolio and people reach out because they found my content helpful."*

---

## The Compound Effect of Knowledge Management

Most note-taking systems are **graveyards of information** - notes go in but nothing comes out.

This system is different. It creates a **living knowledge base** where:

1. **Capture is effortless** → You capture more
2. **AI organizes automatically** → Your notes stay useful
3. **Semantic search finds connections** → Ideas compound
4. **Publishing is one command** → You share more
5. **Sharing generates feedback** → You learn faster

**The result**: Your knowledge grows exponentially instead of linearly.

### Year 1: Foundation

- 100+ captured ideas and insights
- Basic note structure emerges
- Starting to see connections

### Year 2: Acceleration

- 500+ interconnected notes
- Clear topic clusters
- Semantic search becomes powerful
- Published content generates discussions

### Year 3: Compounding

- 1000+ notes forming a personal knowledge graph
- New captures immediately connect to existing knowledge
- Your knowledge base answers questions you didn't know you had
- Published content becomes a reference library

**This is the power of a system that scales with you.**

---

# Technical Setup Guide

> **Note**: If you forked the repository, most of this is already configured. This section is for those building from scratch or wanting to understand the internals.

## Prerequisites

- **macOS, Linux, or Windows** (this guide assumes macOS/Linux)
- **Obsidian** (latest version)
- **Claude Code CLI** installed
- **Git** installed
- **GitHub Account**
- **Node.js** and **npm** (for some MCP servers)
- **Python 3** (for youtube_transcript_api)

## Component 1: Obsidian Setup

### Install Required Plugins

Install these Obsidian community plugins:

1. **Smart Connections** - AI-powered semantic search
2. **Obsidian Local REST API** - Enables external tool access
3. **Obsidian Git** - Git integration for auto-sync
4. **Templater** - Advanced template support
5. **MCP Tools** - Model Context Protocol integration
6. **Terminal** - Run commands from Obsidian

### Configure Smart Connections

1. Install Smart Connections plugin
2. Enable in Settings → Community Plugins
3. Configure:
   - Model: Choose your preferred embedding model
   - Search settings: Adjust limit and threshold
   - Enable "Show in sidebar" for quick access

### Configure Local REST API

1. Install the plugin from Community Plugins
2. Enable it in Settings → Community Plugins
3. Generate an API key in plugin settings
4. Create `.env` file in your vault root:

```bash
LOCAL_REST_API_KEY=your_generated_api_key_here
```

### Vault Structure

```
YourVault/
├── .claude/
│   ├── commands/          # Custom Claude commands
│   └── agents/            # Specialized AI agents
├── .obsidian/
│   └── plugins/
├── .env                   # API keys (never commit!)
└── (your notes here)
```

## Component 2: Claude Code Configuration

### Install Claude Code

```bash
# Install via npm
npm install -g @anthropic-ai/claude-code

# Or download from https://claude.ai/code
```

### Configure Settings

Create `~/.claude/settings.json` with hooks, permissions, and status line configuration. (See fork for complete example)

### Create Custom Commands

#### `/capture` - Smart Content Router

```markdown
---
description: Smart capture command with intelligent routing
allowed-tools: [Write(*), Read(*), Bash(date:*)]
---

Routes content based on type:
- Ideas → /idea
- YouTube → /youtube-note
- Repositories → /gitingest
- Study content → /study-guide
```

#### `/idea` - Quick Idea Capture

```markdown
Creates dated idea notes with tags and clean formatting.
```

#### `/youtube-note` - Video Analysis

```markdown
Fetches transcripts and creates structured learning notes.
```

#### `/gitingest` - Repository Analysis

```markdown
Analyzes GitHub repos without cloning using GitIngest MCP.
```

#### `/study-guide` - Learning Material Creator

```markdown
Transforms any content into structured study guides.
```

#### `/semantic-search` - Vault Discovery

```markdown
Searches vault using Smart Connections semantic API.
```

### Configure MCP Servers

Essential MCP servers for this setup:

1. **obsidian-mcp-tools** - Obsidian integration
2. **github** - GitHub API access
3. **git** - Git operations
4. **fetch** - Web content fetching
5. **gitingest** - Repository analysis

Configure in your Claude desktop app or via environment.

## Component 3: GitHub Setup

### Create Repository

```bash
# Create on GitHub, then clone:
git clone https://github.com/yourusername/knowledge-base.git
cd knowledge-base
```

### Setup GitHub Pages

1. Go to repository **Settings** → **Pages**
2. Source: **GitHub Actions**
3. Select **Jekyll** as static site generator

### Create GitHub Actions Workflows

#### Workflow 1: Jekyll Deployment

Create `.github/workflows/jekyll.yml` to build and deploy your site automatically.

#### Workflow 2: Auto Front Matter

Create `.github/workflows/add-frontmatter.yml` to automatically add Jekyll front matter to markdown files.

### Jekyll Configuration

Create `_config.yml`:

```yaml
title: Your Knowledge Base
description: AI-powered knowledge management
baseurl: "/knowledge-base"
url: "https://yourusername.github.io"

markdown: kramdown
plugins:
  - jekyll-feed
  - jekyll-seo-tag
  - jekyll-sitemap
```

## Component 4: Integration

### Mount Repository as Vault

```bash
# Option 1: Open folder as vault in Obsidian
# Option 2: Create symbolic link
ln -s ~/path/to/repo ~/Documents/Obsidian/YourVault
```

### Test the System

```bash
cd ~/path/to/vault
claude

# Test each command:
> /capture "Test idea"
> /capture https://youtube.com/watch?v=example
> /semantic-search "testing"
```

## Maintenance & Best Practices

### Regular Maintenance

- **Weekly**: Review and organize captured content
- **Monthly**: Check GitHub Actions status
- **Quarterly**: Update plugins and dependencies

### Backup Strategy

Configure Obsidian Git plugin for automatic backups:
- Auto-pull: every 10 minutes
- Auto-commit: every 30 minutes
- Auto-push: every hour

### Security

- Never commit `.env` files (add to `.gitignore`)
- Use GitHub secrets for tokens
- Regularly rotate API keys
- Review public content before publishing

## Customization

### Modify Commands

Edit `.claude/commands/*.md` to customize AI behavior for your workflow.

### Adjust Workflows

Update `.github/workflows/*.yml` to change publishing behavior.

### Add Features

Create new MCP servers or skills in `~/.claude/skills/` for additional capabilities.

---

## Troubleshooting

### Smart Connections Not Working

**Solution**: Check Local REST API is running and verify API key in `.env`

### GitHub Actions Failing

**Solution**: Check workflow logs, verify permissions, ensure Jekyll dependencies are installed

### Commands Not Found

**Solution**: Verify `.claude/commands/` directory exists and check command frontmatter syntax

### YouTube Transcript Fails

**Solution**: Install youtube_transcript_api: `pip install youtube_transcript_api`

---

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

---

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

*Transform information into knowledge. Capture chaos into clarity.*

*Last updated: 2025-10-22*
