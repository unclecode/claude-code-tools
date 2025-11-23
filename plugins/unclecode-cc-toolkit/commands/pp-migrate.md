# Migrate Existing Project to Project Planning Structure

Convert an existing project (with scattered markdown/docs) to use `.context/project/` structure.

## Arguments

$ARGUMENTS

## Overview

This command:
1. Launches explorer agents to scan and understand your project
2. Discovers existing markdown files, subprojects, and current state
3. Creates `.context/project/` structure with pre-filled content
4. Optionally moves existing files to proper locations

## Execution Instructions

### Step 1: Initial Scan with Explorer Agents

Launch 5 parallel explorer agents to gather project information:

**Agent 1: Markdown Discovery**
```
Find all markdown files in the project. For each file:
- Path
- Title/purpose (from content)
- Type: documentation, decision log, notes, readme, spec, etc.
Report as structured list.
```

**Agent 2: Project Structure Analysis**
```
Analyze the codebase structure:
- Main directories and their purposes
- Identify potential subprojects/modules (e.g., frontend/, backend/, api/)
- Key technologies used
- Entry points and main files
Report the logical structure.
```

**Agent 3: Current State Discovery**
```
Find indicators of current project state:
- TODOs in code comments
- README status sections
- Any existing tracking files
- Recent git commits (last 10) for context
- Package.json, requirements.txt, etc. for dependencies
Report what's working, what's in progress.
```

**Agent 4: Configuration & Environment**
```
Find configuration details:
- Environment files (.env.example, config files)
- Build/deployment configs
- API endpoints or external services
- Database schemas if present
Report environment setup.
```

**Agent 5: Documentation Quality**
```
Assess existing documentation:
- Is there a main README?
- API docs?
- Architecture docs?
- Are docs up to date or stale?
Report documentation gaps and strengths.
```

### Step 2: Synthesize Findings

After agents complete:
1. Combine all findings
2. Identify:
   - Project name (from package.json, README, or directory)
   - Subprojects (from structure analysis)
   - Current state (from state discovery)
   - Existing files to migrate
   - Documentation gaps

### Step 3: Ask Clarifying Questions

Present findings and ask:

1. "I found these potential subprojects: [list]. Is this correct? Any to add/remove?"
2. "Which subproject should be active/primary?"
3. "I found [N] markdown files. Should I organize them into the new structure?"
4. "Any additional context about the project I should know?"

### Step 4: Generate Structure with Content

Create `.context/project/` structure using `pp-init-assets/templates/` but:

**Pre-fill root INDEX.md instead of placeholders:**

- **INDEX.md**: Use discovered project name, description from README, list actual subprojects, set active subproject, add discovered high-level TODOs, fill subproject status table
- **WORKFLOW.md**: Copy as-is
- **PRINCIPLES.md**: Copy as-is
- **LESSONS.md**: Copy as-is

**For each subproject:**

- **INDEX.md**: Subproject overview
- **TODO.md**: Convert discovered TODOs from code into task list
- **STATUS.md**: Actual status from analysis
- **CODEBASE.md**: Files in that subproject
- **CHANGELOG.md**: Initialize with recent git history summary for that subproject
- **PRINCIPLES.md**: Only if subproject has specific principles (usually skip)
- **LESSONS.md**: Copy from root template

### Step 5: Migration Plan (Requires Confirmation)

Present migration plan:

```
Files to move:
- docs/api-spec.md → .context/project/{subproject}/docs/
- DECISIONS.md → .context/project/{subproject}/docs/
- old-notes.md → .context/project/archive/

Files to keep in place:
- README.md (root)
- CONTRIBUTING.md (root)
```

Ask: "Should I proceed with moving these files? (y/n)"

If yes, execute moves.
If no, skip migration, just create structure.

### Step 6: Final Report

Show:
1. Created structure (tree view)
2. Files that were migrated
3. Suggested next steps:
   - Review generated STATUS.md for accuracy
   - Add any missing TODOs
   - Create first PRD for current work

## Templates Reference

Uses same templates as `/pp-init` from:
`~/.claude/commands/pp-init-assets/templates/`

But fills content from discovered information instead of placeholders.

## Examples

```bash
# Basic migration
/pp-migrate

# With context hint
/pp-migrate This is a Facebook ads automation project with webhook server and analytics dashboard

# Skip file migration
/pp-migrate --no-move
```

## Flags

- `--no-move`: Create structure but don't move existing files
- `--dry-run`: Show what would be done without making changes

## Notes

- Always backs up files before moving (copies to archive first)
- Preserves git history by using `git mv` when possible
- If `.context/project/` already exists, asks before overwriting
