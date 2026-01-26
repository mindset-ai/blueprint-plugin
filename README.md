# Mindset AI - Claude Code Configuration

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Shell](https://img.shields.io/badge/-Shell-4EAA25?logo=gnu-bash&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white)
![Markdown](https://img.shields.io/badge/-Markdown-000000?logo=markdown&logoColor=white)

**Claude Code configuration for the Mindset AI platform.**

This repository contains production-ready agents, hooks, commands, rules, and MCP configurations tailored for the Mindset AI development workflow.

> **Attribution:** This repository is forked from [everything-claude-code](https://github.com/affaan-m/everything-claude-code) by [Affaan Mustafa](https://x.com/affaanmustafa) ([@affaan-m](https://github.com/affaan-m)), an excellent collection of Claude Code configs developed over 10+ months of intensive daily use. Affaan won the Anthropic x Forum Ventures hackathon building with Claude Code. We have adapted his work for the Mindset AI platform while preserving the valuable patterns and documentation from the original. Check out his guides:
> - [The Shorthand Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2012378465664745795)
> - [The Longform Guide to Everything Claude Code](https://x.com/affaanmustafa/status/2014040193557471352)

---

## Integration with Blueprints

This repository works in conjunction with the **blueprints** repository, which is the central hub for Mindset AI's epic planning and execution documentation.

### Repository Structure

```
dev/active/
|-- blueprints/                    # Epic planning and documentation
|   |-- .claude/
|   |   |-- skills/                # Skills live HERE (not in this repo)
|   |   |   |-- flutter/           # Flutter/Dart patterns
|   |   |   |-- python/            # Python/LangGraph patterns
|   |   |   |-- react/             # React SDK patterns
|   |   |   |-- grpc/              # Protocol buffer patterns
|   |   |   |-- firebase/          # Firebase/Firestore patterns
|   |   |   |-- mcp-servers/       # MCP server patterns
|   |   |   |-- testing/           # TDD workflow patterns
|   |   |   |-- architecture/      # Platform architecture
|   |-- pods/                      # Pod-based feature organization
|   |   |-- wip/                   # Work in progress pod
|   |   |   |-- features/
|   |   |   |   |-- feat-journeys/
|   |   |   |   |-- feat-widget-mcp/
|   |   |-- [other-pod]/
|   |   |   |-- features/
|   |   |   |   |-- feat-*/
|   |-- planning/
|   |   |-- domains/
|   |   |   |-- 00-technology-landscape.md  # Required reading
|   |   |-- epics/                 # Epic documentation
|
|-- blueprints-claude-code/        # THIS REPO - Claude Code configuration
|   |-- agents/                    # Specialized subagents
|   |-- commands/                  # Slash commands
|   |-- rules/                     # Always-follow guidelines
|   |-- hooks/                     # Trigger-based automations
|   |-- scripts/                   # Cross-platform Node.js scripts
|
|-- mindset_v2/                    # Main Flutter platform
|-- pythonia/                      # Python cloud functions
|-- protos/                        # Protocol buffer definitions
```

### Why Skills Live in Blueprints

Skills are stored in `blueprints/.claude/skills/` because:

1. **Workflow Integration** - Skills like `/ms-start-story` need access to epic plans and story documentation stored in blueprints
2. **Shared Context** - Skills reference the technology landscape and architectural decisions documented in blueprints
3. **Single Source of Truth** - Keeping workflow definitions with the planning documentation ensures consistency
4. **Cross-Project Access** - Skills in blueprints can be invoked from any Mindset project directory

---

## What's Inside This Repository

This repo contains the **configuration layer** for Claude Code - agents, commands, rules, and hooks that define how Claude behaves when working on Mindset projects.

```
blueprints-claude-code/
|-- .claude-plugin/   # Plugin and marketplace manifests
|   |-- plugin.json         # Plugin metadata and component paths
|   |-- marketplace.json    # Marketplace catalog
|
|-- agents/           # Specialized subagents for delegation
|   |-- planner.md           # Feature implementation planning
|   |-- architect.md         # System design decisions
|   |-- tdd-guide.md         # Test-driven development
|   |-- code-reviewer.md     # Quality and security review
|   |-- security-reviewer.md # Vulnerability analysis
|   |-- build-error-resolver.md
|   |-- e2e-runner.md        # E2E testing
|   |-- refactor-cleaner.md  # Dead code cleanup
|   |-- doc-updater.md       # Documentation sync
|
|-- commands/         # Slash commands for quick execution
|   |-- tdd.md              # /tdd - Test-driven development
|   |-- plan.md             # /plan - Implementation planning
|   |-- e2e.md              # /e2e - E2E test generation
|   |-- code-review.md      # /code-review - Quality review
|   |-- build-fix.md        # /build-fix - Fix build errors
|   |-- refactor-clean.md   # /refactor-clean - Dead code removal
|   |-- learn.md            # /learn - Extract patterns mid-session
|   |-- checkpoint.md       # /checkpoint - Save verification state
|   |-- verify.md           # /verify - Run verification loop
|   |-- setup-pm.md         # /setup-pm - Configure package manager
|
|-- rules/            # Always-follow guidelines (copy to ~/.claude/rules/)
|   |-- security.md         # Mandatory security checks
|   |-- coding-style.md     # Immutability, file organization
|   |-- testing.md          # TDD, coverage requirements
|   |-- git-workflow.md     # Commit format, PR process
|   |-- agents.md           # When to delegate to subagents
|   |-- performance.md      # Model selection, context management
|
|-- hooks/            # Trigger-based automations
|   |-- hooks.json                # All hooks config
|   |-- memory-persistence/       # Session lifecycle hooks
|   |-- strategic-compact/        # Compaction suggestions
|
|-- scripts/          # Cross-platform Node.js scripts
|   |-- lib/                     # Shared utilities
|   |   |-- utils.js             # Cross-platform file/path/system utilities
|   |   |-- package-manager.js   # Package manager detection
|   |-- hooks/                   # Hook implementations
|   |-- setup-package-manager.js # Interactive PM setup
|
|-- tests/            # Test suite
|   |-- lib/                     # Library tests
|   |-- hooks/                   # Hook tests
|   |-- run-all.js               # Run all tests
|
|-- contexts/         # Dynamic system prompt injection contexts
|   |-- dev.md              # Development mode context
|   |-- review.md           # Code review mode context
|   |-- research.md         # Research/exploration mode context
|
|-- examples/         # Example configurations
|   |-- CLAUDE.md           # Example project-level config
|   |-- user-CLAUDE.md      # Example user-level config
|
|-- mcp-configs/      # MCP server configurations
|   |-- mcp-servers.json    # GitHub, Firebase, GCP, etc.
```

---

## Cross-Platform Support

This configuration fully supports **Windows, macOS, and Linux**. All hooks and scripts have been rewritten in Node.js for maximum compatibility.

### Package Manager Detection

The plugin automatically detects your preferred package manager (npm, pnpm, yarn, or bun) with the following priority:

1. **Environment variable**: `CLAUDE_PACKAGE_MANAGER`
2. **Project config**: `.claude/package-manager.json`
3. **package.json**: `packageManager` field
4. **Lock file**: Detection from lock files
5. **Global config**: `~/.claude/package-manager.json`
6. **Fallback**: First available package manager

To set your preferred package manager:

```bash
# Via environment variable
export CLAUDE_PACKAGE_MANAGER=pnpm

# Via global config
node scripts/setup-package-manager.js --global pnpm

# Via project config
node scripts/setup-package-manager.js --project bun

# Detect current setting
node scripts/setup-package-manager.js --detect
```

---

## Installation

### Option 1: Install as Plugin (Recommended)

Add directly to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "mindset-claude-code": {
      "source": {
        "source": "local",
        "path": "/Users/your-username/dev/active/blueprints-claude-code"
      }
    }
  },
  "enabledPlugins": {
    "mindset-claude-code@blueprints-claude-code": true
  }
}
```

This gives you instant access to all commands, agents, and hooks.

---

### Option 2: Manual Installation

If you prefer manual control over what's installed:

```bash
# Copy agents to your Claude config
cp blueprints-claude-code/agents/*.md ~/.claude/agents/

# Copy rules
cp blueprints-claude-code/rules/*.md ~/.claude/rules/

# Copy commands
cp blueprints-claude-code/commands/*.md ~/.claude/commands/
```

#### Add hooks to settings.json

Copy the hooks from `hooks/hooks.json` to your `~/.claude/settings.json`.

#### Configure MCPs

Copy desired MCP servers from `mcp-configs/mcp-servers.json` to your `~/.claude.json`.

**Important:** Replace placeholder values with your actual API keys and credentials.

---

## Key Concepts

### Agents

Subagents handle delegated tasks with limited scope. Example:

```markdown
---
name: code-reviewer
description: Reviews code for quality, security, and maintainability
tools: Read, Grep, Glob, Bash
model: opus
---

You are a senior code reviewer...
```

For Mindset development, agents are particularly useful for:
- **Flutter code review** - Ensuring GetX patterns and widget best practices
- **Python backend review** - Validating LangGraph agent implementations
- **Proto file validation** - Checking gRPC interface consistency

### Skills (in blueprints/.claude/skills/)

Skills are workflow definitions that live in the blueprints repository:

| Skill | Purpose |
|-------|---------|
| `flutter` | Flutter/Dart development patterns, GetX, melos |
| `python` | Python cloud functions, LangGraph agents |
| `react` | React SDK v3 patterns, TypeScript |
| `grpc` | Protocol buffers, buf CLI workflow |
| `firebase` | Firestore patterns, security rules |
| `mcp-servers` | MCP server development |
| `testing` | TDD workflow, coverage requirements |
| `architecture` | Platform architecture overview |

Skills provide domain-specific patterns and best practices for each technology in the Mindset stack. They are referenced by commands and agents when reviewing or writing code.

### Command Comparison: blueprints-claude-code vs mindset_v2

The mindset_v2 project has its own `/ms-*` commands that provide GitLab integration and security workflows specific to that project. This section clarifies which commands to use and when.

#### Planning Commands

| Aspect | `/plan` (this repo) | `/ms-story-plan` (mindset_v2) |
|--------|---------------------|-------------------------------|
| **Scope** | Any project | mindset_v2 only |
| **Git ops** | None | Creates branch from develop |
| **Ticket integration** | None | GitLab via glab (fetches issue details) |
| **Security** | Risk assessment only | **Required** classification (Standard/Security-Aware/Security-Critical) |
| **Artifacts** | Updates `blueprints/` context.md | Creates `llm/{story}/` folder with requirements + plan |
| **Best for** | General feature planning | Starting a GitLab-tracked story |

**Usage:**
```bash
# General planning (any project)
/plan I need to add a new tool to the AI agent

# mindset_v2 story (with GitLab automation)
/ms-story-plan gitlab-issue-url="https://gitlab.com/.../issues/1234"

# mindset_v2 story (plan only, requirements exist)
/ms-story-plan story-number=1234 requirements-doc-path-or-reference="llm/1234/1234-requirements.md"
```

#### TDD Commands

| Aspect | `/tdd` (this repo) | `/ms-tdd-fix` (mindset_v2) |
|--------|--------------------|-----------------------------|
| **Scope** | Any project (Flutter, Python, React) | mindset_v2 only |
| **Focus** | Full TDD workflow (new features + bugs) | Bug fixes only |
| **Coverage** | Enforces 80%+ coverage | Regression test focus |
| **Examples** | Includes Flutter GetX and Python LangGraph examples | Flutter/Dart patterns |
| **Best for** | New feature development with TDD | Quick bug fix with test |

**Usage:**
```bash
# Full TDD for new feature (any project)
/tdd I need a controller to track widget engagement metrics

# Bug fix with regression test (mindset_v2)
/ms-tdd-fix bug-description="Button doesn't disable when form is invalid"
```

#### Code Review Commands

| Aspect | `/code-review` (this repo) | `/ms-security-review` (mindset_v2) |
|--------|----------------------------|------------------------------------|
| **Scope** | Any project | mindset_v2 only |
| **Focus** | Quality, security, platform patterns | Pre-deployment security verification |
| **When** | After writing code | Before MR (Security-Aware/Critical only) |
| **Checks** | GetX, LangGraph, React patterns, gRPC compat | Security controls, Secret Manager, vulnerability scan |
| **Best for** | General code quality review | Security gate before deployment |

**Usage:**
```bash
# General code review (any project)
/code-review

# Security verification before MR (mindset_v2, Security-Aware/Critical)
/ms-security-review story-number=653 classification="Security-Aware"
```

#### mindset_v2-Only Commands (No Equivalent in This Repo)

These commands are specific to mindset_v2's GitLab workflow and security classification system:

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/ms-classify-story` | Determine security classification | Optional - before planning (auto-done by `/ms-story-plan`) |
| `/ms-implement-secure` | Implement with security practices | During development, after plan approval |
| `/ms-security-critical-checklist` | Create security checklist | Security-Critical stories only (5%), after CTO approval |
| `/ms-create-mr` | Create GitLab merge request | After development complete, creates MR with glab |

**Usage:**
```bash
# Classify a story (optional - ms-story-plan does this automatically)
/ms-classify-story story-number=653 gitlab-issue-url="..."

# Implement with security practices
/ms-implement-secure story-number=653 classification="Security-Aware"

# Create security checklist (Security-Critical only)
/ms-security-critical-checklist story-number=655

# Create merge request
/ms-create-mr story-number=653
```

#### Decision Guide: Which Command to Use?

```
Are you working on mindset_v2 with a GitLab issue?
├─ YES → Use /ms-* commands
│   ├─ Starting new story? → /ms-story-plan
│   ├─ Implementing? → /ms-implement-secure
│   ├─ Fixing a bug? → /ms-tdd-fix
│   ├─ Security review needed? → /ms-security-review
│   └─ Ready for MR? → /ms-create-mr
│
└─ NO → Use blueprints-claude-code commands
    ├─ Planning a feature? → /plan
    ├─ Writing new code with TDD? → /tdd
    ├─ Reviewing code? → /code-review
    ├─ Build errors? → /build-fix
    └─ E2E tests needed? → /e2e
```

#### Typical Workflows

**mindset_v2 Story (Full Workflow):**
```bash
/ms-story-plan gitlab-issue-url="https://gitlab.com/.../issues/653"
# Review requirements and plan, approve when ready
/ms-implement-secure story-number=653 classification="Security-Aware"
/ms-security-review story-number=653 classification="Security-Aware"
/ms-create-mr story-number=653
```

**General Feature (Any Project):**
```bash
/plan I need to add knowledge base search to the AI agent
# Review plan, approve when ready
/tdd  # Implement with TDD
/code-review  # Review before commit
/build-fix  # If build errors occur
```

### Hooks

Hooks fire on tool events. Example - warn about debug statements in Flutter:

```json
{
  "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\\\.dart$\"",
  "hooks": [{
    "type": "command",
    "command": "#!/bin/bash\ngrep -n 'print(' \"$file_path\" && echo '[Hook] Remove debug print statements' >&2"
  }]
}
```

### Rules

Rules are always-follow guidelines. Keep them modular:

```
~/.claude/rules/
  security.md      # No hardcoded secrets
  coding-style.md  # Immutability, file limits
  testing.md       # TDD, coverage requirements
```

For Mindset projects, key rules include:
- **Never auto-commit** - Let developers review and commit changes themselves
- **Test-first bug fixing** - Write failing test before fixing bugs
- **Run dcm analyze** - Dart Code Metrics after Flutter edits

---

## Mindset Platform Integration

### CLAUDE.md Files

Each Mindset project has its own CLAUDE.md with detailed guidance:

| Project | CLAUDE.md Location | Focus |
|---------|-------------------|-------|
| **mindset_v2** | `mindset_v2/CLAUDE.md` | Flutter, Melos, GetX patterns |
| **pythonia** | `pythonia/CLAUDE.md` | Python, LangGraph, gRPC |
| **protos** | `protos/CLAUDE.md` | Buf, Protocol Buffers |
| **blueprints** | `blueprints/.claude/CLAUDE.md` | Epic planning, ecosystem overview |
| **sdk2-docs** | `sdk2-docs/CLAUDE.md` | Mintlify documentation |

### Local Development Services

Common local development setup for Mindset:

```bash
# Python AI Agent (port 50051)
cd pythonia/cloud-functions/monkee.ai_CR_aiagent_v3
./venv/bin/python3 main.py

# Envoy Proxy (port 8080) - gRPC-Web translation
docker start envoy-aiagent-local

# Flutter web in release mode
cd mindset_v2
melos run run:ams_web_release_mode
```

Use the `?envoy=http://localhost:8080` URL parameter to redirect frontend traffic to local services.

---

## Running Tests

The plugin includes a comprehensive test suite:

```bash
# Run all tests
node tests/run-all.js

# Run individual test files
node tests/lib/utils.test.js
node tests/lib/package-manager.test.js
node tests/hooks/hooks.test.js
```

---

## Important Notes

### Context Window Management

**Critical:** Don't enable all MCPs at once. Your 200k context window can shrink significantly with too many tools enabled.

Rule of thumb:
- Have 20-30 MCPs configured
- Keep under 10 enabled per project
- Under 80 tools active

Use `disabledMcpServers` in project config to disable unused ones.

### Customization

These configs are adapted for the Mindset AI platform. You should:
1. Start with what's relevant to your current project
2. Modify for your specific needs
3. Remove what you don't use
4. Contribute improvements back

---

## Original Resource Guides

The original repository includes excellent documentation on advanced topics:

| Topic | What You'll Learn |
|-------|-------------------|
| Token Optimization | Model selection, system prompt slimming, background processes |
| Memory Persistence | Hooks that save/load context across sessions automatically |
| Continuous Learning | Auto-extract patterns from sessions into reusable skills |
| Verification Loops | Checkpoint vs continuous evals, grader types, pass@k metrics |
| Parallelization | Git worktrees, cascade method, when to scale instances |
| Subagent Orchestration | The context problem, iterative retrieval pattern |

See the [original repository](https://github.com/affaan-m/everything-claude-code) for the complete guides.

---

## Contributing

Contributions that improve Mindset AI development workflows are welcome. If you have:
- Useful agents or skills for Flutter/Python development
- Clever hooks for the Mindset codebase
- Better MCP configurations for Firebase/GCP
- Improved rules for our tech stack

Please contribute!

### Ideas for Contributions

- Flutter-specific skills (GetX patterns, widget testing)
- Python backend agents (LangGraph, gRPC patterns)
- Firebase/Firestore workflow automation
- Proto file validation and generation
- Documentation sync between code and sdk2-docs

---

## License

MIT - Use freely, modify as needed, contribute back if you can.

---

## Links

- **Original Repository:** [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- **Blueprints Repository:** Internal Mindset AI planning documentation
- **Technology Landscape:** `blueprints/planning/domains/00-technology-landscape.md`
