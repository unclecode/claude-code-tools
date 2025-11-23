# Save Session Checkpoint

Save current chat session state and prepare instructions for next session.

## Arguments

$ARGUMENTS

Optional query to specify what should happen next.

## Purpose

Called at the end of a chat session (or before hitting token limits) to:
1. Create ultra-compact summary of current session
2. Save "what to do next" instructions for new session
3. Link to full chat transcript (with warnings about size)

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

### Step 3: Generate "What To Do Next"

**If user provided query:**
- Use their query as primary guidance
- Enhance with context from current state

**If NO query provided:**
- Analyze current state:
  - Read `TODO.md` - what's pending
  - Read last 2-3 `CHANGELOG.md` entries - what was just done
  - Read `STATUS.md` - current focus
- Generate clear next actions

### Step 4: Create History Summary (Ultra-Compact)

**Location**: `.context/project/{active}/history/{uuid}-{YYYY-MM-DD}.md`

**Content** (token-efficient, ~200-400 tokens for main summary + warnings):
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

**Important**: Keep main summary VERY compact. Transcript warnings go at end.

### Step 5: Create/Update NEXT.md (Minimal)

**Location**: `.context/project/{active}/NEXT.md`

**Content** (keep minimal, link to history for details):
```markdown
# Next Chat Session

## What To Do Next

{AI-generated guidance OR user's query}

Examples:
- User will fill knowledge base at webhook/maya_knowledge.md
- Begin T010: Node.js project setup
- Continue implementation of LLMService
- Review PRD and provide feedback

## Previous Session Context

**Session**: {uuid} ({YYYY-MM-DD})

See: [history/{uuid}-{YYYY-MM-DD}.md](history/{uuid}-{YYYY-MM-DD}.md)

## Current State

**Active Phase**: {from STATUS.md}
**Pending Tasks**: {high-priority tasks from TODO.md}
**Blockers**: {if any}
**Last Updated**: {files recently modified}
```

**Important**: Keep NEXT.md minimal. All detailed context (including full transcript warnings) goes in the history file.

### Step 6: Output Confirmation

Tell user:
```
✅ Checkpoint saved!

Files created/updated:
- .context/project/{active}/NEXT.md (instructions for next session)
- .context/project/{active}/history/{uuid}-{date}.md (session summary)

When starting next session:
1. Run `/pp-resume` to load context + next steps
2. Continue from where we left off

Session ID: {uuid}
```

## Notes

- Always create history/ directory if it doesn't exist
- NEXT.md is overwritten each time (always shows latest)
- History files are never overwritten (one per session)
- Keep summaries ultra-compact to save tokens
- Full transcript path is for rare cases when specific detail needed

## Error Handling

If unable to find session UUID:
- Inform user
- Still create NEXT.md with guidance
- Skip history file creation (not critical)

If active subproject unclear:
- Ask user which subproject
- Or use root .context/project/ as fallback
