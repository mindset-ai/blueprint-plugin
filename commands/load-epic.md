---
description: Load epic context and skills for a product planning session. Read EPIC.md, research docs, and find linked features.
---

# Load Epic Command

Load all context for an epic. Use this when working on product requirements, reviewing epic status, or before planning features.

## Usage

```
/load-epic {epic-name}
```

## What This Command Does

1. **Find the epic** at `blueprint/pods/pm/epics/{epic-name}/`
2. **Read `EPIC.md`** - Requirements, user stories, scope, success metrics
3. **List research docs** in `research/` folder
4. **Find linked features** - Scan all pods for features with matching `Epic: {epic-name}` in their FEAT.md. List each with pod, name, and status.
5. **Parse Skills section** from EPIC.md and read each declared skill
6. **Output summary**

## Epic Document Location

```
blueprint/pods/pm/epics/{epic-name}/
├── EPIC.md         # Requirements, scope, success metrics, features table
└── research/       # Research documents and analysis
```

## Finding Linked Features

Search across all pods for features that reference this epic:

```bash
grep -rl "Epic:.*{epic-name}" blueprint/pods/*/features/*/FEAT.md 2>/dev/null
```

For each match, read the feature's status from its FEAT.md header.

## Output Summary

```markdown
# Epic Loaded: {epic-name}

## Status
{Current status from EPIC.md}

## Summary
{Summary from EPIC.md}

## Requirements ({count})
{Numbered list}

## Linked Features
| Pod | Feature | Status |
|-----|---------|--------|
{Table of features with Epic: {epic-name}}

## Research Documents
{List of files in research/}

## Open Questions
{From EPIC.md}

## Skills Loaded
{List of skills read}

Ready to work on {epic-name}.
```

## When Epic Not Found

If the epic folder doesn't exist, list available epics:

```bash
ls blueprint/pods/pm/epics/ 2>/dev/null
```

And prompt the user to choose one or create a new epic with `/new-epic`.

## Integration with Other Commands

```
/load-epic epic-journeys       # Load epic context
/new-feature feat-x --epic epic-journeys  # Create linked feature
/load-feature feat-x           # Also loads parent epic
/update-epic epic-journeys     # Update epic after work
```
