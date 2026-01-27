# Plan: Feature Context Commands (`/load-feature` & `/update-feature`)

**Status:** Ready for implementation
**Date:** 2026-01-27
**Session:** Initial design and implementation

---

## Problem Statement

When developers open a fresh Claude Code session, they need a simple way to get full context for the feature they're working on. The previous approach considered pre-loading/injecting data, but this caused concerns about:

1. **Token duplication** - If `/context` pre-loads skills, and `/plan` also loads skills, same content appears twice
2. **No deduplication mechanism** - Claude Code has no way to check "is this already loaded?"
3. **Eager vs lazy loading** - Pre-injecting everything wastes tokens if not all content is needed

---

## Solution: Instruction-Based Commands

After analyzing existing commands (`/plan`, `/tdd`, `/checkpoint`), we discovered they are **instruction documents**, not data loaders. They tell Claude what to do, and Claude uses the Read tool to fetch content. This approach:

- **Lazy loads** - Only reads files when needed
- **No duplication** - Tool results are in conversation history; Claude can see what was already read
- **Consistent** - Same pattern as all other commands

### Commands Created

| Command | Purpose | When to use |
|---------|---------|-------------|
| `/load-feature` | Read feature context + skills | Start of session |
| `/update-feature` | Write progress back to feature docs | During/end of session |

---

## `/load-feature` Command

**File:** `blueprints-cc-plugin/commands/load-feature.md`

### What It Does

1. **Find the feature** at `blueprints/pods/*/features/{feature-name}/`
2. **Read feature documents** in order:
   - `FEAT.md` or `FEATURE.md` (primary living document)
   - `TRACKING.md` (branches, deployment order)
   - `context.md` (current state, blockers)
   - `CLAUDE.md` (feature-specific instructions)
3. **Read latest handoff** from `handoffs/` folder
4. **Parse Skills section** from FEAT.md
5. **Read each declared skill** from `blueprints/.claude/skills/{skill}/SKILL.md`
6. **Output summary** - status, blockers, last session, what's next, skills loaded

### Skills Declaration

Features declare required skills in FEAT.md:

```markdown
## Skills
<!-- Loaded automatically by /load-feature -->
- python
- grpc
- react
```

### Available Skills

Located at `blueprints/.claude/skills/{skill}/SKILL.md`:

| Skill | Description | Approx Tokens |
|-------|-------------|---------------|
| `python` | Pythonia, LangGraph, gRPC services | ~4,400 |
| `flutter` | Flutter/Dart, GetX, ReactiveRecord | ~4,500 |
| `react` | React SDK v3 development | ~3,350 |
| `grpc` | Protocol Buffers, Buf CLI | ~2,900 |
| `firebase` | Firestore, auth, security rules | ~4,600 |
| `mcp-servers` | MCP server development | ~4,150 |
| `testing` | TDD workflow, test patterns | ~5,000 |
| `architecture` | System design, cross-repo | ~4,050 |

### Token Budget

| Component | Tokens | % of 200K |
|-----------|--------|-----------|
| Feature files (4 files) | ~2,750 | 1.4% |
| Latest handoff | ~750 | 0.4% |
| 3 skills (typical) | ~10,600 | 5.3% |
| **Total** | **~14,100** | **~7%** |

Leaves 93%+ of context for actual work.

### Example Output

```markdown
# Feature Loaded: feat-journeys

## Status
In Progress - Phase 1 complete, Phase 2 planning

## Current Blockers
- MCP-servers deployment (needs DevOps)
- Google Maps refactoring (required before widget integration)

## Last Session (2026-01-26)
- Completed voice MCP server implementation
- Tested Deepgram integration locally

## What's Next
1. Resolve MCP deployment with DevOps
2. Plan Phase 2 journey orchestration

## Skills Loaded
- python, grpc, react, mcp-servers

Ready to work on feat-journeys.
```

---

## `/update-feature` Command

**File:** `blueprints-cc-plugin/commands/update-feature.md`

### What It Does

1. **Review current session** - What was accomplished, decisions made, blockers hit
2. **Read existing feature files** - Current state of FEAT.md, context.md, TRACKING.md
3. **Identify changes** - Compare session work against documented state
4. **Propose updates** - Show diff and get user confirmation
5. **Update files** - Apply changes to feature documentation

### What Gets Updated

