# Clean Project Files

Clean up temporary files and archive useful markdown.

## Arguments

$ARGUMENTS

## Purpose

After tasks/updates, clean up the project:
1. **Delete** truly temporary files (test outputs, scratch, debug logs)
2. **Archive** potentially useful markdown (drafts, notes, old versions)
3. Keep project directory clean while preserving history

## Execution Instructions

### Step 1: Scan for Files to Clean

Look in project root and common locations for:

**Temporary files (to DELETE):**
- `*_test.md`, `test_*.md`
- `scratch*.md`, `temp*.md`, `tmp*.md`
- `debug*.md`, `*_debug.md`
- `*.log` (unless in logs/ folder)
- `output*.md`, `*_output.md`
- Files with `DELETE` or `TEMP` in name

**Potentially useful markdown (to ARCHIVE):**
- `draft_*.md`, `*_draft.md`
- `notes_*.md`, `*_notes.md`
- `research_*.md`
- `old_*.md`, `*_old.md`, `*_backup.md`
- `v1_*.md`, `*_v1.md` (old versions)
- Any `.md` file in root that's not part of standard structure
- User-created markdown during session

### Step 2: Parse Optional Query

If query provided, focus on:
- Specific pattern: `/pp-clean test files`
- Specific folder: `/pp-clean webhook/`
- Specific action: `/pp-clean archive only` or `/pp-clean delete only`

### Step 3: Show Preview

```
## Cleanup Preview

### To DELETE (temporary files)
- scratch_booking.md
- test_output.md
- debug_log.md

### To ARCHIVE (move to .context/project/archive/)
- draft_system_prompt.md
- research_notes.md
- old_booking_flow.md

### No Action
- (files that look fine)

---
Proceed with cleanup? (yes/no/edit)
```

If user says "edit", allow them to:
- Move items between delete/archive
- Skip specific files
- Add files to clean

### Step 4: Execute Cleanup

**Delete temporary files:**
```bash
rm {temp_files}
```

**Archive useful markdown:**
```bash
# Create dated archive subfolder if multiple files
mkdir -p .context/project/archive/cleanup_{date}
mv {markdown_files} .context/project/archive/cleanup_{date}/
```

Or if single file:
```bash
mv {file} .context/project/archive/{file}
```

### Step 5: Update CHANGELOG

Add entry to appropriate CHANGELOG:

```markdown
## {DATE} | Cleanup

**Deleted**:
- scratch_booking.md
- test_output.md

**Archived**:
- draft_system_prompt.md → archive/cleanup_{date}/
- research_notes.md → archive/cleanup_{date}/

**Context**:
- Post-task cleanup
- {query context if provided}
```

### Step 6: Confirm Completion

```
## Cleanup Complete

**Deleted**: 3 temporary files
**Archived**: 2 markdown files → archive/cleanup_{date}/

Project directory is clean.
```

## Auto-Trigger Integration

This cleanup logic should also run:
- After `/pp-update` completes
- After major task completion
- Before commits (optional)

When auto-triggered, still show preview and get confirmation.

## Query Examples

```bash
# General cleanup
/pp-clean

# Focus on specific pattern
/pp-clean test files

# Focus on specific folder
/pp-clean webhook/

# Archive only (no deletions)
/pp-clean archive only

# Delete only (no archiving)
/pp-clean delete only

# With context
/pp-clean after completing booking flow
```

## Safety Rules

1. **Always show preview first**
2. **Always get confirmation**
3. **Never delete .md files that might be useful** - archive them
4. **Never touch:**
   - `.context/project/` structure files
   - Files in `prds/`, `docs/` folders
   - `.env`, config files
   - Source code files
5. **Log everything** in CHANGELOG

## File Patterns Reference

### Safe to DELETE
```
*_test.md, test_*.md
scratch*.md, temp*.md, tmp*.md
debug*.md, *_debug.md
output*.md, *_output.md
*.log (loose logs)
*DELETE*, *TEMP*
```

### Should ARCHIVE
```
draft_*.md, *_draft.md
notes_*.md, *_notes.md
research_*.md
old_*.md, *_old.md, *_backup.md
v[0-9]_*.md, *_v[0-9].md
Loose .md files not in structure
```

### Never Touch
```
.context/project/**/*.md (structure files)
prds/*.md, docs/*.md
README.md, CLAUDE.md
.env, *.json, *.py, *.js, etc.
```
