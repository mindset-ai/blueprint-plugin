---
description: Load feature context and skills for a development session. Read feature docs, latest handoff, and declared skills.
---

# Load Feature Command

Load all context needed to work on a feature. This is the recommended first command when starting a session.

## Usage

```
/load-feature feat-journeys
/load-feature feat-widget-mcp
```

## What This Command Does

1. **Find the feature** at `blueprints/pods/*/features/{feature-name}/`
2. **Read feature documents** in this order:
   - `FEAT.md` or `FEATURE.md` (primary living document)
   - `TRACKING.md` (branches, deployment order, files changed)
   - `context.md` (current state, blockers, decisions)
   - `CLAUDE.md` (feature-specific instructions and gotchas)
3. **Read the latest handoff** from `handoffs/` folder (most recent session summary)
4. **Check the Epic field** in FEAT.md. If set (not N/A), read `blueprints/pods/pm/epics/{epic-name}/EPIC.md` to load parent epic context
5. **Parse the Skills section** from FEAT.md and read each declared skill
6. **Output a concise summary** so you're ready to work

## Feature Document Locations

```
blueprints/pods/{pod}/features/{feature-name}/
├── FEAT.md         # Overview, status, architecture, skills declaration
├── TRACKING.md     # Cross-repo branches, deployment order
├── context.md      # Current state, blockers, what's next
├── CLAUDE.md       # Feature-specific instructions
└── handoffs/       # Session summaries
    └── YYYY-MM-DD-session.md
```

## Skills Declaration

Features declare required skills in their FEAT.md:

```markdown
## Skills
<!-- Loaded automatically by /load-feature -->
- python
- grpc
- react
```

Available skills are located at `blueprints/.claude/skills/{skill}/SKILL.md`:
- `python` - Pythonia cloud functions, LangGraph, gRPC services
- `flutter` - Flutter/Dart, GetX, ReactiveRecord, Admin Framework
- `react` - React SDK v3 development
- `grpc` - Protocol Buffers, Buf CLI, service patterns
- `firebase` - Firestore, authentication, security rules
- `mcp` - MCP server development and integration
- `testing` - TDD workflow, test patterns
- `architecture` - System design, cross-repo patterns

## Output Summary

After reading all context, output a summary:

```markdown
# Feature Loaded: {feature-name}

## Epic
{Epic name and key requirements, or "None"}

## Status
{Current status from FEAT.md}

## Current Blockers
{List from context.md or FEAT.md}

## Last Session ({date})
{Brief summary from latest handoff}

## What's Next
{Priority items from context.md}

## Skills Loaded
{List of skills read}

Ready to work on {feature-name}.
```

## When Feature Not Found

If the feature folder doesn't exist, list available features:

```bash
ls -d blueprints/pods/*/features/feat-* 2>/dev/null | xargs -n1 basename
```

And prompt the user to choose one or create a new feature with `/new-feature`.

## When Skills Section Missing

If FEAT.md doesn't have a `## Skills` section:
1. Note this in the output
2. Suggest adding one based on the feature's technology stack
3. Continue without loading skills

## Example Session

```
User: /load-feature feat-journeys

Claude: [Reads FEAT.md, TRACKING.md, context.md, CLAUDE.md, latest handoff]
        [Parses Skills: python, grpc, react]
        [Reads each skill file]

# Feature Loaded: feat-journeys

## Status
In Progress - Phase 1 complete, Phase 2 planning

## Current Blockers
- MCP-servers deployment (needs DevOps)
- Google Maps refactoring (required before widget integration)

## Last Session (2026-01-26)
- Completed voice MCP server implementation
- Tested Deepgram integration locally
- Identified Google Maps alias issue

## What's Next
1. Resolve MCP deployment with DevOps
2. Plan Phase 2 journey orchestration

## Skills Loaded
- python (LangGraph, cloud functions)
- grpc (Protocol Buffers, service patterns)
- react (SDK v3 development)

Ready to work on feat-journeys.
```

## Integration with Other Commands

Typical session flow:

```
/load-feature feat-journeys    # Start: load context
[... do work ...]
/plan-feature                  # If starting new work item
/tdd                           # Implement with tests
/code-review                   # Review changes
/checkpoint feat-journeys      # End: save session state
```

## Notes

- Run this at the **start of every session** to ensure full context
- Skills are read once and remain in conversation history
- If you switch features mid-session, run `/load-feature` again for the new feature
- This command reads files but does not modify anything
