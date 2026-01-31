---
description: List all features across all pods with their pod and status.
---

# List Features Command

List all features organized by pod.

## Usage

```
/list-features
/list-feats
```

## What This Command Does

1. **Glob** for all `FEAT.md` files at `blueprint/pods/*/features/*/FEAT.md`
2. **Skip** the `_template` pod directory
3. **Extract** the pod name and feature name from the file path: `blueprint/pods/{pod}/features/{feature}/FEAT.md`
4. **Read the frontmatter or first heading** of each `FEAT.md` to extract the feature title and status if available
5. **Group features by pod**
6. **Display a table** sorted by pod then feature name

## Output Format

Display results as a markdown table:

```
| Pod | Feature | Status |
|-----|---------|--------|
| platform | blueprint-claude-code-setup | Active |
| wip | feat-journeys | In Progress |
| wip | feat-widget-mcp | In Progress |
```

After the table, show:
- Total count of features
- Reminder: `Use /load-feature <name> to load a feature`

## Notes

- Features live at `blueprint/pods/{pod}/features/{feature-name}/`
- The `_template` pod should always be excluded
- Status comes from the `## Status` section in FEAT.md if present; otherwise show `—`
