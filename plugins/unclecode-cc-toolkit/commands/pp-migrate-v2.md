# Migrate PP v1.0 to v1.1+ Structure

**TEMPORARY COMMAND**: Upgrades old Project Planning structure (v1.0.0) to new structure (v1.1.0+).

## Arguments

None

## Purpose

Migrate existing PP projects from old structure (with root STATUS.md, CODEBASE.md, CHANGELOG.md) to new simplified structure (with combined INDEX.md).

**Use this if you have projects using the old PP structure that need upgrading.**

## What Changes

### Old Structure (v1.0.0):
```
.context/project/
├── INDEX.md (old format)
├── STATUS.md ← will be archived
├── CODEBASE.md ← will be archived
├── CHANGELOG.md ← will be archived
├── WORKFLOW.md (old)
└── subproject/
```

### New Structure (v1.1.0+):
```
.context/project/
├── INDEX.md ← NEW (combines INDEX + STATUS + high-level TODOs)
├── WORKFLOW.md ← UPDATED
├── PRINCIPLES.md
├── LESSONS.md
└── subproject/ ← UNCHANGED
```

## Execution Instructions

### Step 1: Detect Old Structure

Check if this is an old PP structure:
```bash
ls .context/project/STATUS.md
```

**If STATUS.md doesn't exist:**
- Inform user: "Project already using new structure or not initialized. Nothing to migrate."
- Exit

**If STATUS.md exists:**
- Proceed to Step 2

### Step 2: Read Old Files

Read and extract data from old files:

**From STATUS.md:**
- Active subproject (section: "## Active Subproject")
- Subproject summary table (section: "## Subproject Summary")
- Overall progress (if any)
- Cross-project dependencies (if any)
- Recent decisions (if any)

**From INDEX.md (old):**
- Project name
- Project description
- Created date
- Subprojects table
- Environment info

**From CODEBASE.md:**
- Check if it had any important root-level file listings
- (Usually empty or minimal in old structure)

**From CHANGELOG.md:**
- Check if it had any cross-project entries
- (Usually empty or minimal in old structure)

### Step 3: Create New INDEX.md

Generate new INDEX.md using the template from `pp-init-assets/templates/root/INDEX.md`:

**Populate with extracted data:**
- `{{PROJECT_NAME}}` - from old INDEX.md
- `{{PROJECT_DESCRIPTION}}` - from old INDEX.md
- `{{DATE}}` - original created date from old INDEX.md
- `{{ACTIVE_SUBPROJECT}}` - from old STATUS.md
- `{{SUBPROJECT_TABLE}}` - from old INDEX.md
- `{{SUBPROJECT_STATUS_TABLE}}` - from old STATUS.md
- `{{ENVIRONMENT}}` - from old INDEX.md
- `{{TODO_ITEM_1}}`, `{{TODO_ITEM_2}}`, `{{TODO_ITEM_3}}` - set to "TBD" or extract from context

**Save to:** `.context/project/INDEX.md` (overwrite old INDEX.md)

### Step 4: Update WORKFLOW.md

Replace old WORKFLOW.md with new version:
```bash
cp ~/.claude/commands/pp-init-assets/templates/root/WORKFLOW.md .context/project/WORKFLOW.md
```

### Step 5: Archive Old Files

Move old root-level files to archive:
```bash
mkdir -p .context/project/archive/v1.0-migration-$(date +%Y%m%d)
mv .context/project/STATUS.md .context/project/archive/v1.0-migration-*/
mv .context/project/CODEBASE.md .context/project/archive/v1.0-migration-*/
mv .context/project/CHANGELOG.md .context/project/archive/v1.0-migration-*/
```

**Create archive README:**
Create `.context/project/archive/v1.0-migration-{date}/README.md`:
```markdown
# v1.0 Structure Archive

These files were archived during migration from PP v1.0.0 to v1.1.0+ on {date}.

## Archived Files

- STATUS.md - Replaced by combined INDEX.md
- CODEBASE.md - Moved to subproject level only
- CHANGELOG.md - Moved to subproject level only

## What Changed

v1.1.0+ simplified the root structure:
- Root now has only: INDEX.md, WORKFLOW.md, PRINCIPLES.md, LESSONS.md
- All detailed tracking (STATUS, TODO, CHANGELOG, CODEBASE) lives in subprojects
- INDEX.md now combines project overview + active subproject + status summary

## Data Preservation

All data from these files was extracted and merged into the new INDEX.md.
Nothing was lost during migration.
```

### Step 6: Verify Subprojects Unchanged

Confirm that subproject directories remain intact:
```bash
ls .context/project/{subproject}/
```

**Subprojects should still have:**
- STATUS.md
- TODO.md
- CHANGELOG.md
- CODEBASE.md
- etc.

**No changes needed** - subproject structure is the same in both versions.

### Step 7: Show Summary

Display migration summary:
```
## PP Structure Migration Complete

**Old Structure**: v1.0.0
**New Structure**: v1.1.0+
**Date**: {YYYY-MM-DD HH:MM}

### Changes Made:

✅ Created new INDEX.md (combines INDEX + STATUS + high-level TODOs)
✅ Updated WORKFLOW.md to new version
✅ Archived old files:
   - STATUS.md → archive/v1.0-migration-{date}/
   - CODEBASE.md → archive/v1.0-migration-{date}/
   - CHANGELOG.md → archive/v1.0-migration-{date}/

### What Stayed the Same:

✓ All subproject directories unchanged
✓ All subproject files (STATUS, TODO, CHANGELOG, CODEBASE) intact
✓ Project data preserved in new INDEX.md
✓ PRINCIPLES.md, LESSONS.md unchanged

### New Root Structure:

.context/project/
├── INDEX.md ← NEW (combined)
├── WORKFLOW.md ← UPDATED
├── PRINCIPLES.md
├── LESSONS.md
└── {subprojects}/ ← UNCHANGED

### Next Steps:

1. Review new INDEX.md to verify data accuracy
2. Check active subproject is correct
3. Continue using PP commands as normal
4. Old files safely archived if you need to reference them

---

**Note**: This migration is one-way. The old files are archived, not deleted.
You can always reference them in the archive/ folder.
```

## Error Handling

**If .context/project/ doesn't exist:**
```
No PP structure found. Run /pp-init first to create project structure.
```

**If STATUS.md doesn't exist:**
```
Project already using new structure (v1.1.0+) or not fully initialized.

If you have subprojects but no root STATUS.md, you're likely already upgraded.
Nothing to migrate.
```

**If migration fails mid-way:**
```
Migration encountered an error. Your files are safe:
- Original files still in place
- Partial changes may exist

Recommendation:
1. Check .context/project/ directory
2. Manually review what was changed
3. Restore from backup if needed
4. Report issue to plugin maintainer
```

## Safety Features

- **Non-destructive**: Old files archived, not deleted
- **Backup created**: Archive folder with timestamp
- **Verification**: Checks structure before starting
- **Detailed logging**: Shows exactly what changed

## Usage Example

```bash
# In a project with old PP structure
cd /path/to/project

# Run migration
/pp-migrate-v2

# Result: Upgraded to new structure
# Old files safely archived
```

## Notes

- **Temporary command**: Will be removed in future versions once most projects are migrated
- **One-time operation**: Only need to run once per project
- **Idempotent**: Safe to run multiple times (detects if already migrated)
- **Subprojects untouched**: Only root structure changes
- **Data preserved**: Nothing is lost, everything merged into new INDEX.md

## Version History

- Created for v1.1.0 → v1.1.0+ migration
- To be deprecated in v2.0.0
