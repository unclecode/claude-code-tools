# Project Progress System - Help Guide

Complete guide for using the Project Progress (PP) system.

## Arguments

$ARGUMENTS

Optional: Specify topic for focused help (e.g., `/pp-help init`, `/pp-help workflow`)

## Overview

The Project Progress system helps you track development with:
- **Subproject-based organization** (main, backend, frontend, etc.)
- **Structured documentation** (STATUS, TODO, CHANGELOG, CODEBASE)
- **Session continuity** (checkpoint and resume between chats)
- **AI-optimized format** (easy for Claude to understand context)

---

## Quick Start

### For New Projects

```bash
# Interactive mode (recommended for first time)
/pp-init

# Or quiet mode (instant setup with "main" subproject)
/pp-init --quiet
```

### For Existing Projects

```bash
# Migrate existing project (uses AI agents to discover structure)
/pp-migrate
```

---

## File Structure

```
.context/project/
├── INDEX.md              # Project overview + active subproject + high-level TODOs
├── WORKFLOW.md           # How to use this system
├── PRINCIPLES.md         # Project principles
├── LESSONS.md            # Lessons learned
│
└── {subproject}/         # e.g., "main", "backend", "frontend"
    ├── STATUS.md         # Current status, what's working/blocked
    ├── TODO.md           # Detailed task list
    ├── CHANGELOG.md      # Change history
    ├── CODEBASE.md       # File inventory
    ├── LESSONS.md        # Subproject-specific lessons
    ├── PRINCIPLES.md     # (optional) Subproject-specific principles
    ├── prds/             # Product requirement documents
    ├── docs/             # Documentation
    ├── archive/          # Archived items
    └── history/          # Session checkpoints
```

---

## Core Commands

### Start New Session
```bash
/pp-resume              # Load all context, continue work
/pp-resume [direction]  # Resume with specific focus
```

### Update Documentation
```bash
/pp-update                           # Interactive: ask what changed
/pp-update completed T010, T011      # Quick update with description
```

### Session Management
```bash
/pp-checkpoint                 # Save session, prepare for next chat
/pp-checkpoint [instructions]  # Save with specific next-session instructions
```

### View Status
```bash
/pp-status          # Full project status
/pp-status tasks    # Focus on tasks only
/pp-status blockers # Focus on blockers
```

### Manage Structure
```bash
/pp-add [checkpoint]    # Add a checkpoint
/pp-remove [checkpoint] # Remove a checkpoint
/pp-clean              # Clean temporary files
```

### Utilities
```bash
/pp-version  # Show plugin version
/pp-help     # This help guide
```

---

## Typical Workflow

### 1. First Session (New Project)

```bash
# Initialize structure
/pp-init

# Work on your project...
# (code, debug, build features)

# Before ending session
/pp-update added initial setup files
/pp-checkpoint Continue with user authentication
```

### 2. Next Session

```bash
# Load context
/pp-resume

# Claude shows: "Ready to continue with user authentication"
# Work on tasks...

# Update as you go
/pp-update completed T015, added auth service

# End session
/pp-checkpoint Implement password reset flow
```

### 3. Check Status Anytime

```bash
/pp-status
```

Shows:
- Active subproject
- In-progress tasks
- Pending tasks
- Blockers
- Recent changes

---

## File Purpose Guide

| File | When to Update | Purpose |
|------|----------------|---------|
| **INDEX.md** (root) | Switch active subproject, add high-level TODOs | Project overview + status |
| **WORKFLOW.md** | Never (system guide) | How to use PP system |
| **PRINCIPLES.md** | Add project-wide principles | Methodology & rules |
| **STATUS.md** (subproject) | State changes | Current status, working/blocked |
| **TODO.md** | Add/complete tasks | Task tracking |
| **CHANGELOG.md** | After changes | Change history with context |
| **CODEBASE.md** | Add/modify files | File inventory |
| **LESSONS.md** | After debugging | Problems → solutions |

---

## Best Practices

### Task Management (TODO.md)

```markdown
- [ ] T001: Task description
- [>] T002: In-progress task  (currently working)
- [x] T003: Completed task
- [-] T004: Cancelled task
```

### Changelog Format

```markdown
## 2025-01-15 | Commit: abc123

**Changes**: Added user authentication
**Usage**: `POST /api/auth/login` with email/password
**Context**: Using JWT with 24h expiry
```

### Status Clarity

Keep STATUS.md current:
- What's working
- What's blocked (and why)
- Current focus (1-3 items)
- Next actions

---

## Multiple Subprojects

```bash
# Initialize with multiple subprojects
/pp-init Full-stack app with backend (FastAPI), frontend (React), and mobile (React Native). Starting with backend.
```

Creates:
```
.context/project/
├── INDEX.md
├── WORKFLOW.md
├── PRINCIPLES.md
└── backend/
    └── (STATUS, TODO, CHANGELOG, etc.)
└── frontend/
    └── (STATUS, TODO, CHANGELOG, etc.)
└── mobile/
    └── (STATUS, TODO, CHANGELOG, etc.)
```

Switch active subproject by editing INDEX.md or using `/pp-update`.

---

## Getting Help

**By Topic:**
```bash
/pp-help init       # Help with initialization
/pp-help workflow   # Workflow guide
/pp-help commands   # Command reference
```

**Troubleshooting:**
- Structure missing? Run `/pp-init` or `/pp-migrate`
- Commands not working? Check `/pp-version`
- Need to update? Reinstall plugin

**Support:**
- GitHub: https://github.com/unclecode/claude-code-tools
- Issues: https://github.com/unclecode/claude-code-tools/issues

---

## Version

Run `/pp-version` to check your installed version.
