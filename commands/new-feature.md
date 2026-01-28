---
description: Create a new feature folder with FEAT.md and TRACKING.md from templates
---

# New Feature Command

Creates a new feature folder structure in the blueprints repository.

## What This Command Does

1. **Prompt for details** - Get feature name and pod location
2. **Create folder** - Set up `blueprints/pods/{pod}/features/{feature-name}/`
3. **Copy templates** - Create FEAT.md and TRACKING.md from templates
4. **Initialize content** - Replace placeholders with feature-specific values
5. **Create archive folder** - For historical documents

## Usage

```
/new-feature {feature-name}
/new-feature {feature-name} in {pod-name}
```

Examples:
```
/new-feature feat-knowledge-search
/new-feature feat-widget-export in wip
/new-feature feat-auth-refresh in core
```

## Process

### Step 1: Determine Location

If pod not specified, ask user which pod:
- `wip` - Wip pod features
- `platform` - Core platform pod features
- Or create a new pod after verifying we need a new pod

### Step 2: Create Structure

```
blueprints/pods/{pod}/features/{feature-name}/
├── FEAT.md         # Living document - status, blockers, patterns, local dev
├── TRACKING.md     # Cross-repo branches, deployment order
└── archive/        # For historical docs later
```

### Step 3: Initialize FEAT.md

Copy from `blueprints/pods/_template/features/_template/FEAT.md` and replace:
- `{Feature Name}` → Feature name (title case, spaces)
- `{branch-name}` → `feature/{feature-name}`
- `{YYYY-MM-DD}` → Today's date
- `{repos}` → Leave as placeholder for user to fill

### Step 4: Initialize TRACKING.md

Copy from `blueprints/pods/_template/features/_template/TRACKING.md` and replace:
- `{Feature Name}` → Feature name (title case, spaces)

### Step 5: Confirm

Display the created structure and next steps:
```
Created: blueprints/pods/{pod}/features/{feature-name}/
├── FEAT.md
├── TRACKING.md
└── archive/

Next steps:
1. Edit FEAT.md to add your feature summary and capabilities
2. Run /plan-feature {feature-name}: {description} to create implementation plan
```

## Template Locations

- FEAT.md template: `blueprints/pods/_template/features/_template/FEAT.md`
- TRACKING.md template: `blueprints/pods/_template/features/_template/TRACKING.md`

## Notes

- Feature names should use kebab-case with `feat-` prefix (e.g., `feat-widget-export`)
- The command creates minimal structure - flesh out FEAT.md before running `/plan-feature`
- Archive folder starts empty - move old docs here as feature progresses
