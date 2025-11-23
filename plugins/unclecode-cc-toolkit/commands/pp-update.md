# Update Project Documentation

Update project files after making changes.

## Arguments

$ARGUMENTS

## Purpose

Run after making code changes to keep project documentation in sync. Updates:
- TODO.md (mark tasks done/in-progress)
- CHANGELOG.md (log changes)
- STATUS.md (if state changed)
- CODEBASE.md (if files added/modified)
- LESSONS.md (if debugging insights gained)

## Execution Instructions

### Step 1: Identify Active Subproject

Read `.context/project/INDEX.md` to get active subproject.

### Step 2: Parse Query

Query should describe what was done:
- "completed T010, T011" → Mark tasks done
- "added webhook/llm_service.py" → Update CODEBASE
- "fixed the routing bug" → Update CHANGELOG, possibly LESSONS
- "changed booking flow approach" → Update CHANGELOG with context

If no query, ask: "What changes did you make? (tasks completed, files added, bugs fixed)"

### Step 3: Determine What to Update

Based on query, identify which files need updates:

| Query Pattern | Files to Update |
|---------------|-----------------|
| "completed T###" | TODO.md, CHANGELOG.md |
| "added/created file" | CODEBASE.md, CHANGELOG.md |
| "fixed bug" | CHANGELOG.md, possibly LESSONS.md |
| "changed approach" | CHANGELOG.md, possibly STATUS.md |
| "working on T###" | TODO.md (mark in-progress) |

### Step 4: Collect Additional Info

Ask for any missing info:
- For CHANGELOG: "Brief description of what changed?"
- For CHANGELOG: "Any usage examples to include?"
- For LESSONS: "What was the root cause and solution?"

### Step 5: Make Updates

Update files in this order:
1. TODO.md - Mark tasks
2. CODEBASE.md - Add files
3. LESSONS.md - Add learnings
4. CHANGELOG.md - Log everything
5. STATUS.md - Update state if needed

### Step 6: Confirm Updates

Show summary:
```
## Updated Project Docs

**TODO.md**: Marked T010, T011 as completed
**CODEBASE.md**: Added webhook/llm_service.py
**CHANGELOG.md**: Added entry for LLM service implementation

Ready to commit? Run: git add . && git commit
```

### Step 7: Trigger Cleanup Check

After confirming updates, automatically check for cleanup:

1. Scan for temporary/draft files (see `/pp-clean` for patterns)
2. If files found, show preview:
```
## Cleanup Check

Found files to clean:
- DELETE: scratch_test.md, debug_output.md
- ARCHIVE: draft_notes.md

Run cleanup? (yes/no/skip)
```

3. If user confirms, execute cleanup per `/pp-clean` logic
4. If user skips, remind: "Run `/pp-clean` later to tidy up"

This keeps project clean after each update cycle.

## Query Examples

```bash
# After completing tasks
/pp-update completed T010 and T011, added llm_service.py

# After fixing a bug
/pp-update fixed the referral data extraction bug

# After changing approach
/pp-update changed booking flow to conversational style

# General update (will ask questions)
/pp-update
```

## Update Both Levels

- If change affects only subproject → update subproject files
- If change affects multiple subprojects → update root files too
- CHANGELOG goes in subproject unless it's cross-project
