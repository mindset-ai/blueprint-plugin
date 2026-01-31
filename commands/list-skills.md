---
description: List all available skills from the blueprint skills directory with their descriptions.
---

# List Skills Command

List all available skills with their names and descriptions.

## Usage

```
/list-skills
```

## What This Command Does

1. **Glob** for all `SKILL.md` files at `blueprint/.claude/skills/*/SKILL.md`
2. **Read the frontmatter** of each file to extract `name` and `description`
3. **Skip** the `learned` skill directory (internal use only)
4. **Display a table** of all available skills sorted alphabetically by name

## Output Format

Display results as a code block containing an ASCII table (NOT a markdown pipe-table, as the CLI renders those poorly):

```
┌──────────────┬─────────────────────────────────────────────────┐
│ Skill        │ Description                                     │
├──────────────┼─────────────────────────────────────────────────┤
│ ai-agent     │ LangGraph-based AI Agent v3 patterns...         │
├──────────────┼─────────────────────────────────────────────────┤
│ architecture │ System design, cross-repo patterns...           │
├──────────────┼─────────────────────────────────────────────────┤
│ ...          │ ...                                             │
└──────────────┴─────────────────────────────────────────────────┘
```

Add a `├──────────────┼──...┤` separator line between EVERY row, not just after the header.

IMPORTANT: Wrap the table in a fenced code block (```) so it renders with monospace alignment.

After the table, show:
- Total count of available skills
- Reminder: `Use /load-skill <name> to load a skill`

## Notes

- Skill files live at `blueprint/.claude/skills/{name}/SKILL.md`
- Each SKILL.md has YAML frontmatter with `name` and `description` fields
- The `learned` directory contains auto-captured patterns and should be excluded from the listing
