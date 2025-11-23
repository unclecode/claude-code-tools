# Project Workflow

How to work with this project management system.

---

## For New Chat Session

```
1. Read INDEX.md → understand project, current state, active subproject
2. Read WORKFLOW.md → understand system (this file)
3. Read PRINCIPLES.md → understand project principles
4. Go to active subproject folder (listed in INDEX.md)
5. Read subproject's STATUS.md, TODO.md, CODEBASE.md, CHANGELOG.md
6. Read subproject PRINCIPLES.md (if exists, overrides root)
7. Read LESSONS.md → avoid past mistakes
```

---

## After Making Changes

*All updates happen in the active subproject folder:*

1. **Update TODO.md** - Mark tasks done/in-progress
2. **Update CHANGELOG.md** - Log what changed with usage examples
3. **Update STATUS.md** - If system state changed
4. **Update LESSONS.md** - If debugging insight gained
5. **Update CODEBASE.md** - If files added/modified
6. **Update root INDEX.md** - If switching active subproject or adding high-level TODOs
7. **Commit** - Message references TODO ID

---

## File Update Guidelines

### TODO.md
- `[ ]` pending
- `[>]` in progress
- `[x]` completed
- `[-]` cancelled
- Format: `[status] T###: Description`
- Group by phase/PRD

### CHANGELOG.md
- Newest first (reverse chronological)
- Format: `## YYYY-MM-DD | Commit: <hash>`
- Include: Changes, Usage (examples/commands), Context
- Archive when >50 entries

### STATUS.md
- Keep current, not historical
- Sections: Working, Blocked, Current Focus, Recent Decisions
- Short bullet points

### LESSONS.md
- Format: Problem → Investigation → Root Cause → Solution
- Tag by category: [BUG], [CONFIG], [API], [LOGIC]
- Include searchable keywords

### CODEBASE.md
- Update after adding/modifying files
- One-line descriptions
- Group by component/purpose

---

## When to Create New PRD

- New phase starting
- Significant new feature
- Major refactor

PRD naming: `###_short_name.md` (e.g., `001_initial_setup.md`)

---

## When to Archive

**CHANGELOG.md**:
- When >50 entries or file getting very long
- Move older entries to `archive/CHANGELOG_YYYY_MM.md`
- Update archive index table in CHANGELOG.md with:
  - Archive filename
  - Date range
  - Topic summary
- Keep index in main file for reference

**PRDs**:
- Completed PRDs → move to archive/
- Keep reference in INDEX.md

**Outdated docs**:
- Move to archive/ with date suffix

---

## Commit Message Format

```
<type>: <short description>

[T###] <todo item completed>
[T###] <another if multiple>

<optional details>
```

Types: feat, fix, docs, refactor, test

Example:
```
feat: Add user authentication

[T010] Create auth service
[T011] Add login endpoint

Using JWT with 24h expiry
```

---

## File Inheritance

Root files apply to all subprojects. Subproject files extend/override.

**Reading order:**
1. Root level files (base)
2. Subproject files (extends/overrides)

Example: Root PRINCIPLES.md has base rules, subproject PRINCIPLES.md adds specific rules.

---

## Principles Reference

Before debugging or adding features, check:
1. PRINCIPLES.md - methodology (root + subproject)
2. LESSONS.md - past solutions (root + subproject)

Don't repeat past mistakes.
