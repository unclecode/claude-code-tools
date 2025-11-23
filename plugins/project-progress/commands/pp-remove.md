# Remove Subproject

Remove a subproject from the project structure.

## Arguments

$ARGUMENTS

## Purpose

Safely remove a subproject. This will:
1. Archive the subproject folder (not delete)
2. Update root files to remove references
3. Log the removal

## Execution Instructions

### Step 1: Parse Query

Extract subproject name from query.

If empty, list available subprojects and ask which to remove.

### Step 2: Verify Subproject Exists

```bash
ls .context/project/{name}/INDEX.md
```

If not found, show error: "Subproject '{name}' not found."

### Step 3: Check if Active

Read `.context/project/STATUS.md` to check if this is the active subproject.

If active, ask: "This is the active subproject. Which subproject should become active instead?"

### Step 4: Confirm Removal

Show warning:
```
## Removing Subproject: {name}/

This will:
- Archive {name}/ to archive/{name}_{date}/
- Remove from INDEX.md subproject table
- Remove from STATUS.md summary
- Remove from CODEBASE.md refs
- Add removal entry to CHANGELOG.md

Files will NOT be deleted, just archived.

Proceed? (yes/no)
```

Wait for confirmation.

### Step 5: Archive Subproject

```bash
mv .context/project/{name} .context/project/archive/{name}_{date}
```

### Step 6: Update Root Files

**INDEX.md**: Remove from subproject table

**STATUS.md**:
- Remove from subproject summary table
- If was active, update active subproject

**CODEBASE.md**: Remove from subproject refs

**CHANGELOG.md**: Add entry
```markdown
## {DATE} | Removed Subproject: {name}

**Changes**:
- Archived {name}/ to archive/{name}_{date}/
- Removed from project structure

**Context**:
- {reason if provided}
- Files preserved in archive
```

### Step 7: Confirm Removal

```
## Removed Subproject: {name}/

**Archived to**: archive/{name}_{date}/
**Active subproject**: {new_active}

Updated:
- Root INDEX.md
- Root STATUS.md
- Root CODEBASE.md
- Root CHANGELOG.md

To restore, move from archive back to project root.
```

## Query Examples

```bash
# With name
/pp-remove old-api

# With reason
/pp-remove old-api - replaced by new api design

# Interactive (will list and ask)
/pp-remove
```

## Safety Notes

- Never deletes files, only archives
- Always asks for confirmation
- Logs removal in CHANGELOG
- Can be restored by moving from archive
