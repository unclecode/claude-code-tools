# Add New Subproject

Create a new subproject in the project structure.

## Arguments

$ARGUMENTS

## Purpose

Add a new subproject with full file structure. Query should contain:
- Subproject name (folder name)
- Description/purpose
- Optionally: initial scope, dependencies

## Execution Instructions

### Step 1: Parse Query

Extract from query:
- `name`: Folder name (lowercase, no spaces)
- `title`: Human-readable title
- `description`: What this subproject does
- `dependencies`: Relationships to other subprojects

If query is empty or missing info, ask:
1. "Subproject folder name (e.g., 'api', 'frontend'):"
2. "Title (e.g., 'REST API Backend'):"
3. "Brief description:"
4. "Dependencies on other subprojects? (optional)"

### Step 2: Create Directory Structure

```bash
mkdir -p .context/project/{name}/prds .context/project/{name}/docs .context/project/{name}/archive
```

### Step 3: Copy and Process Templates

Copy subproject templates from `~/.claude/commands/pp-init-assets/templates/subproject/`:

```bash
# Copy all subproject templates
cp ~/.claude/commands/pp-init-assets/templates/subproject/*.md .context/project/{name}/
```

Then process placeholders:
- `{{SUBPROJECT_NAME}}` → name
- `{{SUBPROJECT_TITLE}}` → title
- `{{SUBPROJECT_DESCRIPTION}}` → description
- `{{DATE}}` → current date
- `{{SUBPROJECT_STATUS}}` → "Not Started"
- Other placeholders → "TBD" or empty

### Step 4: Update Root Files

**INDEX.md**: Add to subproject table
```markdown
| {name}/ | Planned | {description} |
```

**STATUS.md**: Add to subproject summary table
```markdown
| {name}/ | - | Not Started |
```

**CODEBASE.md**: Add to subproject refs
```markdown
- `{name}/CODEBASE.md` - {title} files
```

**CHANGELOG.md**: Add entry
```markdown
## {DATE} | Added Subproject: {name}

**Changes**:
- Created {name}/ subproject structure

**Context**:
- {description}
```

### Step 5: Confirm Creation

```
## Created Subproject: {name}/

**Title**: {title}
**Description**: {description}

**Files created**:
- {name}/INDEX.md
- {name}/STATUS.md
- {name}/TODO.md
- {name}/CHANGELOG.md
- {name}/PRINCIPLES.md
- {name}/CODEBASE.md
- {name}/LESSONS.md

**Updated**:
- Root INDEX.md
- Root STATUS.md
- Root CODEBASE.md
- Root CHANGELOG.md

To start working on this subproject, update STATUS.md to set it as active.
```

## Query Examples

```bash
# Full description
/pp-add api - REST API backend using FastAPI, handles auth and data

# Just name (will ask for details)
/pp-add mobile

# With dependencies
/pp-add frontend - React dashboard, depends on api for data
```
