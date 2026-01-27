---
description: Update feature documentation based on current session work. Sync FEAT.md, context.md, and TRACKING.md with new decisions, blockers, and progress.
---

# Update Feature Command

Update feature documentation to reflect work done in the current session. This keeps feature files in sync with reality.

## Usage

```
/update-feature feat-journeys
/update-feature feat-widget-mcp
```

## What This Command Does

1. **Review current session** - Look at what was accomplished, decisions made, blockers hit
2. **Read existing feature files** - Load current state of FEAT.md, context.md, TRACKING.md
3. **Identify what changed** - Compare session work against documented state
4. **Propose updates** - Show what will be updated and get user confirmation
5. **Update files** - Apply changes to feature documentation

## Files That May Be Updated

### FEAT.md
- **Status** - Update if phase/progress changed
- **What's Working** - Add newly working functionality
- **Blockers** - Add new blockers, remove resolved ones
- **Key Decisions** - Add decisions made this session
- **What's Next** - Update priorities based on progress

### context.md
- **Current State** - Reflect latest progress
- **Blockers** - Add/remove/update blockers
- **Session Log** - Add entry for this session

### TRACKING.md
- **Files Changed** - Add new files modified
- **Branches** - Update branch status if changed
- **Deployment Order** - Update if dependencies changed

## Workflow

```
/load-feature feat-journeys     # Start: load context
[... do work ...]
/update-feature feat-journeys   # During/End: sync documentation
/checkpoint feat-journeys       # End: create handoff for next session
```

**Note:** `/update-feature` updates the living documents. `/checkpoint` creates a point-in-time handoff.

## What to Update

When running this command, analyze the session for:

### New Blockers
```markdown
## Found in session:
- MCP server deployment failing with auth error
- Google Maps alias preventing widget integration

## Add to Blockers section in FEAT.md:
1. **MCP Auth Failure** - Priority: High
   - Status: Needs investigation
   - Details: Server returns 401 despite correct token
   - Relevant code: `mindset-tools/packages/widget-creator/src/auth.ts`
```

### Resolved Blockers
```markdown
## Resolved in session:
- Fixed Envoy IPv6 routing issue

## Remove from Blockers, add to What's Working:
- Envoy proxy correctly routes to Python backend (IPv4)
```

### Key Decisions
```markdown
## Decisions made in session:
- Use Deepgram instead of OpenAI Realtime for voice
- Store API keys in Firestore, not environment variables

## Add to Key Decisions table:
| Use Deepgram Voice Agent API | Lower cost, LLM flexibility, equivalent latency | 2026-01-27 |
| Firestore for API key storage | Enables per-app configuration | 2026-01-27 |
```

### Progress Updates
```markdown
## Completed in session:
- Voice MCP server basic implementation
- Widget canvas state management

## Update What's Working:
**mindset-tools:**
- Voice MCP server with Deepgram integration
- WebSocket connection management

## Update What's Next (remove completed items):
- [x] ~~Voice MCP server implementation~~ (done)
- [ ] Deploy MCP server to Cloud Run (next)
```

## Example

```
User: /update-feature feat-widget-mcp

Claude: I'll review the session and propose updates to feat-widget-mcp.

## Session Summary

**Work completed:**
- Fixed agent looping issue in ReActAgentNode
- Added streaming progress updates to MCP tools

**Decisions made:**
- Use tuple response format for MCP tools

**New blockers:**
- None

**Resolved blockers:**
- Agent looping on consecutive modifications (was High priority)

## Proposed Updates

### FEAT.md

**Remove from Blockers:**
```diff
- 1. **Agent Looping on Consecutive Modifications** - Priority: High
-    - Status: Needs investigation
-    - Details: Agent works for initial widget creation but enters tool-calling loop...
```

**Add to What's Working (pythonia):**
```diff
+ - Consecutive widget modifications working correctly
+ - Streaming progress updates during tool execution
```

**Add to Key Decisions:**
```diff
+ | Tuple response format for MCP | Enables content + artifact separation | 2026-01-27 |
```

**Update What's Next:**
```diff
- 1. **Fix agent looping on consecutive modifications**
+ 1. **Deploy MCP server to Cloud Run** (was #2)
```

Proceed with these updates? (yes/no/modify)
```

## Important Notes

- **Always show diff before applying** - User must confirm changes
- **Preserve existing content** - Only modify relevant sections
- **Use proper formatting** - Match existing style in each file
- **Don't duplicate /checkpoint work** - This updates docs, checkpoint creates handoffs
- **Keep it focused** - Only update what actually changed this session

## When to Use

| Situation | Command |
|-----------|---------|
| Start of session | `/load-feature` |
| Made significant progress | `/update-feature` |
| Hit a new blocker | `/update-feature` |
| Made important decision | `/update-feature` |
| End of session | `/update-feature` then `/checkpoint` |

## Integration with Other Commands

```
/load-feature feat-journeys    # Read context
/plan                          # Plan work (updates What's Next)
/tdd                           # Implement
/update-feature feat-journeys  # Sync docs with progress
/checkpoint feat-journeys      # Create handoff
```
