---
description: Create a new epic folder with EPIC.md from template
---

# New Epic Command

Creates a new epic in the `pm` pod.

## Usage

```
/new-epic {epic-name}
```

Examples:
```
/new-epic epic-knowledge-search
/new-epic epic-voice-journeys
```

## What This Command Does

1. **Validate name** - Epic names should use kebab-case with `epic-` prefix
2. **Create folder** at `blueprints/pods/pm/epics/{epic-name}/`
3. **Copy EPIC.md** from `blueprints/pods/_template/epics/_template/EPIC.md`
4. **Replace placeholders**:
   - `{Epic Name}` → Epic name (title case, spaces)
   - `{YYYY-MM-DD}` → Today's date
5. **Create `research/` subdirectory**
6. **Confirm and suggest next steps**

## Process

### Step 1: Create Structure

```
blueprints/pods/pm/epics/{epic-name}/
├── EPIC.md         # Product requirements, scope, success metrics
└── research/       # For research documents
```

### Step 2: Initialize EPIC.md

Copy from template and replace placeholders.

### Step 3: Confirm

```
Created: blueprints/pods/pm/epics/{epic-name}/
├── EPIC.md
└── research/

Next steps:
1. Edit EPIC.md to add summary, requirements, and user stories
2. Create features with /new-feature feat-x --epic {epic-name}
3. Add research documents to research/ as needed
```

## Template Location

- EPIC.md template: `blueprints/pods/_template/epics/_template/EPIC.md`

## Notes

- Epic names should use kebab-case with `epic-` prefix (e.g., `epic-voice-journeys`)
- All epics live in the `pm` pod at `blueprints/pods/pm/epics/`
- Link features to epics using the `Epic:` field in FEAT.md
