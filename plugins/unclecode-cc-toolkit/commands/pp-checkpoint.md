# Save Session Checkpoint

Save current chat session state and prepare instructions for next session.

## Arguments

$ARGUMENTS

Optional query to add extra guidance for the next session. This query will be incorporated into the "What To Do Next" section of NEXT.md.

Examples:
- `/pp-checkpoint focus on fixing the auth bug first`
- `/pp-checkpoint user will provide new API docs tomorrow`
- `/pp-checkpoint skip T260, move directly to T262`

## Purpose

Called at the end of a chat session (or before hitting token limits) to:
1. Create compact summary of current session (history file)
2. Save comprehensive "what to do next" instructions (NEXT.md)
3. Optionally create AI-compressed dump of chat history
4. Link to full chat transcript

## Execution Instructions

### Step 1: Identify Current Session

```bash
# Get current working directory
pwd

# Convert to Claude projects path
# Example: /Users/unclecode/devs/project
# Becomes: ~/.claude/projects/-Users-unclecode-devs-project/

# Find most recent .jsonl file (exclude agent-* files)
ls -lt ~/.claude/projects/-Users-unclecode-devs-{converted-path}/*.jsonl | grep -v "agent-" | head -1

# Extract UUID from filename
# Example: 530170ff-52f5-4ad6-a333-22a09bd64773.jsonl
# UUID: 530170ff-52f5-4ad6-a333-22a09bd64773
```

### Step 2: Determine Active Subproject

Read `.context/project/INDEX.md` to identify active subproject.

### Step 3: Ask User About Chat History Dump

**ALWAYS ask the user:**

```
Before saving checkpoint, do you want me to create a compressed dump of this chat session?

This will use AI to compress the full conversation into a readable summary (~5-10% of original size) that captures all important details, decisions, and code changes.

Options:
1. **Yes** - Create compressed dump (takes ~30-60 seconds)
2. **No** - Skip dump, just save checkpoint files

Note: The raw .jsonl transcript is always available for grep/search regardless.
```

**If user says Yes:**
- Run the `/compress-chat` command on the current session
- Save compressed output to `.context/project/{active}/history/{uuid}-{YYYY-MM-DD}-dump.md`

**If user says No:**
- Skip dump creation, proceed to next step

### Step 4: Process User Query (if provided)

**If user provided query in $ARGUMENTS:**
- Parse the query for guidance/instructions
- This becomes the PRIMARY content for "What To Do Next" section
- Enhance with context from current state (TODO.md, STATUS.md)

**If NO query provided:**
- Analyze current state:
  - Read `TODO.md` - what's pending
  - Read last 2-3 `CHANGELOG.md` entries - what was just done
  - Read `STATUS.md` - current focus
- Generate clear next actions

### Step 5: Create History Summary (Compact)

**Location**: `.context/project/{active}/history/{uuid}-{YYYY-MM-DD}.md`

**Content** (compact, ~200-400 tokens):
```markdown
# Chat Session {uuid}
**Date**: {YYYY-MM-DD HH:MM}
**Tokens Used**: ~{approximate} / 200k

## Key Achievements
- [T###] Task completed
- Major milestone reached
- Decision made

## Technical Decisions
- Technology choice with rationale
- Architecture decision
- Library/framework selected

## Files Created/Modified
- path/to/file.ext
- another/file.js

## What's Next
{Brief 1-2 line summary}

---

## Full Chat Transcript

**⚠️ IMPORTANT**: Full conversation available at:
`{full-path-to-jsonl-file}`

**WARNING**: This file is LARGE (~2-3MB, ~140k+ tokens). NEVER load it entirely.

**How to use it**:
- Use `grep` to search for specific topics
- Use `tail -n 100` for recent messages
- Use `head -n 100` for session start
- Use `sed -n 'START,ENDp'` for specific line ranges
- Parse JSON with `jq` for structured queries

**Example commands**:
```bash
# Search for specific topic
grep -i "your topic" {path-to-file}

# Get last 50 messages
tail -n 50 {path-to-file}

# Count total lines
wc -l {path-to-file}
```
```

### Step 6: Create/Update NEXT.md (Comprehensive)

**Location**: `.context/project/{active}/NEXT.md`

**Purpose**: NEXT.md is the PRIMARY file a new AI session reads. It must contain enough actionable information for the new session to work effectively WITHOUT needing to read the history file.

**Content** (comprehensive, actionable):
```markdown
# Next Chat Session

## Previous Session Context

**Session**: {uuid} ({YYYY-MM-DD})
**History**: [history/{uuid}-{YYYY-MM-DD}.md](history/{uuid}-{YYYY-MM-DD}.md)
{If dump exists: **Dump**: [history/{uuid}-{YYYY-MM-DD}-dump.md](history/{uuid}-{YYYY-MM-DD}-dump.md)}

**Active Phase**: {from STATUS.md}
**Pending Tasks**: {from TODO.md}
**Last Modified**: {files changed this session}

---

## What To Do Next

{If user provided query: Use their query as PRIMARY guidance, enhanced with context}
{If no query: AI-generated guidance based on TODO.md, STATUS.md, CHANGELOG.md}

### Immediate Tasks
1. Task with context
2. Another task

### Quick Start Commands
```bash
{Common commands the new session will need}
```

---

## Key Reference Data

{Include any important data/tables the new session needs - benchmarks, configs, etc.}

---

## Key Files

| File | Purpose |
|------|---------|
| path/to/file | What it does |

---

## Session Achievements (What Was Done)

- What was accomplished this session
- Tasks completed
- Decisions made

---

## Notes & Gotchas

- Important technical note
- Gotcha to avoid wasting time
- Environment variable or config reminder
```

**Important**: NEXT.md should be COMPREHENSIVE. The new AI session should be able to start working immediately after reading it.

### Step 7: Output Confirmation

Tell user:
```
✅ Checkpoint saved!

Files created/updated:
- .context/project/{active}/NEXT.md (instructions for next session)
- .context/project/{active}/history/{uuid}-{date}.md (session summary)
{If dump created: - .context/project/{active}/history/{uuid}-{date}-dump.md (compressed chat)}

When starting next session:
1. Run `/pp-resume` to load context + next steps
2. Continue from where we left off

Session ID: {uuid}
```

## Notes

- Always create history/ directory if it doesn't exist
- NEXT.md is overwritten each time (always shows latest)
- History files are never overwritten (one per session)
- NEXT.md should be COMPREHENSIVE - new AI session's primary reference
- History file is COMPACT - backup/deep-dive reference only
- User query from $ARGUMENTS takes priority for "What To Do Next"
- Always ask about dump before proceeding

## Error Handling

If unable to find session UUID:
- Inform user
- Still create NEXT.md with guidance
- Skip history file creation (not critical)

If active subproject unclear:
- Ask user which subproject
- Or use root .context/project/ as fallback
