# Project Status Summary

Show compact status of the entire project.

## Arguments

$ARGUMENTS

## Purpose

Quick overview of project state without loading full context. Shows:
- Active subproject and its focus
- All subprojects status
- In-progress tasks
- Recent changes
- Any blockers

## Execution Instructions

### Step 1: Read Status Files

Read these files:
- `.context/project/STATUS.md`
- `.context/project/INDEX.md`
- `.context/project/{active}/STATUS.md`
- `.context/project/{active}/TODO.md`

### Step 2: Parse Optional Query

If query provided, focus on:
- Specific subproject: `/pp-status maya`
- Specific aspect: `/pp-status tasks` or `/pp-status blockers`

### Step 3: Generate Compact Summary

Output format:

```
## Project Status: {PROJECT_NAME}

### Active: {subproject}/
**Phase**: {current phase}
**Focus**: {one-line current focus}

### In Progress
{tasks marked [>], one line each with T### ID}

### Pending (Next Up)
{next 3-5 pending tasks}

### Subprojects
| Name | Status | Phase |
|------|--------|-------|
{table from root STATUS.md}

### Blockers
{any blocked items from STATUS.md, or "None"}

### Recent Activity
{last 2-3 changelog entries, date + one-line summary}

---
Last updated: {date from STATUS.md}
```

### Step 4: Subproject-Specific Status

If query specifies a subproject:

```
## Subproject Status: {name}/

**Phase**: {phase}
**Status**: {status}

### What's Working
{bullet list}

### What's Blocked
{bullet list or "Nothing blocked"}

### Current Focus
{numbered list}

### Tasks
**In Progress**: {count}
**Pending**: {count}
**Completed**: {count}

### Next Actions
{from STATUS.md}
```

## Query Examples

```bash
# Full project status
/pp-status

# Specific subproject
/pp-status maya

# Focus on tasks only
/pp-status tasks

# Focus on blockers
/pp-status blockers
```

## Output Notes

- Keep it COMPACT - fits in one screen
- Use tables for multiple items
- One line per item max
- Show counts, not full lists
- Highlight blockers prominently
- This is READ-ONLY (no modifications)