**FEAT.md:**
- Status (if phase/progress changed)
- What's Working (add newly working functionality)
- Blockers (add new, remove resolved)
- Key Decisions (add decisions made this session)
- What's Next (update priorities)

**context.md:**
- Current State
- Blockers
- Session Log entry

**TRACKING.md:**
- Files Changed
- Branches
- Deployment Order

### Key Principle

Always show diff before applying changes. User must confirm.

---

## FEAT.md Template Update

**File:** `blueprints/pods/_template/features/_template/FEAT.md`

Added Skills section after Key Capabilities:

```markdown
---

## Skills

<!-- Loaded automatically by /load-feature. See blueprints/.claude/skills/ for available skills. -->
<!-- Available: python, flutter, react, grpc, firebase, mcp-servers, testing, architecture -->

- {skill-1}
- {skill-2}

---
```

---

## Updated Existing Features

Added Skills sections to:

### feat-journeys
```markdown
## Skills
- python
- grpc
- react
- mcp-servers
```

### feat-widget-mcp
```markdown
## Skills
- python
- react
- mcp-servers
- testing
```

---

## Workflow

```
/load-feature feat-journeys    # 1. Start: load context + skills
[... do work ...]
/plan                          # 2. Plan new work (if needed)
/tdd                           # 3. Implement with tests
/code-review                   # 4. Review changes
/update-feature feat-journeys  # 5. Sync docs with progress
/checkpoint feat-journeys      # 6. Create handoff for next session
```

### Command Responsibilities

| Command | Reads | Writes |
|---------|-------|--------|
| `/load-feature` | FEAT.md, TRACKING.md, context.md, CLAUDE.md, handoffs/, skills | Nothing |
| `/update-feature` | Session context | FEAT.md, context.md, TRACKING.md |
| `/checkpoint` | Session context | handoffs/YYYY-MM-DD-session.md |

---

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| Instruction-based, not data injection | Matches existing command pattern, avoids duplication |
| Skills declared in FEAT.md | Feature authors decide what's relevant, single source of truth |
| Separate load/update commands | Clear read vs write responsibilities |
| `/update-feature` requires confirmation | User controls what gets written |
| Skills in `blueprints/.claude/skills/` | Centralized, reusable across features |

---

## Files Changed

| File | Action |
|------|--------|
| `blueprints-cc-plugin/commands/load-feature.md` | Created |
| `blueprints-cc-plugin/commands/update-feature.md` | Created |
| `blueprints-cc-plugin/README.md` | Updated with new commands |
| `blueprints/pods/_template/features/_template/FEAT.md` | Added Skills section |
| `blueprints/pods/wip/features/feat-journeys/FEATURE.md` | Added Skills section |
| `blueprints/pods/wip/features/feat-widget-mcp/FEAT.md` | Added Skills section |

---

## Future Considerations

### Not Implemented (Out of Scope)

1. **Automatic skill detection** - Could infer skills from file types in feature, but explicit declaration is clearer
2. **Skill deduplication at runtime** - Not needed since commands are instruction-based
3. **`/context --skills python grpc`** - Ad-hoc skill loading without a feature; could add if needed

### Potential Enhancements

1. **Skill dependencies** - Skills could declare other skills they depend on
2. **Feature templates per tech stack** - `flutter-feature`, `python-feature` with pre-declared skills
3. **Session state persistence** - Track which features/skills were loaded this session

---

## Testing

To verify implementation:

1. **Test `/load-feature`:**
   ```
   /load-feature feat-journeys
   ```
   - Should read 4+ feature files
   - Should parse and read 4 skill files (python, grpc, react, mcp-servers)
   - Should output summary with status, blockers, last session, skills

2. **Test `/update-feature`:**
   ```
   /update-feature feat-journeys
   ```
   - Should analyze session for changes
   - Should propose updates with diff
   - Should wait for confirmation before writing

3. **Test missing feature:**
   ```
   /load-feature feat-nonexistent
   ```
   - Should list available features
   - Should suggest `/new-feature`

4. **Test missing Skills section:**
   - Remove Skills section from a FEAT.md
   - Run `/load-feature`
   - Should warn about missing skills, continue without them

---

## References

- **Command files:** `blueprints-cc-plugin/commands/`
- **Skill files:** `blueprints/.claude/skills/*/SKILL.md`
- **Feature template:** `blueprints/pods/_template/features/_template/`
- **Existing features:** `blueprints/pods/wip/features/`
