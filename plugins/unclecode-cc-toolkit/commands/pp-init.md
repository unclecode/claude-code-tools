# Initialize Project Management Structure

Generate a multi-subproject management structure in `.context/project/`.

## Arguments

$ARGUMENTS

## Assets Location

Template files are in `~/.claude/commands/pp-init-assets/templates/`:
- `root/` - Root level templates (WORKFLOW.md, PRINCIPLES.md, etc.)
- `subproject/` - Subproject templates

## Execution Instructions

### Step 1: Parse Arguments

Check for mode:
- `--quiet` or `-q` → Quiet mode
- Contains text (not flags) → Description mode
- Empty → Interactive mode

### Step 2: Create Directory Structure

```bash
# Create root directories
mkdir -p .context/project/prds .context/project/docs .context/project/archive

# Create subproject directories (for each subproject)
mkdir -p .context/project/{subproject}/prds
mkdir -p .context/project/{subproject}/docs
mkdir -p .context/project/{subproject}/archive
mkdir -p .context/project/{subproject}/history
```

**Note**: The `history/` directory stores ultra-compact session summaries created by `/pp-checkpoint`.

### Step 3: Copy Fixed Templates

These files are copied as-is (no placeholders to fill):

```bash
# Copy WORKFLOW.md and PRINCIPLES.md (fully populated)
cp ~/.claude/commands/pp-init-assets/templates/root/WORKFLOW.md .context/project/
cp ~/.claude/commands/pp-init-assets/templates/root/PRINCIPLES.md .context/project/
cp ~/.claude/commands/pp-init-assets/templates/root/LESSONS.md .context/project/
```

### Step 4: Process Template Files

For files with `{{PLACEHOLDER}}` markers:
1. Read template from assets
2. Replace placeholders with collected data
3. Write to destination

**Root templates to process:**
- INDEX.md
- STATUS.md
- CODEBASE.md
- CHANGELOG.md

**Subproject templates to process (for each subproject):**
- INDEX.md
- STATUS.md
- TODO.md
- CHANGELOG.md
- PRINCIPLES.md
- CODEBASE.md
- LESSONS.md

### Step 5: Mode-Specific Behavior

#### Quiet Mode (`--quiet`)

1. Create structure with 1 default subproject named "main"
2. Copy fixed templates
3. Process other templates with minimal placeholders:
   - `{{DATE}}` → current date
   - `{{PROJECT_NAME}}` → "Project"
   - `{{SUBPROJECT_NAME}}` → "main"
   - Other placeholders → leave as "TBD" or empty tables
4. No questions asked

#### Description Mode (text provided)

1. Parse description for:
   - Project name
   - Subproject names (look for patterns like "with X, Y, Z subprojects")
   - Active subproject (look for "starting with" or first mentioned)
   - Technologies mentioned
2. Ask clarifying questions only if critical info missing
3. Generate with parsed data

#### Interactive Mode (no arguments)

Ask these questions:
1. "What is the project name?"
2. "Brief description (one line):"
3. "List subprojects (comma-separated, e.g., 'frontend, backend, mobile'):"
4. "Which subproject is active first?"
5. "Key technologies? (optional)"
6. "Any environment details? (optional)"

Then generate with collected data.

## Placeholder Reference

### Root Level

| Placeholder | Description |
|-------------|-------------|
| `{{DATE}}` | Current date (YYYY-MM-DD) |
| `{{PROJECT_NAME}}` | Project name |
| `{{PROJECT_DESCRIPTION}}` | One-line description |
| `{{SUBPROJECT_TABLE}}` | Markdown table of subprojects |
| `{{SUBPROJECT_STATUS_TABLE}}` | Status table for STATUS.md |
| `{{SUBPROJECT_CODEBASE_REFS}}` | List of subproject CODEBASE.md refs |
| `{{ACTIVE_SUBPROJECT}}` | Name of active subproject |
| `{{ENVIRONMENT}}` | Environment details |
| `{{SHARED_FILES}}` | List of shared files |
| `{{PROGRESS_CHECKLIST}}` | Overall progress checklist |
| `{{DEPENDENCIES}}` | Cross-project dependencies table |
| `{{DECISIONS}}` | Recent decisions table |
| `{{SUBPROJECTS}}` | Comma-separated subproject names |

### Subproject Level

| Placeholder | Description |
|-------------|-------------|
| `{{SUBPROJECT_NAME}}` | Subproject folder name |
| `{{SUBPROJECT_TITLE}}` | Human-readable title |
| `{{SUBPROJECT_DESCRIPTION}}` | What this subproject does |
| `{{SUBPROJECT_STATUS}}` | Current status |
| `{{SCOPE}}` | Bullet list of scope items |
| `{{WORKING}}` | What's currently working |
| `{{BLOCKED}}` | What's blocked |
| `{{FOCUS}}` | Current focus items |
| `{{NEXT_ACTIONS}}` | Next action items |
| `{{PHASE_1_TASKS}}` | Initial tasks |
| `{{FUTURE_TASKS}}` | Future phase tasks |
| `{{PRINCIPLES}}` | Subproject-specific principles |
| `{{RULES}}` | Subproject-specific rules |
| `{{EXISTING_FILES}}` | Existing file table rows |
| `{{TO_CREATE_FILES}}` | Files to create table rows |
| `{{FILE_DEPENDENCIES}}` | File dependencies table rows |
| `{{CREATION_CONTEXT}}` | Why subproject was created |

## After Generation

1. Show created structure (tree view)
2. List files that need customization
3. Suggest: "Fill in placeholders marked 'TBD', then create your first PRD"

## Examples

```bash
# Quiet mode - minimal structure
/pp-init --quiet

# With description
/pp-init E-commerce platform with frontend (React), backend (FastAPI), and mobile (React Native). Starting with backend.

# Interactive
/pp-init
```
