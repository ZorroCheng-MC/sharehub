---
title: "NEW Claude Code SDK - Full Setup Guide"
type: video
source: youtube
url: https://youtu.be/dVfW6Xsx2H8
channel: Harry Roper
duration: 19 minutes
date: 2026-02-09
date_published: 2025-10-27
date_captured: 2025-11-25
priority: high
tags:
  - video
  - AI
  - Claude
  - development
  - coding
  - automation
  - tutorial
  - technical
  - inbox
thumbnail: https://img.youtube.com/vi/dVfW6Xsx2H8/maxresdefault.jpg
---

# NEW Claude Code SDK - Full Setup Guide

## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/dVfW6Xsx2H8" frameborder="0" allowfullscreen></iframe>

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

Harry Roper provides a comprehensive walkthrough of building an AI agent using Anthropic's Claude Code SDK. The tutorial covers creating an orchestrator agent with state management, sub-agents for task delegation, memory persistence using local markdown files, and tool integration. This is a practical, hands-on guide for developers wanting to build autonomous AI systems that can run 24/7.

### Architecture Diagram

![Claude SDK Orchestrator Architecture](/sharehub/images/claude-sdk-orchestrator-architecture.png)

*The orchestrator pattern illustrated above shows the central agent architecture. The **Orchestrator** sits at the core, connected to: (1) **Save and read Memory** for persistence across sessions, (2) **State Machine** enabling continuous loop operation, (3) **Tools** for direct capabilities, and (4) **Sub Agents** like Writer and Reviewer teams. Each sub-agent has specialized tools—the Writer can write files while the Reviewer can add to the database. This hierarchical design allows the orchestrator to delegate specialized tasks while maintaining overall control and memory state.*

## Curriculum

### Module 1: Architecture Overview (0:00-2:00)
- [ ] Orchestrator as central "glue" managing sub-agents
- [ ] Sub-agents for specialized tasks (writer, reviewer)
- [ ] Tools attached to each agent layer
- [ ] State machine for continuous loop operation

### Module 2: Project Setup (2:00-5:00)
- [ ] UV package manager installation
- [ ] Virtual environment creation (`uv venv`)
- [ ] Installing dependencies from requirements.txt
- [ ] Setting up .env for API keys

### Module 3: Building the Orchestrator (5:00-10:00)
- [ ] Creating state management
- [ ] Implementing the main loop
- [ ] Adding Rich library for better CLI output
- [ ] Hypothesis sub-agent for decision making
- [ ] Wait/sleep tool for pacing

### Module 4: Memory System (10:00-15:00)
- [ ] Why local markdown files beat database calls
- [ ] Memory tools: write, read, list
- [ ] Tagged memory entries with keys
- [ ] Agent bootstrapping from existing memories
- [ ] Persistence across restarts

### Module 5: Refactoring (15:00-19:00)
- [ ] Clean project structure best practices
- [ ] Separating agents into modules
- [ ] Tools in dedicated folder
- [ ] Documentation with README
- [ ] Future extensibility

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

## Main Insights

1. **Orchestrator Pattern**: The orchestrator is the central "glue" that manages sub-agents, checks memory, and stays on objective - it doesn't do the heavy lifting itself

2. **Local Memory > Database**: Reading a markdown file is faster than database calls - a simple hack from the Claude team that makes agents "spooky" powerful

3. **Sub-agents for Thinking**: Offload complex reasoning to sub-agents (like hypothesis generation) to avoid overwhelming the orchestrator

4. **State Machine Loop**: Agent runs continuously, choosing between actions (hypothesis) or waiting (sleep) - no step-by-step workflow, just autonomous decision-making

5. **Clear Memory on Refactor**: Always clear agent memories when making structural changes to prevent old memories from interfering with new behavior

## Technical Setup

### Prerequisites
```bash
# Install UV (Python package manager)
# Create virtual environment
uv venv

# Activate virtual environment
source .venv/bin/activate

# Install dependencies
uv pip install -r requirements.txt
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

## Actionable Points

- [ ] Install UV package manager for Python projects
- [ ] Use Rich library for better CLI agent output
- [ ] Implement memory as local markdown files, not database
- [ ] Create separate modules for agents, tools, and orchestrator
- [ ] Tag memories with keys for efficient retrieval
- [ ] Run memory cleanup before testing new agent versions

## Quotes

> "This is an agent running by itself making its own decisions. I'm not telling it what to do. There's no step-by-step workflow. Instead, I'm entrusting the model to make the right decisions."

> "It's actually a lot quicker to read a text file than it is to do a database call and wait for that information to come back. It's a simple hack that the Claude team have recently introduced."

## Target Audience

Developers interested in building autonomous AI agents using Claude's SDK. Requires basic Python knowledge and familiarity with virtual environments. Best suited for those wanting to move beyond simple API calls to agentic systems.

## Resources

- [Anthropic Claude Agent SDK Documentation](https://docs.anthropic.com)
- [UV Package Manager](https://github.com/astral-sh/uv)
- [Rich Python Library](https://github.com/Textualize/rich)
- [Harry's Discord](https://discord.gg/R8RKrrQgjS)
- [Harry on X/Twitter](https://x.com/Harry_Aldian)
- [A Class with Harry](https://aclasswithharry.com)

## Related Topics

- Claude Code SDK
- Agentic AI patterns
- Python automation
- State machines
- Memory-augmented agents

## Connections

- [[Claude Code SDK Documentation]]
- [[AI Agent Architecture Patterns]]
- [[Python Development Tools]]

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
**Topics**: AI, Claude, development, coding, automation (core technical content)
**Status**: inbox (newly captured)
**Priority**: high - Directly relevant to building Claude Code agents
**Metadata**: tutorial (step-by-step guide), technical (code-heavy content)

### Bases Filtering Suggestions
- `type = video AND tags contains "Claude"` - Find Claude-related video tutorials
- `tags contains "tutorial" AND tags contains "AI"` - AI tutorial collection
- `tags contains "development" AND tags contains "coding"` - Developer resources
- `tags contains "technical" AND tags contains "automation"` - Autonomous systems

---
*Captured: 2025-11-25*
*Channel: Harry Roper*
*Duration: 19 minutes*
