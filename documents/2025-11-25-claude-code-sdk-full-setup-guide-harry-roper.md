---
title: "NEW Claude Code SDK - Full Setup Guide"
type: video
source: youtube
url: https://youtu.be/dVfW6Xsx2H8
channel: Harry Roper
duration: 19 minutes
date_published: 2025-10-27
date_captured: 2025-11-25
tags:
  - video
  - AI
  - Claude
  - development
  - coding
  - tutorial
  - technical
  - inbox
thumbnail: https://img.youtube.com/vi/dVfW6Xsx2H8/maxresdefault.jpg
---

# NEW Claude Code SDK - Full Setup Guide

## Video Information

| Field | Value |
|-------|-------|
| **Channel** | Harry Roper |
| **Duration** | 19 minutes |
| **Published** | 2025-10-27 |
| **URL** | [Watch on YouTube](https://youtu.be/dVfW6Xsx2H8) |

## Description

A Full guide on setting up your first Claude Code Agent with the new 4.5 SDK!

**Resources:**
- More tips: https://aclasswithharry.com
- Twitter/X: https://x.com/Harry_Aldian
- Discord: https://discord.gg/R8RKrrQgjS

## Summary

This tutorial walks through building an AI agent orchestrator system using Anthropic's Claude API and the new SDK. Harry Roper demonstrates how to create an autonomous agent that runs in a loop, makes its own decisions, manages state, and has persistent memory.

### Architecture Diagram

![[images/claude-sdk-orchestrator-architecture.png]]

*The orchestrator pattern illustrated above shows the central agent architecture. The **Orchestrator** sits at the core, connected to: (1) **Save and read Memory** for persistence across sessions, (2) **State Machine** enabling continuous loop operation, (3) **Tools** for direct capabilities, and (4) **Sub Agents** like Writer and Reviewer teams. Each sub-agent has specialized tools—the Writer can write files while the Reviewer can add to the database. This hierarchical design allows the orchestrator to delegate specialized tasks while maintaining overall control and memory state.*

## Key Concepts

### 1. Agent Architecture
- **Orchestrator**: The main agent that manages everything
  - Checks memory
  - Maintains objectives
  - Coordinates sub-agents
  - Has access to tools
- **Sub-agents**: Smaller specialized teams (e.g., writer + reviewer)
- **Tools**: Capabilities given to agents (file writing, database access, etc.)

### 2. State Machine Implementation
- Agent runs continuously in a loop
- Makes autonomous decisions without step-by-step workflow
- Uses hypothesis generation as a sub-agent
- Implements wait/sleep functionality as a tool

### 3. Memory System (Key Innovation)
- **Why local files over database**: Reading text files is faster than database calls
- **Memory tools provided**:
  - Write to memory (with tags for different keys)
  - Read individual memories
  - List all memories
- **Benefits**:
  - Agent can pick up where it left off after restart
  - Tracks iteration count and active tasks
  - Enables long-running agent sessions

## Technical Setup

### Prerequisites
```bash
# Install UV (Python package manager)
# Create virtual environment
uvm

# Activate virtual environment
source .venv/bin/activate

# Install dependencies
uvp pip install -r requirements.txt
```

### Key Packages
- `anthropic` - Claude API
- `python-dotenv` - Environment variable management
- `rich` - Better CLI experience

### Running the Agent
```bash
uv run main.py
```

## Project Structure (After Refactor)
```
project/
├── main.py              # Entry point
├── agents/              # Agent profiles
│   ├── orchestrator.py
│   └── sub_agents/
├── tools/               # Tool definitions
│   ├── memory.py
│   └── ...
├── .env                 # API keys
└── requirements.txt
```

## Learning Objectives

- [ ] Understand agent orchestrator pattern
- [ ] Implement state machine for continuous operation
- [ ] Add memory persistence for agents
- [ ] Use sub-agents for specialized tasks
- [ ] Refactor vibe-coded projects into clean structure

## Key Takeaways

1. **Orchestrator Pattern**: Central agent manages sub-agents and tools
2. **Memory is Crucial**: Local file-based memory faster than database for quick reads
3. **Autonomous Loop**: Agent makes its own decisions without manual steps
4. **Vibe Code → Refactor**: Start with MVP/prototype, then organize properly
5. **Claude Documentation**: Reference docs for tool calling and sub-agents

## Quotes

> "This is an agent running by itself making its own decisions. I'm not telling it what to do. There's no step-by-step workflow. Instead, I'm entrusting the model to make the right decisions."

> "It's actually a lot quicker to read a text file than it is to do a database call and wait for that information to come back. It's a simple hack that the Claude team have recently introduced."

## Related Topics

- Claude Code SDK
- Agentic AI patterns
- Python automation
- State machines
- Memory-augmented agents

## Transcript

[music]
Let's talk about Asians.
So, there's something I've not really
been honest to you guys about, which is
that I've been working on a secret
stealth startup for the last year that
recently got funded. So, in today's
video, I kind of want to share what I've
been building with agents, how you can
build your own robot, and watch as a
Claude agent makes its own decisions.
It's mind-blowing. Let's uh get into the
video.

[music]

All right, guys. So, we're inside my
computer now, and I wanted to start at
the whiteboard just to kind of explain
the concept to you so you guys
understand what we're building.
Basically, what we're going to do here
is use Anthropics API to be able to help
us run an agent. And what you call an
agent who's like managing everything.
They're the orchestrator. They're the
agent that's telling people what to do,
checking its memory, making sure that
it's on its objective. And there's going
to be a few things that this
orchestrator can do. First of all, he's
going to be able to save and read his
memory. He's going to have a state
machine attached so that he runs in a
loop. And then after that, access to a
few sub agents who can do some work for
him. And then any tools that we want to
give it as well. Sub agents are
obviously these like smaller teams that
do really good. A sub agent might be a
writer and a reviewer for example and
then they'll have their own tools
whether that's like to tools to write
files or maybe a tool to add something
to the DB. As you can see the
orchestrator is kind of like the glue in
the middle that organizes and manages
our agents. And we give each of our
agents, whether it's the orchestrator or
the teams that are working under the
orchestrator, specific tools that they
can use.

All right, now that I've explained that concept, let's get
into cursor and start making our first
orchestrator. Okay, so we're inside our
agent editing tool inside cursor and I'm
going to come in here and just tag the
claude agent docs to begin with. I'm
then going to say uh make an
orchestrator that has state management
and runs in a loop. really simple
prompt. Whilst that's running, there's a
few little prerequisites we need to do
to be able to set this Python project
up. So, I'm going to use UV. It's a
package manager that allows you to
easily install all of the packages that
you need for a Python project.

[Transcript continues with detailed implementation steps...]

---

## Tag Analysis

**Content Type**: video (tutorial format)
**Topics**: AI, Claude, development, coding (core technical content)
**Status**: inbox (newly captured)
**Metadata**: tutorial (step-by-step guide), technical (code-heavy content)

### Bases Filtering Suggestions
- `type = video AND tags contains "Claude"` - Find Claude-related video tutorials
- `tags contains "tutorial" AND tags contains "AI"` - AI tutorial collection
- `tags contains "development" AND tags contains "coding"` - Developer resources
