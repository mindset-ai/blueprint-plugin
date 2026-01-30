# blueprint-plugin

## A Developer Force-Multiplier System

> **One Developer + Claude Code + Blueprint = Full-Stack Development Team**

This plugin transforms Claude Code from a coding assistant into an **intelligent development orchestrator**. It enables each developer to effectively manage a team of AI-driven specialists — planners, architects, testers, reviewers, and implementers — working in harmony across your entire platform stack.

**The result:** Every developer becomes a full-stack developer, capable of shipping features across Flutter, Python, React, and gRPC with the velocity of an entire team.

## Watch the Video

[![Blueprint Plugin Overview](https://img.youtube.com/vi/RGXmYNv1KT4/maxresdefault.jpg)](https://youtu.be/RGXmYNv1KT4)

---

**Claude Code plugin for the Mindset AI platform.**

Commands, agents, and rules to accelerate development across Flutter, Python, and React codebases.

---

## Quick Start

**New here?** See the full [QUICKSTART.md](QUICKSTART.md) guide for step-by-step setup.

**Already have the repos cloned?** Add to your `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "mindset-claude-code@blueprint-plugin": true
  },
  "extraKnownMarketplaces": {
    "mindset-claude-code": {
      "source": {
        "source": "directory",
        "path": "/path/to/blueprint-plugin"
      }
    }
  }
}
```

---

## Commands

### Epic Context (Product)

| Command | What it does |
|---------|--------------|
| `/load-epic` | Load epic context. Reads EPIC.md, lists research docs, finds linked features across pods. |
| `/new-epic` | Create new epic folder with EPIC.md template in the `pm` pod. |
| `/update-epic` | Update epic docs with decisions, requirement changes, feature status. |

### Feature Context (Start Here)

| Command | What it does |
|---------|--------------|
| `/load-feature` | **Start of session.** Load feature context and skills. Reads FEAT.md, TRACKING.md, context.md, latest handoff, declared skills, and parent epic if linked. |
| `/load-skill` | Load skills without a feature. Supports aliases (e.g., `dart` → `flutter`, `proto` → `grpc`). |
| `/update-feature` | **During/end of session.** Update feature docs with progress, new blockers, decisions made. |
| `/new-feature` | Create new feature folder with FEAT.md and TRACKING.md from templates. Supports `--epic` to link to an epic. |

### Planning & Development

| Command | What it does |
|---------|--------------|
| `/plan-feature` | Create implementation plan. Reads feature context, breaks down into phases, waits for approval before coding. |
| `/tdd` | Test-driven development. Write failing tests first, implement minimal code to pass, refactor. Enforces 80%+ coverage. |
| `/build-fix` | Fix build errors. Detects project type (Flutter/Python/React), runs build, fixes errors iteratively. |
| `/code-review` | Review uncommitted changes. Checks security, patterns (GetX, LangGraph, React), gRPC compatibility. Blocks on critical issues. |

### Testing & Verification

| Command | What it does |
|---------|--------------|
| `/test-coverage` | Analyze test coverage. Identifies untested code paths, suggests tests to add. |
| `/verify` | Run verification loop. Executes tests, checks build, validates changes meet requirements. |

### Code Quality

| Command | What it does |
|---------|--------------|
| `/refactor-clean` | Remove dead code. Runs analysis tools (knip, depcheck, ts-prune), safely removes unused code. |
| `/learn` | Extract patterns mid-session. Identifies reusable patterns from current work, saves to skills. |

### Documentation & Handoff

| Command | What it does |
|---------|--------------|
| `/checkpoint` | Save session state to blueprint. Creates handoff document with context for next session. |
| `/update-docs` | Sync documentation. Updates READMEs, API docs, and guides based on code changes. |
| `/update-codemaps` | Regenerate codemaps. Updates `docs/CODEMAPS/*` with current codebase structure. |

### Other

| Command | What it does |
|---------|--------------|
| `/setup-pm` | Configure package manager. Sets npm/pnpm/yarn/bun preference for the project. |
| `/orchestrate` | Run multiple agents. Coordinates parallel work across different parts of codebase. |
| `/eval` | Run evaluation. Tests agent responses against expected outputs. |
| `/e2e` | Generate and run Playwright E2E tests. Creates test journeys, captures screenshots/videos/traces. |

---

## Workflow

How to use these commands together:

```
0. EPIC       →  /load-epic          Load epic context (product requirements)
0. CONTEXT    →  /load-feature      Load feature context and skills (START HERE)
1. PLAN       →  /plan-feature       Create implementation plan, get approval
2. IMPLEMENT  →  /tdd               Write tests first, then code
3. BUILD      →  /build-fix         Fix any build/type errors
4. REVIEW     →  /code-review       Check quality and security
5. VERIFY     →  /verify            Run full test suite
6. UPDATE     →  /update-feature    Sync docs with progress, decisions, blockers
7. HANDOFF    →  /checkpoint        Save context for next session
```

### Typical Flow

**Starting a session (existing feature):**
```
/load-feature feat-widget-mcp
```
Claude reads all feature docs, latest handoff, and declared skills. You're ready to work.

**Creating a new feature:**
```
/new-feature feat-knowledge-search
```
Claude creates the folder structure with FEAT.md and TRACKING.md from templates.

**Planning work:**
```
/plan-feature feat-widget-mcp: add retry logic to failed requests
```
Claude reads FEAT.md + TRACKING.md, creates phased plan, waits for your approval.

**Implementing:**
```
/tdd
```
Claude writes failing tests for the feature, then implements minimal code to pass.

**Before committing:**
```
/code-review
```
Claude reviews all uncommitted changes for security, patterns, and quality.

**Build broken:**
```
/build-fix
```
Claude detects project type, runs build, fixes errors iteratively until green.

**End of session:**
```
/update-feature feat-widget-mcp
/checkpoint feat-widget-mcp
```
Claude updates feature docs with progress, then creates handoff for next session.

---

## Agents

Specialized subagents that handle delegated tasks:

| Agent | When Claude uses it |
|-------|---------------------|
| `planner` | Planning features, breaking down complex tasks |
| `architect` | System design decisions, cross-repo architecture |
| `tdd-guide` | Enforcing test-driven development workflow |
| `code-reviewer` | Reviewing code for quality and security |
| `security-reviewer` | Deep security analysis, vulnerability detection |
| `build-error-resolver` | Fixing build/type/lint errors |
| `e2e-runner` | Generating and running E2E tests |
| `refactor-cleaner` | Finding and removing dead code |
| `doc-updater` | Updating documentation and codemaps |

---

## Rules

Always-follow guidelines loaded into every session:

| Rule | What it enforces |
|------|------------------|
| `security.md` | No hardcoded secrets, input validation, auth checks |
| `coding-style.md` | Immutability, file size limits, naming conventions |
| `testing.md` | TDD workflow, 80%+ coverage requirement |
| `git-workflow.md` | Never auto-commit, let user review changes |
| `agents.md` | When to delegate to specialized subagents |
| `performance.md` | Model selection (haiku for simple, opus for complex) |

---

## Epic & Feature Structure (Blueprint)

### Epics (Product owns - `pm` pod)

```
blueprint/pods/pm/epics/{epic-name}/
├── EPIC.md         # Requirements, user stories, scope, success metrics
└── research/       # Research documents and analysis
```

### Features (Engineering owns - pod-specific)

```
blueprint/pods/{pod}/features/{feature}/
├── FEAT.md         # THE living document - status, epic link, blockers, patterns, skills, local dev
├── TRACKING.md     # Cross-repo branches, deployment order, files changed
├── context.md      # Current state, session log
├── CLAUDE.md       # Feature-specific instructions
├── handoffs/       # Session summaries
└── archive/        # Historical docs
```

Features link to their parent epic via the `Epic:` field in FEAT.md. `/load-feature` automatically loads the parent epic context.

**Skills Declaration** in FEAT.md:
```markdown
## Skills
<!-- Loaded automatically by /load-feature -->
- python
- grpc
- react
```

`/load-feature` reads all docs and declared skills. `/plan-feature` creates implementation plans. `/update-feature` syncs docs with progress.

---

## Installation

See the [QUICKSTART.md](QUICKSTART.md#getting-started) guide for step-by-step installation instructions for VS Code and CLI.

---

## Local Development Services

```bash
# Python AI Agent (port 50051)
cd pythonia/cloud-functions/monkee.ai_CR_aiagent_v3
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json
./venv/bin/python3 main.py

# Envoy Proxy (port 8080)
docker start envoy-aiagent-local

# Flutter web
cd mindset_v2
melos run run:ams_web_release_mode
```

Test with: `?envoy=http://localhost:8080`

---

## Attribution

Forked from [everything-claude-code](https://github.com/affaan-m/everything-claude-code) by [Affaan Mustafa](https://x.com/affaanmustafa). Adapted for Mindset AI platform.

---

## License

MIT
