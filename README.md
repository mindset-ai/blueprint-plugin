# blueprints-cc-plugin

**Claude Code plugin for the Mindset AI platform.**

Commands, agents, and rules to accelerate development across Flutter, Python, and React codebases.

---

## Quick Start

Add to your `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "mindset-claude-code@blueprints-cc-plugin": true
  },
  "extraKnownMarketplaces": {
    "mindset-claude-code": {
      "source": { "source": "local", "path": "/path/to/blueprints-cc-plugin" }
    }
  }
}
```

---

## Commands

### Planning & Development

| Command | What it does |
|---------|--------------|
| `/new-feature` | Create new feature folder with FEAT.md and TRACKING.md from templates. |
| `/plan` | Create implementation plan. Reads feature context, breaks down into phases, waits for approval before coding. |
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
| `/checkpoint` | Save session state to blueprints. Creates handoff document with context for next session. |
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
0. SETUP      →  /new-feature       Create feature folder from templates
1. PLAN       →  /plan              Create implementation plan, get approval
2. IMPLEMENT  →  /tdd               Write tests first, then code
3. BUILD      →  /build-fix         Fix any build/type errors
4. REVIEW     →  /code-review       Check quality and security
5. VERIFY     →  /verify            Run full test suite
6. HANDOFF    →  /checkpoint        Save context for next session
```

### Typical Flow

**Creating a new feature:**
```
/new-feature feat-knowledge-search
```
Claude creates the folder structure with FEAT.md and TRACKING.md from templates.

**Planning work:**
```
/plan feat-widget-mcp: add retry logic to failed requests
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
/checkpoint
```
Claude saves current state to blueprints for seamless handoff.

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

## mindset_v2 Commands (GitLab Integration)

The mindset_v2 project has additional `/ms-*` commands with GitLab integration:

| Command | What it does |
|---------|--------------|
| `/ms-story-plan` | Full story setup. Fetches GitLab issue, creates branch, requirements doc, and implementation plan with security classification. |
| `/ms-implement-secure` | Implement with security practices based on classification (Standard/Security-Aware/Security-Critical). |
| `/ms-tdd-fix` | TDD bug fix. Write failing test first, then fix. |
| `/ms-security-review` | Pre-deployment security verification for Security-Aware/Critical stories. |
| `/ms-create-mr` | Create GitLab MR with proper description, reviewers, and labels. |
| `/ms-classify-story` | Determine security classification (optional - ms-story-plan does this automatically). |

### Which to use?

```
Working on mindset_v2 with a GitLab issue?
├─ YES → Use /ms-* commands
│   ├─ Starting story? → /ms-story-plan
│   ├─ Implementing? → /ms-implement-secure  
│   ├─ Bug fix? → /ms-tdd-fix
│   └─ Ready for MR? → /ms-create-mr
│
└─ NO → Use plugin commands
    ├─ Planning? → /plan
    ├─ New code? → /tdd
    ├─ Review? → /code-review
    └─ Build errors? → /build-fix
```

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

## Feature Structure (Blueprints)

Features in the blueprints repo use 2 files:

```
blueprints/pods/{pod}/features/{feature}/
├── FEAT.md         # THE living document - status, blockers, patterns, local dev
├── TRACKING.md     # Cross-repo branches, deployment order, files changed
└── archive/        # Historical docs
```

`/plan` reads FEAT.md and TRACKING.md, then updates "What's Next" with the approved plan.

---

## Installation Options

### Plugin (Recommended)

```json
{
  "enabledPlugins": {
    "mindset-claude-code@blueprints-cc-plugin": true
  },
  "extraKnownMarketplaces": {
    "mindset-claude-code": {
      "source": { "source": "local", "path": "/path/to/blueprints-cc-plugin" }
    }
  }
}
```

### Manual

```bash
cp blueprints-cc-plugin/agents/*.md ~/.claude/agents/
cp blueprints-cc-plugin/rules/*.md ~/.claude/rules/
cp blueprints-cc-plugin/commands/*.md ~/.claude/commands/
```

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
