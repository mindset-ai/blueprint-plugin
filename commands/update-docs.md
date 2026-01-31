---
description: Update sdk2-docs documentation site based on current feature work
---

# Update Documentation

Update the Mintlify-based sdk2-docs site to reflect current feature work.

## What This Command Does

1. Determine which feature work needs documenting (ask if not obvious)
2. Read sdk2-docs/CLAUDE.md and docs.json for site structure
3. Search existing MDX files for related content
4. Propose changes: edits to existing files, new files, navigation updates
5. Wait for user approval
6. Create a docs branch in sdk2-docs repo
7. Make the approved changes
8. Show summary of all changes

## Process

### Step 1: Gather Context
- If a feature is loaded, use that context
- Otherwise ask: "Which feature or work do you want to document?"
- Ask: "What specifically changed that needs documenting?" with options like:
  - New feature/capability
  - API changes
  - SDK integration changes
  - Configuration/setup changes
  - Other

### Step 2: Analyse sdk2-docs
- Read sdk2-docs/CLAUDE.md for site structure
- Read sdk2-docs/docs.json for navigation and content organization
- Search existing MDX files for content related to the feature
- Identify which section(s) the changes belong in:
  - features/ (feature overviews for AI Managers)
  - deploy/ (SDK/API integration for developers)
  - build-optimize/ (how-to guides)
  - introduction/ (getting started)

### Step 3: Propose Changes
Present a clear plan showing:
- **Files to update**: Path, what changes, why
- **New files to create**: Path, title, brief outline
- **Navigation changes**: Any docs.json updates needed

Format as:
```
Changes for sdk2-docs:

MODIFY: deploy/sdk3/sdk3-features.mdx
  → Add section on [new feature name]

CREATE: deploy/sdk3/sdk3-[feature].mdx
  → New page covering [description]
  → Add to docs.json navigation under Deploy > SDK 3.0

MODIFY: docs.json
  → Add new page to SDK 3.0 navigation group
```

### Step 4: Get Approval
Ask: "Proceed with these changes? (yes/no/modify)"

### Step 5: Create Branch & Make Changes
- cd to sdk2-docs/
- Create branch: `docs/{feature-name}`
- Make approved edits/creates
- Follow Mintlify MDX conventions (use Steps, Cards, Tabs, Notes, Tips, etc.)

### Step 6: Summary
Show all files changed/created and remind user:
- Review changes
- Run `mintlify dev` to preview locally
- Create MR when ready

## Important
- All content must follow Mintlify MDX format
- Use existing components: Steps, Step, Cards, CardGroup, Tabs, Tab, Note, Tip, Warning, ParamField
- Match the tone and style of existing sdk2-docs content
- sdk2-docs repo is at: ../sdk2-docs/ (relative to blueprint)
- NEVER commit - let the user review and commit
