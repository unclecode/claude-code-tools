# Principles & Rules (Root)

Base methodology and rules for all subprojects. Subprojects can extend/override.

---

## Core Philosophy

**Small, Smart, Strong**
- Small changes, well-tested
- Smart solutions, not over-engineered
- Strong foundations before features

---

## Debugging Methodology

### Inverted Confidence Rule

**Code I wrote this session**: 20% confidence it's correct
- Assume I made a mistake
- Prove it works before moving on
- Test thoroughly

**Existing code**: 80% confidence it's correct
- Assume it works as intended
- Prove it's broken before touching it
- Respect that it was tested/debugged

### Debugging Decision Tree

```
Bug appears
├─ Is there NEW code involved?
│  └─ YES (95% probability) → Start here
│     └─ Test new code in isolation
│     └─ Compare with working baseline
│     └─ Only if proven correct → investigate old code
│
└─ Is it ONLY old code?
   └─ Verify nothing changed
   └─ Check environment, dependencies, inputs
```

### Before Blaming Existing Code

1. Why would this have worked before if it's broken?
2. What changed between working and not working?
3. Is there any NEW code in the path?

If answer to #3 is YES → investigate new code FIRST.

---

## Feature Addition Approach

1. **Understand first** - Read existing code, understand patterns
2. **Plan before coding** - Explain changes, get confirmation
3. **Minimal changes** - Don't refactor unrelated code
4. **Test incrementally** - Small steps, verify each
5. **Document** - Update TODO, CHANGELOG, relevant docs

---

## Code Change Rules

1. **Focus on explicit request** - Don't add unrequested changes
2. **Root cause analysis** - For non-trivial bugs, explain before fixing
3. **Explain fundamental changes** - Before implementing, discuss
4. **Don't over-commit** - Test before suggesting commit
5. **Stay doubtful** - Don't assume it works, verify

---

## Error Handling

- Technical issues → Inform user, explain what's being checked
- Unexpected inputs → Handle gracefully or escalate
- Escalate early if uncertain

---

## Testing Approach

1. Unit test new functions
2. Integration test with mocked APIs
3. End-to-end test with real systems
4. Log everything during testing
5. Verify before marking task complete

---

# Rules (Dos & Don'ts)

Hard rules for Claude behavior. Added by user based on observed issues.

**Never remove rules. Only add.**

Format: `R###: Rule description` + Context

---

## General Behavior

**R001**: Do not add "co-author" or Claude Code references in commit messages
Context: User preference, keep commits clean

**R002**: Do not say "you're absolutely right" or similar excessive validation
Context: Maintain objectivity, don't sugar coat

**R003**: Do not assume task is done and suggest commit without testing
Context: Stay doubtful, verify before declaring success

**R004**: Do not start implementation without confirmation when user explains feature
Context: User wants to see proposed changes first, then approve

**R005**: When explaining changes, wait for confirmation before coding
Context: Avoid wasted work on wrong approach

**R006**: Always show major file content for review before writing
Context: User is system designer, needs to approve before files are created

**R007**: Commit after completing each small, logical change
Context: Git history becomes a book telling the story of how the app was built. Each commit is a chapter. Keeps history clean and reviewable.

---

## Code & Architecture

**R010**: Do not create new files unless absolutely necessary
Context: Prefer editing existing files

**R011**: Do not refactor unrelated code when fixing a bug
Context: Focus on explicit request only

**R012**: Do not introduce security vulnerabilities (injection, XSS, etc.)
Context: Standard security practice

---

## Documentation

**R020**: Update project docs (TODO, CHANGELOG, STATUS) after changes
Context: Maintain context for future sessions

**R021**: Log debugging experiences in LESSONS.md
Context: Don't repeat past debugging efforts

---

<!--
Add new rules below. Use next available R### number.
Subproject-specific rules go in subproject's PRINCIPLES.md
-->
