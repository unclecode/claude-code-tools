# Edit Project Progress Files

Quick, natural language edits to project planning files.

## Arguments

$ARGUMENTS

Query describing what to edit.

## Purpose

Make direct edits to PP files using natural language without manually finding and editing files.

## Execution Instructions

### Step 1: Parse Edit Query

Extract from query:
1. **Target file** (INDEX.md, WORKFLOW.md, PRINCIPLES.md, STATUS.md, TODO.md, etc.)
2. **Action** (add, update, remove, change)
3. **Content** (what to add/change/remove)
4. **Location** (root or subproject)

**Query patterns:**
- "add X to Y" → append content
- "update X in Y to Z" → replace content
- "remove X from Y" → delete content
- "change X to Y" → replace content

### Step 2: Identify File Location

**Root files** (in `.context/project/`):
- INDEX.md
- WORKFLOW.md
- PRINCIPLES.md
- LESSONS.md

**Subproject files** (in `.context/project/{subproject}/`):
- STATUS.md
- TODO.md
- CHANGELOG.md
- CODEBASE.md
- LESSONS.md
- PRINCIPLES.md (if exists)

If query doesn't specify subproject and file is subproject-level:
- Read INDEX.md to get active subproject
- Apply edit to active subproject's file

### Step 3: Validate Target

Check if target file exists:
```bash
ls .context/project/{target_file}
# or
ls .context/project/{subproject}/{target_file}
```

If file doesn't exist, inform user and ask if they want to create it.

### Step 4: Apply Edit

**For "add" operations:**
- Append content to appropriate section
- Maintain markdown formatting
- Add to end of file or specific section if mentioned

**For "update/change" operations:**
- Find existing content
- Replace with new content
- Preserve formatting

**For "remove" operations:**
- Find and delete content
- Clean up extra blank lines

### Step 5: Confirm Changes

Show diff or summary:
```
## Edit Applied

**File**: .context/project/PRINCIPLES.md
**Action**: Added new principle

**Content added**:
> Always use TypeScript strict mode

**Location**: End of file

File updated successfully.
```

## Query Examples

```bash
# Add to root files
/pp-edit add "Daily standup at 9am" to WORKFLOW.md
/pp-edit update PRINCIPLES.md to include "Use functional programming patterns"

# Update INDEX.md
/pp-edit change active subproject to backend
/pp-edit add high-level TODO "Launch MVP by March 15"

# Edit subproject files (uses active subproject)
/pp-edit add task "T050: Implement password reset" to TODO.md
/pp-edit update STATUS.md working section with "API endpoints complete"
/pp-edit remove "T010: Initial setup" from TODO.md

# Edit specific subproject
/pp-edit add "Use Redis for caching" to backend PRINCIPLES.md
/pp-edit update frontend STATUS.md blocked section with "Waiting for API completion"
```

## Smart Parsing

If query is ambiguous, ask clarifying questions:
- "Which file? (INDEX.md, WORKFLOW.md, PRINCIPLES.md, STATUS.md, TODO.md)"
- "Which subproject? (main, backend, frontend)"
- "Where in the file? (beginning, end, specific section)"

## Notes

- Always preserve existing content unless explicitly asked to replace
- Maintain markdown formatting and structure
- If adding to structured files (TODO.md, CHANGELOG.md), follow existing format
- Show what changed so user can verify
- Suggest commit after edit: "Ready to commit? Run: /pp-update"
