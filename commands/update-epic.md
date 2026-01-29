---
description: Update epic documentation based on current session work. Sync EPIC.md with new decisions, requirements changes, and feature status.
---

# Update Epic Command

Update an epic's documentation after a session. Reviews work done and proposes changes to EPIC.md.

## Usage

```
/update-epic {epic-name}
```

## What This Command Does

1. **Read existing EPIC.md** from `blueprints/pods/pm/epics/{epic-name}/`
2. **Review session work** - Look at conversation history for decisions, requirement changes, new questions
3. **Scan linked features** - Check current status of all features with `Epic: {epic-name}`
4. **Propose updates** - Show diff of proposed changes to EPIC.md
5. **Get user confirmation** before writing
6. **Update EPIC.md** with confirmed changes

## What Gets Updated

- **Status** - If epic status has changed
- **Last Updated** - Set to today's date
- **Requirements** - New or modified requirements
- **Features table** - Updated feature statuses from scanning pods
- **Decisions** - New decisions made during session
- **Open Questions** - Resolved questions removed, new ones added
- **Success Metrics** - Refined metrics

## Output Format

```markdown
# Proposed Updates to {epic-name}

## Changes:
1. Updated Features table (feat-x status: In Progress → In Review)
2. Added Decision: "Use WebSocket for real-time updates"
3. Resolved Open Question: "Which streaming protocol?"
4. Added Requirement #5: "Support offline mode"

## Diff:
[Show specific text changes]

Apply these updates? (y/n)
```

## Notes

- This command reads files and proposes changes but waits for confirmation
- Always updates the `Last Updated` date
- Features table is rebuilt by scanning all pods for matching `Epic:` fields
