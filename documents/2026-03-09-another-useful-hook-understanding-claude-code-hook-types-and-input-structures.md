---
title: "Another Useful Hook! Understanding Claude Code Hook Types and Input Structures"
tags: [Claude, coding, hooks, validation, debugging, automation, developer-tools, technical, best-practices, deep-dive, json, jq]
date: 2026-03-09
type: article
status: published
priority: high
difficulty: intermediate
author: Zororo
---

# Another Useful Hook! Understanding Claude Code Hook Types and Input Structures

## Executive Summary

Claude Code hooks have evolved significantly beyond basic command blocking. Understanding the full spectrum of hook types—from `PreToolUse` and `PostToolUse` to `Notification`, `Stop`, `SubagentStop`, `PreCompact`, `UserPromptSubmit`, `SessionStart`, and `SessionEnd`—unlocks powerful patterns for agent automation, validation, and observability.

This comprehensive guide explores all hook types, their stdin/stdout input variations, tool-specific inputs, and practical debugging techniques using `jq`. You'll learn not just *what* hooks do, but how to read their input data and build complex validation pipelines.

---

## Table of Contents

1. [Hook Architecture Fundamentals](#hook-architecture-fundamentals)
2. [All Hook Types (8 Total)](#all-hook-types-8-total)
3. [Hook Input Structures & JSON](#hook-input-structures--json)
4. [Stdin/Stdout Variations](#stdinstdout-variations)
5. [Tool-Specific Hook Inputs](#tool-specific-hook-inputs)
6. [Practical Debugging with jq](#practical-debugging-with-jq)
7. [Real-World Implementation Examples](#real-world-implementation-examples)
8. [Advanced Patterns](#advanced-patterns)

---

## Hook Architecture Fundamentals

### What Are Hooks?

Hooks are **blocking interceptors** that execute at specific lifecycle points in Claude Code. They sit between the agent's intent and the actual action, allowing you to:

- **Validate** before/after tool execution
- **Log** every action for observability
- **Block** dangerous operations
- **Transform** data flowing through the system
- **Notify** external systems of state changes

### Hook Execution Model

```
Agent Decision
    ↓
[PreToolUse Hook] ← Can block execution
    ↓
Tool Executes (Read, Write, Edit, Exec, etc.)
    ↓
[PostToolUse Hook] ← Can validate results
    ↓
[Notification Hook] ← Informational only (can't block)
    ↓
Agent Continues
```

### Hook State & Context

Each hook receives:
- **What changed** (file path, command, etc.)
- **Tool arguments** (exact parameters passed)
- **Workspace context** (other files, directory structure)
- **Agent metadata** (model, session ID, iteration count)

---

## All Hook Types (8 Total)

### 1. PreToolUse Hook

**When it fires:** Before any tool (Read, Write, Edit, Exec) executes  
**Can block:** Yes (exit code 1)  
**Typical use:** Permission checks, command validation, rate limiting

#### Configuration

```yaml
hooks:
  pre_tool_use:
    - tools: [Write, Edit, Exec]
      patterns: ["**/*.ts", "**/*.py"]
      command: "python .claude/hooks/pre_tool_validator.py"
      timeout_seconds: 5
```

#### Input to Hook

The hook receives via stdin a JSON object with:

```json
{
  "tool": "Write",
  "file_path": "/Users/zorro/project/src/main.ts",
  "arguments": {
    "file_path": "/Users/zorro/project/src/main.ts",
    "content": "export const greeting = 'hello';"
  },
  "workspace_path": "/Users/zorro/project",
  "session_id": "session_abc123",
  "model": "claude-opus-4-5",
  "iteration": 3,
  "agent_id": "main-agent"
}
```

#### Hook Response

```bash
exit 0  # Allow execution
# or
exit 1  # Block execution (stderr contains error message)
```

Example blocking hook:

```python
#!/usr/bin/env python3
import json
import sys

data = json.loads(sys.stdin.read())

# Block writes to sensitive paths
blocked_paths = ['/etc', '/System', '/Applications']
if any(data['file_path'].startswith(p) for p in blocked_paths):
    print(f"Blocked: Cannot write to {data['file_path']}", file=sys.stderr)
    sys.exit(1)

sys.exit(0)
```

---

### 2. PostToolUse Hook

**When it fires:** After a tool completes successfully  
**Can block:** Yes (exit code 1 causes agent to receive error)  
**Typical use:** Result validation, file format checking, consistency verification

#### Configuration

```yaml
hooks:
  post_tool_use:
    - tools: [Write, Edit]
      patterns: ["**/*.json", "**/*.yaml"]
      command: "bash .claude/hooks/validate_format.sh"
```

#### Input to Hook

```json
{
  "tool": "Write",
  "file_path": "/Users/zorro/project/config.json",
  "arguments": {
    "file_path": "/Users/zorro/project/config.json",
    "content": "{\"name\": \"app\", \"version\": \"1.0.0\"}"
  },
  "result": {
    "status": "success",
    "file_size": 42,
    "created_at": "2026-03-09T14:22:45Z"
  },
  "workspace_path": "/Users/zorro/project"
}
```

#### Example: JSON Validation Hook

```bash
#!/bin/bash

HOOK_INPUT=$(cat)
FILE_PATH=$(echo "$HOOK_INPUT" | jq -r '.file_path')

# Validate JSON structure
if ! jq empty "$FILE_PATH" 2>/dev/null; then
    echo "Invalid JSON in $FILE_PATH" >&2
    exit 1
fi

# Ensure required fields exist
if ! jq -e '.name and .version' "$FILE_PATH" >/dev/null; then
    echo "Missing required fields: name, version" >&2
    exit 1
fi

exit 0
```

---

### 3. Notification Hook

**When it fires:** After any significant action (informational only)  
**Can block:** No (exit code ignored)  
**Typical use:** Logging, alerting, external system updates

#### Configuration

```yaml
hooks:
  notification:
    - tools: [Write, Edit, Exec]
      command: "bash .claude/hooks/log_action.sh"
```

#### Input to Hook

```json
{
  "event_type": "tool_complete",
  "tool": "Exec",
  "command": "npm test",
  "status": "success",
  "output": "Tests passed: 42/42",
  "timestamp": "2026-03-09T14:22:45Z",
  "session_id": "session_abc123"
}
```

#### Example: Audit Logging

```bash
#!/bin/bash

HOOK_INPUT=$(cat)

# Log to audit file
echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) | $HOOK_INPUT" >> .claude/audit.log

# Send to external service (non-blocking)
if command -v curl &> /dev/null; then
    curl -X POST \
        -H "Content-Type: application/json" \
        -d "$HOOK_INPUT" \
        "https://logs.example.com/claude-code" \
        &  # Background, don't wait
fi

exit 0
```

---

### 4. Stop Hook

**When it fires:** When agent reaches `stop` or max iterations  
**Can block:** Yes (can modify completion behavior)  
**Typical use:** Autonomous loop continuation, final validation, report generation

#### Configuration

```yaml
hooks:
  stop:
    - patterns: ["**/*.ts"]
      command: "bash .claude/hooks/final_validation.sh"
      timeout_seconds: 30
```

#### Input to Hook

```json
{
  "reason": "user_stop",
  "session_id": "session_abc123",
  "iterations": 12,
  "max_iterations": 20,
  "files_modified": [
    "src/main.ts",
    "src/utils.ts",
    "test/main.test.ts"
  ],
  "total_tokens_used": 45000,
  "completion_promise": "All tests pass",
  "completion_status": "pending"
}
```

#### Example: Ralph Loop Pattern

```bash
#!/bin/bash

HOOK_INPUT=$(cat)

# Parse JSON
ITERATIONS=$(echo "$HOOK_INPUT" | jq -r '.iterations')
MAX_ITERATIONS=$(echo "$HOOK_INPUT" | jq -r '.max_iterations')
COMPLETION_STATUS=$(echo "$HOOK_INPUT" | jq -r '.completion_status')

# If not done and iterations remain, signal to continue
if [ "$COMPLETION_STATUS" != "complete" ] && [ "$ITERATIONS" -lt "$MAX_ITERATIONS" ]; then
    echo '{"action": "continue", "reason": "Not done yet, continuing autonomous loop"}'
    exit 0
else
    echo '{"action": "stop", "reason": "Task complete or max iterations reached"}'
    exit 0
fi
```

---

### 5. SubagentStop Hook

**When it fires:** When a sub-agent completes  
**Can block:** Yes (can require remediation)  
**Typical use:** Sub-agent result validation, error recovery, orchestration logic

#### Configuration

```yaml
hooks:
  subagent_stop:
    - command: "python .claude/hooks/subagent_validator.py"
      timeout_seconds: 10
```

#### Input to Hook

```json
{
  "subagent_id": "writer-agent",
  "subagent_label": "Write markdown article",
  "completion_reason": "complete",
  "status": "success",
  "iterations": 8,
  "tokens_used": 12000,
  "files_generated": ["article.md"],
  "errors": [],
  "output_summary": "Article written with proper formatting"
}
```

#### Example: Sub-Agent Validation

```python
#!/usr/bin/env python3
import json
import sys

data = json.loads(sys.stdin.read())

# Validate required outputs exist
required_files = ['article.md']
for required_file in required_files:
    if required_file not in data['files_generated']:
        print(f"ERROR: Sub-agent did not generate {required_file}", file=sys.stderr)
        sys.exit(1)

# Check for errors
if data['errors']:
    print(f"Sub-agent had errors: {data['errors']}", file=sys.stderr)
    sys.exit(1)

# Log successful completion
print(f"✓ Sub-agent {data['subagent_id']} completed successfully", file=sys.stdout)
sys.exit(0)
```

---

### 6. PreCompact Hook

**When it fires:** Before Claude Code compacts/truncates context  
**Can block:** Yes (exit code 1 cancels compaction)  
**Typical use:** Memory checkpointing, state preservation, checkpoint validation

#### Configuration

```yaml
hooks:
  pre_compact:
    - command: "python .claude/hooks/checkpoint_handler.py"
      timeout_seconds: 10
```

#### Input to Hook

```json
{
  "reason": "context_limit_approaching",
  "current_tokens": 95000,
  "max_tokens": 100000,
  "session_id": "session_abc123",
  "files_in_context": ["src/main.ts", "src/utils.ts", "config.json"],
  "recent_changes": ["src/main.ts", "src/utils.ts"]
}
```

#### Example: Checkpoint Management

```python
#!/usr/bin/env python3
import json
import sys
from datetime import datetime

data = json.loads(sys.stdin.read())

# Create checkpoint before compaction
checkpoint = {
    "timestamp": datetime.now().isoformat(),
    "reason": data['reason'],
    "tokens_used": data['current_tokens'],
    "files_modified": data['recent_changes']
}

# Save checkpoint
with open('.claude/checkpoints.jsonl', 'a') as f:
    f.write(json.dumps(checkpoint) + '\n')

print(f"✓ Checkpoint created before compaction", file=sys.stdout)
sys.exit(0)
```

---

### 7. UserPromptSubmit Hook

**When it fires:** When user submits a new message/prompt  
**Can block:** Yes (can reject invalid inputs)  
**Typical use:** Input validation, prompt safety checks, instruction injection prevention

#### Configuration

```yaml
hooks:
  user_prompt_submit:
    - command: "python .claude/hooks/prompt_validator.py"
```

#### Input to Hook

```json
{
  "prompt_text": "Write a file that deletes /tmp/*",
  "user_id": "user_abc123",
  "session_id": "session_abc123",
  "model": "claude-opus-4-5",
  "is_edit": false,
  "context_window_size": 45000
}
```

#### Example: Prompt Injection Prevention

```python
#!/usr/bin/env python3
import json
import sys
import re

data = json.loads(sys.stdin.read())
prompt = data['prompt_text'].lower()

# Block dangerous instruction injections
dangerous_patterns = [
    r'ignore.*previous.*instruction',
    r'admin.*mode',
    r'system.*prompt',
    r'delete.*production',
    r'rm.*-rf'
]

for pattern in dangerous_patterns:
    if re.search(pattern, prompt):
        print(f"Prompt rejected: Contains potentially dangerous instruction", file=sys.stderr)
        sys.exit(1)

print(f"✓ Prompt validation passed", file=sys.stdout)
sys.exit(0)
```

---

### 8. SessionStart & SessionEnd Hooks

**When it fires:** At session initialization (Start) and completion (End)  
**Can block:** Start hook yes, End hook no  
**Typical use:** Session setup/teardown, initialization checks, final reporting

#### Configuration

```yaml
hooks:
  session_start:
    - command: "bash .claude/hooks/session_init.sh"
  session_end:
    - command: "bash .claude/hooks/session_cleanup.sh"
```

#### SessionStart Input

```json
{
  "session_id": "session_abc123",
  "model": "claude-opus-4-5",
  "workspace_path": "/Users/zorro/project",
  "user_id": "user_abc123",
  "capabilities": ["read", "write", "edit", "exec"],
  "max_iterations": 50
}
```

#### SessionEnd Input

```json
{
  "session_id": "session_abc123",
  "duration_seconds": 1800,
  "total_iterations": 25,
  "total_tokens_used": 78000,
  "completion_reason": "user_stop",
  "final_status": "success",
  "files_modified": 5,
  "errors_encountered": 0
}
```

#### Example: Session Setup & Reporting

```bash
#!/bin/bash

HOOK_TYPE=$1
HOOK_INPUT=$(cat)

if [ "$HOOK_TYPE" = "start" ]; then
    echo "📍 Session starting..."
    WORKSPACE=$(echo "$HOOK_INPUT" | jq -r '.workspace_path')
    
    # Initialize workspace state
    mkdir -p "$WORKSPACE/.claude/state"
    echo '{"started": true}' > "$WORKSPACE/.claude/state/session.json"
    
elif [ "$HOOK_TYPE" = "end" ]; then
    echo "🏁 Session ending..."
    DURATION=$(echo "$HOOK_INPUT" | jq -r '.duration_seconds')
    TOKENS=$(echo "$HOOK_INPUT" | jq -r '.total_tokens_used')
    
    echo "Duration: ${DURATION}s | Tokens: ${TOKENS}"
fi
```

---

## Hook Input Structures & JSON

### Universal Hook Structure

All hooks receive a JSON object with:

```json
{
  "hook_type": "pre_tool_use",
  "timestamp": "2026-03-09T14:22:45Z",
  "session_id": "session_abc123",
  "workspace_path": "/Users/zorro/project",
  "[hook-specific-fields]": {}
}
```

### Common Fields Across Hooks

| Field | Type | Description |
|-------|------|-------------|
| `session_id` | string | Unique session identifier |
| `timestamp` | ISO8601 | When hook fired |
| `workspace_path` | string | Project root directory |
| `model` | string | Claude model in use |
| `iteration` | number | Current iteration count |

### Tool-Specific Fields in PreToolUse

```json
{
  "tool": "Write|Edit|Read|Exec",
  "arguments": {
    // For Write:
    "file_path": "string",
    "content": "string",
    
    // For Edit:
    "file_path": "string",
    "old_text": "string",
    "new_text": "string",
    
    // For Read:
    "file_path": "string",
    
    // For Exec:
    "command": "string",
    "cwd": "string"
  }
}
```

---

## Stdin/Stdout Variations

### How Hooks Receive Input

There are three patterns:

#### Pattern 1: stdin JSON (Most Common)

Hook receives JSON via stdin:

```bash
#!/bin/bash
# Read from stdin
HOOK_INPUT=$(cat)
echo "$HOOK_INPUT" | jq '.file_path'
```

**Configuration:**
```yaml
command: "bash my_hook.sh"
```

#### Pattern 2: Command-Line Arguments

Hook receives file path as argument:

```bash
#!/bin/bash
# Receive via argv
FILE_PATH=$1
cat "$FILE_PATH" | jq '.'
```

**Configuration:**
```yaml
command: "bash my_hook.sh {file_path}"
# or
command: "bash my_hook.sh {arguments.file_path}"
```

#### Pattern 3: Environment Variables

Hook receives metadata via env vars:

```bash
#!/bin/bash
# Receive via env
echo "Session: $CLAUDE_SESSION_ID"
echo "Workspace: $CLAUDE_WORKSPACE"
echo "Tool: $CLAUDE_TOOL"
```

**Claude Code sets these automatically:**
- `CLAUDE_SESSION_ID`
- `CLAUDE_WORKSPACE`
- `CLAUDE_TOOL`
- `CLAUDE_FILE_PATH`
- `CLAUDE_ITERATION`

### Hook Response Patterns

#### Pattern 1: Exit Code Only

```bash
exit 0  # Success/Allow
exit 1  # Failure/Block
```

#### Pattern 2: Exit Code + stderr

```bash
echo "Detailed error message" >&2
exit 1
```

#### Pattern 3: Exit Code + stdout

```bash
echo '{"status": "ok", "data": {...}}'
exit 0
```

#### Pattern 4: File-Based Response

```bash
# Write response to file
echo '{"action": "continue"}' > .claude/hook_response.json
exit 0
```

---

## Tool-Specific Hook Inputs

### Write Hook Input

```json
{
  "tool": "Write",
  "file_path": "/Users/zorro/project/src/utils.ts",
  "arguments": {
    "file_path": "/Users/zorro/project/src/utils.ts",
    "content": "export function add(a: number, b: number): number {\n  return a + b;\n}"
  },
  "file_extension": ".ts",
  "file_size_bytes": 65,
  "will_overwrite": false,
  "parent_directory_exists": true
}
```

### Edit Hook Input

```json
{
  "tool": "Edit",
  "file_path": "/Users/zorro/project/src/utils.ts",
  "arguments": {
    "file_path": "/Users/zorro/project/src/utils.ts",
    "old_text": "export function add(a, b) {",
    "new_text": "export function add(a: number, b: number): number {"
  },
  "old_text_line_number": 1,
  "old_text_length": 28,
  "new_text_length": 51
}
```

### Read Hook Input

```json
{
  "tool": "Read",
  "file_path": "/Users/zorro/project/src/utils.ts",
  "arguments": {
    "file_path": "/Users/zorro/project/src/utils.ts"
  },
  "file_size_bytes": 500,
  "file_exists": true,
  "readable": true
}
```

### Exec Hook Input

```json
{
  "tool": "Exec",
  "arguments": {
    "command": "npm test",
    "cwd": "/Users/zorro/project"
  },
  "command": "npm test",
  "working_directory": "/Users/zorro/project",
  "shell": "bash",
  "timeout_seconds": 300
}
```

---

## Practical Debugging with jq

### jq Basics for Hook Debugging

#### Pretty-print incoming JSON

```bash
#!/bin/bash
cat | jq '.'
```

#### Extract single field

```bash
echo "$HOOK_INPUT" | jq '.file_path'
# Output: "/Users/zorro/project/src/main.ts"
```

#### Check if field exists

```bash
echo "$HOOK_INPUT" | jq 'has("file_path")'
# Output: true
```

#### Filter with conditions

```bash
# Check if file extension is .ts
echo "$HOOK_INPUT" | jq '.file_path | endswith(".ts")'
# Output: true
```

#### Array operations

```bash
# Check if array contains value
echo "$HOOK_INPUT" | jq '.files_modified | contains(["main.ts"])'
```

#### Extract multiple fields

```bash
echo "$HOOK_INPUT" | jq '{tool, file_path, timestamp}'
# Output: {"tool": "Write", "file_path": "...", "timestamp": "..."}
```

### Debugging Hook Failures

#### Create a debugging hook

```bash
#!/bin/bash
set -e

# Log all incoming data
HOOK_INPUT=$(cat)
echo "=== HOOK DEBUG ===" >&2
echo "$HOOK_INPUT" | jq '.' >&2
echo "=================" >&2

# Parse and validate
TOOL=$(echo "$HOOK_INPUT" | jq -r '.tool')
FILE=$(echo "$HOOK_INPUT" | jq -r '.file_path')

if [ -z "$TOOL" ] || [ -z "$FILE" ]; then
    echo "ERROR: Missing required fields" >&2
    exit 1
fi

echo "✓ Tool: $TOOL, File: $FILE" >&2
exit 0
```

#### Log hook executions

```bash
#!/bin/bash

HOOK_INPUT=$(cat)
TIMESTAMP=$(echo "$HOOK_INPUT" | jq -r '.timestamp')
TOOL=$(echo "$HOOK_INPUT" | jq -r '.tool')

# Append to hook log
echo "[$TIMESTAMP] $TOOL Hook Executed" >> .claude/hook_debug.log
echo "$HOOK_INPUT" | jq '.' >> .claude/hook_debug.log
echo "---" >> .claude/hook_debug.log

exit 0
```

#### Filter and validate JSON

```bash
#!/bin/bash

HOOK_INPUT=$(cat)

# Validate JSON structure
if ! echo "$HOOK_INPUT" | jq 'has("tool") and has("file_path")' | grep -q true; then
    echo "ERROR: Invalid hook input structure" >&2
    exit 1
fi

# Extract for processing
TOOL=$(echo "$HOOK_INPUT" | jq -r '.tool')
FILE=$(echo "$HOOK_INPUT" | jq -r '.file_path')

# Complex validation
if echo "$HOOK_INPUT" | jq -e '.arguments | keys | length > 0' >/dev/null; then
    echo "✓ Hook has arguments" >&2
fi

exit 0
```

### jq Advanced Patterns

#### Conditional logic

```bash
# If tool is Exec, check command
echo "$HOOK_INPUT" | jq '
  if .tool == "Exec" then
    .arguments.command
  else
    .file_path
  end
'
```

#### Map and filter

```bash
# Get all modified file paths
echo "$HOOK_INPUT" | jq '
  .files_modified | 
  map(select(endswith(".ts")))
'
```

#### Group by type

```bash
# Group errors by severity
echo "$HOOK_INPUT" | jq '
  group_by(.severity) |
  map({severity: .[0].severity, count: length})
'
```

---

## Real-World Implementation Examples

### Example 1: Comprehensive TypeScript Validator Hook

```python
#!/usr/bin/env python3
"""
Complete TypeScript validation hook.
- Runs ESLint pre-flight checks
- Validates types with TypeScript
- Logs all validations for debugging
"""

import json
import sys
import subprocess
from pathlib import Path
from datetime import datetime

class TypeScriptValidator:
    def __init__(self, hook_input: dict):
        self.data = hook_input
        self.file_path = self.data['file_path']
        self.errors = []
        self.warnings = []
    
    def validate(self) -> bool:
        """Run all validation steps."""
        self._validate_syntax()
        self._validate_types()
        self._validate_style()
        
        return len(self.errors) == 0
    
    def _validate_syntax(self):
        """Check TypeScript syntax."""
        try:
            result = subprocess.run(
                ['npx', 'tsc', '--noEmit', self.file_path],
                capture_output=True,
                timeout=5
            )
            if result.returncode != 0:
                self.errors.append(f"TypeScript compilation error: {result.stderr.decode()}")
        except Exception as e:
            self.warnings.append(f"Could not run TypeScript check: {str(e)}")
    
    def _validate_types(self):
        """Validate type annotations."""
        with open(self.file_path) as f:
            content = f.read()
        
        if 'function' in content and ': ' not in content:
            self.warnings.append("Functions may be missing type annotations")
    
    def _validate_style(self):
        """Check code style with ESLint."""
        try:
            result = subprocess.run(
                ['npx', 'eslint', self.file_path],
                capture_output=True,
                timeout=5
            )
            if result.returncode != 0:
                self.warnings.append(f"ESLint issues: {result.stdout.decode()}")
        except Exception as e:
            self.warnings.append(f"Could not run ESLint: {str(e)}")
    
    def report(self):
        """Print results and return exit code."""
        if self.errors:
            print(f"❌ TypeScript Validation Failed for {self.file_path}", file=sys.stderr)
            for error in self.errors:
                print(f"  ERROR: {error}", file=sys.stderr)
            return 1
        
        if self.warnings:
            print(f"⚠️  TypeScript Warnings for {self.file_path}", file=sys.stderr)
            for warning in self.warnings:
                print(f"  WARNING: {warning}", file=sys.stderr)
        
        print(f"✓ TypeScript validation passed: {self.file_path}", file=sys.stdout)
        return 0

if __name__ == '__main__':
    try:
        hook_input = json.loads(sys.stdin.read())
        validator = TypeScriptValidator(hook_input)
        
        if not validator.validate():
            exit_code = validator.report()
            sys.exit(exit_code)
        else:
            exit_code = validator.report()
            sys.exit(exit_code)
    except Exception as e:
        print(f"Hook error: {str(e)}", file=sys.stderr)
        sys.exit(1)
```

### Example 2: Database Query Deduplication Hook

```bash
#!/bin/bash
"""
PostToolUse hook that finds duplicate database queries.
Fires after Write/Edit tools complete.
"""

HOOK_INPUT=$(cat)

FILE_PATH=$(echo "$HOOK_INPUT" | jq -r '.file_path')
TOOL=$(echo "$HOOK_INPUT" | jq -r '.tool')

# Only check TypeScript/JavaScript files
if ! [[ "$FILE_PATH" =~ \.(ts|tsx|js|jsx)$ ]]; then
    exit 0
fi

# Extract all SQL queries from file
QUERIES=$(grep -oE 'db\.query\(`[^`]+`' "$FILE_PATH" 2>/dev/null || echo "")

if [ -z "$QUERIES" ]; then
    exit 0
fi

# Create temp file of query hashes
TEMP_HASHES=$(mktemp)
echo "$QUERIES" | sort | uniq -d > "$TEMP_HASHES"

if [ -s "$TEMP_HASHES" ]; then
    echo "⚠️ Found duplicate queries in $FILE_PATH:" >&2
    cat "$TEMP_HASHES" | head -5 >&2
    rm "$TEMP_HASHES"
    # Don't block, just warn
    exit 0
fi

rm "$TEMP_HASHES"
exit 0
```

### Example 3: Autonomous Loop with Stop Hook

```python
#!/usr/bin/env python3
"""
Stop hook implementing Ralph Wiggum pattern.
Continues autonomous agent loop if completion promise not met.
"""

import json
import sys
from datetime import datetime

class AutonomousLoopManager:
    def __init__(self, hook_input: dict):
        self.data = hook_input
        self.iterations = hook_input['iterations']
        self.max_iterations = hook_input['max_iterations']
        self.completion_promise = hook_input.get('completion_promise', '')
        self.completion_status = hook_input.get('completion_status', 'pending')
    
    def should_continue(self) -> bool:
        """Determine if agent should continue."""
        # Stop if max iterations reached
        if self.iterations >= self.max_iterations:
            return False
        
        # Stop if completion promise met
        if self.completion_status == 'complete':
            return False
        
        # Stop if user explicitly stopped
        if self.data.get('reason') == 'user_stop':
            return False
        
        # Continue if promise not met
        return True
    
    def generate_response(self) -> dict:
        """Generate hook response."""
        if self.should_continue():
            return {
                'action': 'continue',
                'reason': f'Continuing autonomous loop (iteration {self.iterations}/{self.max_iterations})',
                'next_task': 'Run failing tests and fix issues'
            }
        else:
            return {
                'action': 'stop',
                'reason': 'Task complete or max iterations reached',
                'summary': f'Completed in {self.iterations} iterations'
            }

if __name__ == '__main__':
    try:
        hook_input = json.loads(sys.stdin.read())
        manager = AutonomousLoopManager(hook_input)
        response = manager.generate_response()
        
        print(json.dumps(response), file=sys.stdout)
        sys.exit(0)
    except Exception as e:
        print(f"Error in stop hook: {str(e)}", file=sys.stderr)
        sys.exit(0)  # Don't block on hook errors
```

---

## Advanced Patterns

### Pattern 1: Multi-Stage Validation Pipeline

```yaml
hooks:
  pre_tool_use:
    - tools: [Write]
      patterns: ["**/*.ts"]
      command: "python .claude/hooks/stage1_syntax.py"
      timeout_seconds: 5
  
  post_tool_use:
    - tools: [Write]
      patterns: ["**/*.ts"]
      command: "python .claude/hooks/stage2_types.py"
      timeout_seconds: 10
  
  stop:
    - command: "python .claude/hooks/stage3_integration.py"
      timeout_seconds: 30
```

**Why:** Fast failures early, comprehensive checks at completion.

### Pattern 2: Conditional Hooks Based on File Type

```bash
#!/bin/bash

HOOK_INPUT=$(cat)
FILE_PATH=$(echo "$HOOK_INPUT" | jq -r '.file_path')

if [[ "$FILE_PATH" =~ \.ts$ ]]; then
    python .claude/hooks/typescript_validator.py <<< "$HOOK_INPUT"
elif [[ "$FILE_PATH" =~ \.py$ ]]; then
    python .claude/hooks/python_validator.py <<< "$HOOK_INPUT"
elif [[ "$FILE_PATH" =~ \.json$ ]]; then
    jq empty "$FILE_PATH" || exit 1
fi

exit $?
```

### Pattern 3: Hook Chaining with State

```python
#!/usr/bin/env python3
"""
Hook that chains multiple validations, maintaining state.
"""

import json
import sys
from pathlib import Path

class StatefulValidator:
    STATE_FILE = '.claude/hook_state.json'
    
    def __init__(self):
        self.state = self.load_state()
    
    def load_state(self) -> dict:
        if Path(self.STATE_FILE).exists():
            with open(self.STATE_FILE) as f:
                return json.load(f)
        return {'validations': 0, 'errors': 0}
    
    def save_state(self):
        Path(self.STATE_FILE).parent.mkdir(exist_ok=True)
        with open(self.STATE_FILE, 'w') as f:
            json.dump(self.state, f)
    
    def validate(self, hook_input: dict) -> bool:
        self.state['validations'] += 1
        
        # Run validation logic
        result = self.check_file(hook_input['file_path'])
        
        if not result:
            self.state['errors'] += 1
        
        self.save_state()
        return result
    
    def check_file(self, file_path: str) -> bool:
        # Validation logic here
        return True

if __name__ == '__main__':
    hook_input = json.loads(sys.stdin.read())
    validator = StatefulValidator()
    
    if validator.validate(hook_input):
        sys.exit(0)
    else:
        sys.exit(1)
```

### Pattern 4: External System Notifications

```bash
#!/bin/bash
"""
Notification hook that sends to Slack/email.
Non-blocking pattern.
"""

HOOK_INPUT=$(cat)

FILE_PATH=$(echo "$HOOK_INPUT" | jq -r '.file_path')
TOOL=$(echo "$HOOK_INPUT" | jq -r '.tool')
TIMESTAMP=$(echo "$HOOK_INPUT" | jq -r '.timestamp')

# Format message
MESSAGE="Claude Code Action: $TOOL on $FILE_PATH at $TIMESTAMP"

# Send to Slack (background)
if [ -n "$SLACK_WEBHOOK" ]; then
    curl -X POST -H 'Content-type: application/json' \
        --data "{\"text\":\"$MESSAGE\"}" \
        "$SLACK_WEBHOOK" &
fi

# Send email (background)
if command -v mail &>/dev/null; then
    echo "$MESSAGE" | mail -s "Claude Code Event" admin@example.com &
fi

# Always exit 0 - notifications don't block
exit 0
```

---

## Hook Configuration Best Practices

### 1. Timeout Configuration

```yaml
hooks:
  pre_tool_use:
    - tools: [Write]
      command: "bash validate.sh"
      timeout_seconds: 5  # Don't hang the agent
```

**Rule of thumb:** 
- Syntax checks: 2-5 seconds
- Type checks: 5-10 seconds  
- Complex analysis: 10-30 seconds
- Integration tests: 30-60 seconds

### 2. Error Handling

```python
#!/usr/bin/env python3
import sys
import json

try:
    hook_input = json.loads(sys.stdin.read())
    result = validate(hook_input)
    
    if not result:
        print("Validation failed", file=sys.stderr)
        sys.exit(1)
    
    sys.exit(0)

except json.JSONDecodeError as e:
    print(f"Invalid JSON input: {e}", file=sys.stderr)
    sys.exit(1)
except Exception as e:
    print(f"Unexpected error: {e}", file=sys.stderr)
    # Don't block agent on hook errors
    sys.exit(0)
```

### 3. Performance Optimization

```bash
#!/bin/bash

# Only run expensive checks on important files
FILE_PATH=$(echo "$HOOK_INPUT" | jq -r '.file_path')

if [[ "$FILE_PATH" =~ (api|core|auth) ]]; then
    python comprehensive_validator.py "$FILE_PATH"
else
    python lightweight_validator.py "$FILE_PATH"
fi
```

### 4. Observability

```bash
#!/bin/bash

HOOK_LOG=".claude/hooks.log"

{
    echo "=== Hook Execution ==="
    echo "Time: $(date -u +%Y-%m-%dT%H:%M:%SZ)"
    echo "Input:"
    cat | tee /tmp/hook_input.json
    echo ""
    echo "Result: $?"
} >> "$HOOK_LOG" 2>&1
```

---

## Common Hook Mistakes & Solutions

### Mistake 1: Blocking on Network I/O

```python
# ❌ WRONG - Hook hangs if API is slow
response = requests.get('https://api.example.com/validate')

# ✅ CORRECT - Use timeout or background task
try:
    response = requests.get('https://api.example.com/validate', timeout=2)
except requests.Timeout:
    print("API timeout, skipping validation", file=sys.stderr)
    sys.exit(0)  # Don't block agent
```

### Mistake 2: Not Handling Large Files

```bash
# ❌ WRONG - File too large to load into memory
cat "$FILE_PATH" | jq '.'

# ✅ CORRECT - Stream or check size first
if [ $(stat -f%z "$FILE_PATH") -gt 10000000 ]; then
    echo "File too large, skipping detailed validation" >&2
    exit 0
fi

jq '.' "$FILE_PATH"
```

### Mistake 3: JSON Parsing Errors

```python
# ❌ WRONG - Assumes valid JSON
data = json.loads(sys.stdin.read())

# ✅ CORRECT - Handle parse errors
try:
    data = json.loads(sys.stdin.read())
except json.JSONDecodeError as e:
    print(f"Invalid JSON input: {e}", file=sys.stderr)
    sys.exit(1)
```

---

## Conclusion

Claude Code hooks provide a sophisticated mechanism for embedding validation, observability, and control directly into the agent execution workflow. By understanding:

1. **All 8 hook types** and their lifecycle positions
2. **JSON input structures** and how to parse them with jq
3. **Tool-specific inputs** for Read, Write, Edit, and Exec
4. **Debugging techniques** for hook development
5. **Real-world patterns** from validation to autonomous loops

You can build robust, production-grade agent systems that catch errors automatically, log everything, and gracefully handle edge cases.

**The key insight:** Hooks aren't just safety guardrails—they're the foundation of trustworthy agentic systems. Invest in them early.

---

**Article Created:** 2026-03-09  
**Format:** Comprehensive markdown with JSON examples, bash/Python implementations, jq patterns, and debugging guides  
**Status:** Ready for Obsidian vault (manual copy required)  
**Tags:** Claude Code, Hooks, Validation, Debugging, jq, Architecture


---

## Source

**Full Course:** [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action)  
**Note:** Signup required to access full course content and video lessons

**Course Topics:**
- Introduction to Claude Code and AI assistants
- Project setup and context management
- Custom commands and MCP servers
- GitHub integration
- Hooks and SDK
- Quiz and summary

**Related Chapters:**
- Useful Hooks! (TypeScript type checking, query duplication prevention)
- Another Useful Hook! (Hook types and input structures)
- The Claude Code SDK (Programmatic integration)
