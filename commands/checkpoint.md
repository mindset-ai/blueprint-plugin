# Checkpoint Command (Blueprints Handoff)

Create a session handoff document for the current feature and update its context.

## Usage

`/checkpoint [feature-name]`

If `feature-name` is not provided, prompt the user to specify which feature they're working on.

## What This Command Does

1. Creates a timestamped handoff document at:
   ```
   blueprints/features/[feature-name]/handoffs/YYYY-MM-DD-session.md
   ```

2. Updates the feature's `context.md` with session progress

3. Preserves any useful state verification information

## Handoff Document Structure

Create the handoff file with this structure:

```markdown
# Session Handoff: [Feature Name]

**Date:** YYYY-MM-DD
**Session Focus:** [Brief description of what this session addressed]

---

## What Was Accomplished

### Backend (Python/pythonia)
- [List of backend work completed]
- [Include specific files modified]
- [Note any new endpoints or changes]

### Frontend (Flutter/React)
- [List of frontend work completed]
- [Include specific files modified]
- [Note any UI changes or new components]

### Other
- [Documentation, configuration, or cross-cutting changes]

---

## What's Working

- [List features/functionality that are now working]
- [Include any test results or verification steps performed]
- [Note integration points that are functioning]

---

## What's Blocked

| Blocker | Impact | Notes |
|---------|--------|-------|
| [Description] | [What it prevents] | [Any workarounds or context] |

---

## Commands Used

```bash
# Key commands run during this session
[command 1]
[command 2]
```

---

## Next Steps

### Immediate (Next Session)
1. [First priority item]
2. [Second priority item]

### Backend Tasks
- [ ] [Python/backend task]

### Frontend Tasks
- [ ] [Flutter/React task]

### Blocked Until
- [ ] [Task waiting on blocker resolution]

---

## Session Notes

[Any additional context, decisions made, or observations that would help the next session]

---

## Verification State

**Git Status at End of Session:**
- Branch: `[current branch]`
- Uncommitted changes: [yes/no - list key files if yes]

**Tests:**
- Python tests: [pass/fail/not run]
- Flutter tests: [pass/fail/not run]
- Integration: [pass/fail/not run]
```

## Updating context.md

After creating the handoff, update the feature's `context.md`:

1. Add a new entry to the **Session Log** section:
   ```markdown
   ### YYYY-MM-DD - [Session Title]
   - **Focus:** [What was worked on]
   - **Accomplished:** [What got done]
   - **Blockers Encountered:** [Any issues]
   - **Next Session:** [What to pick up next]
   ```

2. Update **What's Done** if items were completed

3. Update **What's In Progress** with current status

4. Update **What's Next** if priorities changed

5. Update **Current Blockers** table if blockers changed

## Workflow

Typical checkpoint flow during a development session:

```
[Start Session]
   |
   v
Read context.md and latest handoff
   |
   v
[Do Work]
   |
   v
/checkpoint [feature-name]
   |
   v
Review generated handoff
   |
   v
[End Session - user commits if desired]
```

## Finding Features

If the user doesn't specify a feature, list available features:

```bash
ls -d blueprints/features/feat-* 2>/dev/null | xargs -n1 basename
```

Or check for features in the pods structure:
```bash
find blueprints/pods/*/features -maxdepth 1 -type d -name "feat-*" 2>/dev/null
```

## Arguments

$ARGUMENTS:
- `[feature-name]` - The feature to create a handoff for (e.g., `feat-widget-mcp`, `feat-journeys`)

## Example

```
/checkpoint feat-widget-mcp
```

Creates:
- `blueprints/features/feat-widget-mcp/handoffs/2026-01-25-session.md`
- Updates `blueprints/features/feat-widget-mcp/context.md` session log

## Multiple Sessions Same Day

If a handoff already exists for today, append a session number:
- First session: `2026-01-25-session.md`
- Second session: `2026-01-25-session-2.md`
- Third session: `2026-01-25-session-3.md`

## Notes

- Always read the existing `context.md` before updating it
- Preserve existing content in `context.md` - only add/update relevant sections
- If the handoffs directory doesn't exist, create it
- The handoff should be detailed enough that someone unfamiliar with the session can pick up where you left off
