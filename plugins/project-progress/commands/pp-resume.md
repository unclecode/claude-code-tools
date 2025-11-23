# Resume Project Session

Start a new chat session with full project context loaded.

## Arguments

$ARGUMENTS

## Purpose

This is the FIRST command to run when starting a new chat session. It:
1. Loads all project context into memory
2. Understands current state
3. Prepares to continue work
4. Optionally focuses on specific direction from query

## Execution Instructions

### Step 1: Verify Project Exists

```bash
ls .context/project/INDEX.md
```

If not found, inform user: "No project structure found. Run `/pp-init` first."

### Step 2: Read Root Context

Read these files in order:
1. `.context/project/INDEX.md` - Project overview, subprojects
2. `.context/project/WORKFLOW.md` - How this system works
3. `.context/project/STATUS.md` - Current state, active subproject
4. `.context/project/PRINCIPLES.md` - Methodology and rules
5. `.context/project/CODEBASE.md` - File map
6. `.context/project/LESSONS.md` - Past learnings

### Step 3: Read Active Subproject Context

From STATUS.md, identify active subproject, then read:
1. `.context/project/{active}/INDEX.md`
2. `.context/project/{active}/STATUS.md`
3. `.context/project/{active}/TODO.md`
4. `.context/project/{active}/PRINCIPLES.md`
5. `.context/project/{active}/CODEBASE.md`
6. `.context/project/{active}/LESSONS.md`

### Step 4: Read Recent Changes

Read last 3-5 entries from:
- `.context/project/CHANGELOG.md`
- `.context/project/{active}/CHANGELOG.md`

### Step 5: Process Optional Query

If query provided:
- Parse for direction (e.g., "continue booking flow", "fix the API bug")
- Focus context on relevant TODO items
- Identify relevant LESSONS if debugging

### Step 6: Check for Next Session Instructions (NEW)

**After loading all standard files**, check for continuation instructions:

```bash
ls .context/project/{active}/NEXT.md
```

**If NEXT.md exists:**
1. Read `.context/project/{active}/NEXT.md`
2. Extract history file path from NEXT.md
3. Read that history file for previous session context
4. Note the full chat transcript path (but DO NOT load it)

**If NEXT.md does NOT exist:**
- Skip this step, proceed to summary

### Step 7: Generate Resume Summary

Output a compact summary:

```
## Session Resumed: {PROJECT_NAME}

**Active**: {subproject}/ - {current phase}
**Status**: {brief status}

{IF NEXT.md exists:}
### Instructions from Previous Session
{content from NEXT.md "What To Do Next" section}

**Previous Session**: {date} - {brief summary from history file}

### Current Focus
{from STATUS.md current focus}

### In Progress
{tasks marked [>] from TODO.md}

### Recent Changes
{last 2-3 changelog entries, one line each}

### Key Context
{if query provided, relevant context for that direction}

---

Ready to continue. {If NEXT.md exists: "Following previous session's plan." else: "What would you like to work on?"}
```

## Query Examples

```bash
# General resume - load everything
/pp-resume

# With direction - focus on specific area
/pp-resume continue the booking flow implementation
/pp-resume fix the WhatsApp routing bug
/pp-resume review yesterday's changes
```

## Important Notes

- This command is READ-ONLY (no file modifications)
- After this, Claude has full project context
- User can immediately start working
- If query mentions something not in TODO, suggest adding it
