# Triage Issue or Feature Request

Root cause investigation and solution design for issues, bugs, or feature requests.

## Arguments

$ARGUMENTS

Description of the issue or feature. Can include error messages, URLs, or attach screenshots.

## Purpose

Deep investigation to find ROOT CAUSES, not just symptoms. Uses triage-agent for systematic analysis.

**Philosophy:** Don't just treat the rash, find the root cause (diet, stress, etc.) and fix both.

## Execution Instructions

### Step 1: Collect Issue Information

From user query, gather:
- **Issue description** (what's happening)
- **Error messages** (if any)
- **Steps to reproduce** (if provided)
- **Expected vs actual behavior**
- **Screenshots/links** (if attached)
- **Affected area** (frontend, backend, API, etc.)

If critical info missing, ask clarifying questions:
- "What error message do you see?"
- "When does this happen?"
- "Which part of the system is affected?"

### Step 2: Read PP Context First

**IMPORTANT**: Before launching agent, gather context from PP files:

Read these files:
1. `.context/project/INDEX.md` - Identify active subproject
2. `.context/project/{active}/CODEBASE.md` - Understand file structure
3. `.context/project/{active}/STATUS.md` - Current state, known issues
4. `.context/project/{active}/CHANGELOG.md` (last 5-10 entries) - Recent changes
5. `.context/project/{active}/LESSONS.md` - Past solutions
6. `.context/project/PRINCIPLES.md` - Project principles

This context is passed to the agent so it doesn't re-explore everything.

### Step 3: Launch Triage Agent

Spawn the triage-agent with:
- Issue description
- PP context (from Step 2)
- Specific focus area (if mentioned)

```
Use Task tool with subagent_type: "triage-agent"
Prompt: Investigate this issue using PP context:

Issue: {user's issue description}

PP Context:
- Active subproject: {from INDEX.md}
- Codebase structure: {from CODEBASE.md}
- Current status: {from STATUS.md}
- Recent changes: {from CHANGELOG.md}
- Past lessons: {from LESSONS.md}
- Principles: {from PRINCIPLES.md}

Find ROOT CAUSE and provide solution or debugging plan.
```

### Step 4: Agent Investigation

The triage-agent will:
1. Start with PP context (not from scratch)
2. Investigate code systematically
3. Find root cause (causation, not just correlation)
4. Determine if cause is clear or needs debugging
5. Provide solution OR debugging technique
6. Format findings as structured report

### Step 5: Present Report

Agent returns report in this format:

```markdown
## Triage Report: {Issue Title}

**Date**: {YYYY-MM-DD HH:MM}
**Subproject**: {subproject name}
**Severity**: {Critical / High / Medium / Low}

---

### Issue Summary

{Brief description of reported issue}

---

### Root Cause Analysis

**Symptom**: {What appears to be happening}

**Investigation Path**:
1. {Step 1 of investigation}
2. {Step 2 of investigation}
3. {Step 3 of investigation}

**Root Cause**: {The actual underlying cause}

**Why This Matters**: {Explanation of causation, not just correlation}

---

### Solution

{IF ROOT CAUSE IS CLEAR}:

**Recommended Fix**:
1. {Step 1}
2. {Step 2}
3. {Step 3}

**Files to Modify**:
- `path/to/file.ext` - {what to change}
- `another/file.js` - {what to change}

**Implementation Notes**:
- {Important consideration 1}
- {Important consideration 2}

{IF ROOT CAUSE IS UNCLEAR}:

**Debugging Strategy**:
1. **Add logging**:
   - In `file.ext` line X: log variable Y
   - In `file2.js` line Z: log state before/after

2. **Test hypothesis**:
   - {Hypothesis 1}: Test by {method}
   - {Hypothesis 2}: Test by {method}

3. **Narrow down**:
   - {Approach to isolate the issue}

**Next Steps**:
1. {Action item 1}
2. {Action item 2}
3. {Re-run /pp-triage after gathering data}

---

### Related Context

**Recent Changes That May Be Related**:
- {CHANGELOG entry if relevant}

**Past Similar Issues**:
- {LESSONS.md entry if relevant}

**Affected Components**:
- {Component 1}
- {Component 2}

---

### Recommended Documentation Updates

**Add to LESSONS.md**:
```
## {Date} | {Issue Title}

**Problem**: {Brief problem}
**Root Cause**: {Cause}
**Solution**: {Solution}
**Tag**: [BUG] or [FEATURE] or [CONFIG]
```

**Update STATUS.md**:
- If blocking: Add to "Blocked" section
- If in progress: Add to "Current Focus"

---

### Confidence Level

**Root Cause Certainty**: {High / Medium / Low}
**Solution Confidence**: {High / Medium / Low}

{If low confidence, explain why and what additional info is needed}
```

### Step 6: Offer Follow-Up Actions

After presenting report, offer:

```
## Next Steps

Would you like me to:
1. Implement the recommended fix?
2. Add debugging logs as suggested?
3. Update LESSONS.md with this finding?
4. Create a TODO task for this fix?
5. Something else?
```

## Query Examples

```bash
# Bug report
/pp-triage Getting 500 errors on /api/users endpoint when user has no profile

# Feature request
/pp-triage Need to add email notifications when order status changes

# Performance issue
/pp-triage Dashboard loads slowly with more than 100 items

# With error message
/pp-triage TypeError: Cannot read property 'name' of undefined in UserProfile component

# With screenshot
/pp-triage [attach screenshot] Login button not working on mobile
```

## Notes

- Triage agent has access to all PP files for context
- Agent focuses on ROOT CAUSE, not symptoms
- If multiple possible causes, agent ranks by likelihood
- Report is designed to be saved in docs/ or archive/ for reference
- Always offer to update LESSONS.md with findings
- Principle: Fix the cause (diet) not just the symptom (rash)

## Agent Behavior

The triage-agent is trained to:
- Read PP context FIRST (don't re-explore)
- Think systematically (hypothesis → test → conclude)
- Find causation, not correlation
- Provide actionable solutions
- Suggest debugging when uncertain
- Be honest about confidence level
- Format output for easy scanning and future reference
