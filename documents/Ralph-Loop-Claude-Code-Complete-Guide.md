---
title: "Ralph Loop: Complete Guide to Autonomous Claude Code"
tags: [video, repository, AI, Claude, automation, coding, development, tools, actionable, deep-dive, evergreen]
type: study-guide
status: evergreen
priority: high
date: 2026-01-14
author: Zorro (compiled)
channel: Multiple Sources (Compiled)
consolidated_from:
  - 2025-12-29-aicodeking-ralph-loop-claude-code-framework-9Gv7eZemHrE.md
  - 2025-12-30-developers-digest-claude-code-autonomous-stop-hooks-o-pMCoVPN_k.md
  - 2025-12-30-anthropic-ralph-wiggum-plugin-iterative-ai-loops.md
---

# Ralph Loop: Complete Guide to Autonomous Claude Code

![Ralph Loop - Autonomous Claude Code](https://img.youtube.com/vi/9Gv7eZemHrE/maxresdefault.jpg)

> "Ralph is a Bash loop" — Geoffrey Huntley

## What is Ralph Loop?

The **Ralph Wiggum plugin** enables Claude Code to run autonomously for hours by intercepting exit attempts and forcing task completion. Named after the Simpsons character embodying persistent iteration despite setbacks.

**Core Problem Solved**: AI agents quit early, hallucinate success, or give up after one attempt.

**Solution**: Stop hooks intercept exit attempts and feed the same prompt back until a completion promise is met.

## Quick Start

```bash
# Install the plugin
# (from anthropics/claude-code/plugins/ralph-wiggum)

# Basic usage
/ralph-loop "Your task" --max-iterations 20 --completion-promise "COMPLETE"
```

## How It Works

```
┌─────────────────────────────────────────┐
│  1. You run /ralph-loop with prompt     │
├─────────────────────────────────────────┤
│  2. Claude works on task                │
├─────────────────────────────────────────┤
│  3. Claude tries to exit                │
├─────────────────────────────────────────┤
│  4. Stop hook intercepts exit           │
│     - Checks for completion promise     │
│     - If not found → feed prompt back   │
│     - If found → allow exit             │
├─────────────────────────────────────────┤
│  5. Repeat until complete or max iter   │
└─────────────────────────────────────────┘
```

**Key insight**: Prompts never change between iterations, but Claude's work persists in files and git history. Each iteration sees modified files.

## Benchmarks & Real Results

### Autonomous Runtime (Meter Benchmark)
| Model | Runtime @ 50% | Runtime @ 80% |
|-------|---------------|---------------|
| Claude Opus 4.5 | **4h 49m** | Lower |
| GPT-4 (original) | 5 min | - |

### Boris Journey (Claude Code Creator)
- **259 PRs** in 30 days
- **457 commits**
- **40,000+ lines** added/removed
- All written by Claude Code with Opus 4.5

### Geoffrey Huntley
- 6 repositories generated overnight (Y Combinator hackathon)
- $50k contract completed for **$297 in API costs**
- Created entire programming language over 3 months

## Technical Implementation

### Directory Structure
```
ralph-wiggum/
├── .claude-plugin         # Plugin configuration
├── commands/
│   └── ralph-loop.md      # /ralph-loop command
├── hooks/
│   └── stop-hook.sh       # Stop hook intercepts exit
├── scripts/
│   └── setup-ralph-loop.sh
└── README.md
```

### State File Format
Location: `.claude/ralph-loop.local.md`

```markdown
---
iteration: 1
max_iterations: 50
completion_promise: "COMPLETE"
---

Your task prompt here...
```

### Completion Promise Detection
Uses exact string matching with `<promise>` XML tags:

```bash
PROMISE_TEXT=$(echo "$LAST_OUTPUT" | perl -0777 -pe 's/.*?<promise>(.*?)<\/promise>.*/$1/s')
if [[ "$PROMISE_TEXT" = "$COMPLETION_PROMISE" ]]; then
  # Loop complete
fi
```

## Writing Effective Prompts

### Binary Success Criteria

✅ **Good criteria** (verifiable):
- Tests passing
- Code compiling
- Coverage targets met
- Linter passing
- Build succeeding

❌ **Bad criteria** (subjective):
- "Make it good"
- "Make it pretty"
- "Improve the code"

### Prompt Template

```markdown
/ralph-loop "Build [FEATURE] with [TECH STACK].

Requirements:
- [Specific requirement 1]
- [Specific requirement 2]

Validation:
1. Write tests first (TDD)
2. Implement feature
3. Run tests
4. If tests fail, debug and fix
5. Repeat until all green

When complete:
- All CRUD endpoints working
- Tests passing (coverage > 80%)
- README with docs

Output: <promise>COMPLETE</promise>" 
--max-iterations 20
--completion-promise "COMPLETE"
```

### Real Example

```bash
/ralph-loop "Build a movie tracker with Next.js and Supabase. 
Implement dark mode. Write test suite. Run tests. 
If tests fail, fix the code. 
Do not stop until all tests pass." 
--completion-promise complete
--max-iterations 20
```

## Todo List Pattern

Create a `task.md` for complex multi-step work:

```markdown
# task.md
- [ ] Implement feature A
- [ ] Run unit tests
- [ ] Implement feature B  
- [ ] Run integration tests
- [ ] Final validation
```

Prompt: "Go through task.md step by step, mark items complete, run tests after each step"

## Hook Types in Claude Code

| Hook Type | When It Fires | Use Case |
|-----------|---------------|----------|
| **Pre-tool use** | Before tool invocation | Block dangerous commands |
| **Post-tool use** | After tool completion | Logging, notifications |
| **Permission request** | When permission needed | Auto-approve certain tools |
| **Stop hook** | When Claude tries to exit | **Ralph Loop mechanism** |

You can **stack multiple hooks** together for logging, notifications, etc.

## Safety Mechanisms

### Always Set Max Iterations
```bash
--max-iterations 20  # Prevents infinite loops
```

### Loop Integrity Rules
The command instructs Claude:
- Promise statements MUST be completely TRUE
- Do NOT output false statements to exit
- Even if stuck, do NOT lie
- Trust the process

### Security Fixes (in current version)
- Removed `eval` usage (command injection risk)
- Added numeric validation before arithmetic
- Removed silent error suppression
- Input validation for all arguments

## When to Use / Not Use

### Good For
- Well-defined tasks with clear success criteria
- Tasks requiring iteration (getting tests to pass)
- Greenfield projects (walk away overnight)
- Tasks with automatic verification

### Not Good For
- Tasks requiring human judgment
- One-shot operations
- Unclear success criteria
- Production debugging

## Commands Reference

| Command | Purpose |
|---------|---------|
| `/ralph-loop "prompt"` | Start autonomous loop |
| `/cancel-ralph` | Cancel active loop |
| `--max-iterations N` | Safety limit |
| `--completion-promise "text"` | Exit condition |

## Cost Considerations

- **Opus 4.5**: ~$25/million output tokens
- Long runs can accumulate costs
- Geoffrey Huntley's $50k project = $297 API costs (good ROI)

## Resources

- [Ralph Wiggum Plugin (GitHub)](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum)
- [Original Ralph Technique (ghuntley.com)](https://ghuntley.com/ralph/)
- [Ralph Orchestrator](https://github.com/mikeyobrien/ralph-orchestrator)

## Source Videos

- [AICodeKing: Ralph Loop Framework](https://www.youtube.com/watch?v=9Gv7eZemHrE) (8 min) - Prompt writing focus
- [Developers Digest: Autonomous Claude Code](https://www.youtube.com/watch?v=o-pMCoVPN_k) (13 min) - Overview + Boris stats

---

*Consolidated: 2025-12-30 from 3 related notes*
