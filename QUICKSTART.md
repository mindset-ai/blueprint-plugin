# Quick Start Guide

## What Is This?

The Blueprints system is made up of **two repositories** that work together:

### 1. blueprints (the knowledge base)

This is where all the **documentation lives**:

- **Feature documentation** — What each feature does, its current status, blockers, key decisions
- **Session handoffs** — Notes from previous work sessions so the next person can pick up seamlessly
- **Context files** — Living documents that track what's done, what's in progress, what's blocked
- **Skills** — Domain knowledge for each technology stack (Python, Flutter, React, gRPC, etc.)

Think of it as **the team's shared brain** — everything anyone needs to know about any feature, captured in markdown files that survive across sessions and developers.

### 2. blueprints-cc-plugin (the tooling)

This is the **Claude Code plugin** that makes the knowledge base useful:

- **Commands** — Slash commands like `/load-feature`, `/plan`, `/checkpoint` that automate common workflows
- **Agents** — Specialized AI sub-agents for planning, code review, testing, security analysis
- **Rules** — Guidelines that Claude always follows (security, testing, code style, git workflow)

Think of it as **the smart assistant** that knows how to navigate the knowledge base and orchestrate work across your entire stack.

---

## Why Two Repos?

**blueprints** grows constantly — every feature adds docs, every session adds handoffs, every decision gets recorded.

**blueprints-cc-plugin** is relatively stable — the commands and agents don't change often.

Keeping them separate means:
- The plugin can be versioned independently
- Your feature documentation doesn't bloat the tooling repo
- Teams can share the same plugin while having different feature sets

---

## What's In Each Repo?

### blueprints/

```
blueprints/
├── .claude/
│   └── skills/           # Domain knowledge (python, flutter, react, grpc, etc.)
├── pods/                 # Teams organized by pod
│   ├── wip/              # Work-in-progress
│   │   └── features/
│   │       └── feat-journeys/
│   │           ├── FEAT.md        # Feature overview and status
│   │           ├── TRACKING.md    # Git branches across repos
│   │           ├── context.md     # Current state and blockers
│   │           └── handoffs/      # Session summaries
│   ├── ps1/              # Sprint 1
│   ├── ps2/              # Sprint 2
│   └── platform/         # Infrastructure
```

### blueprints-cc-plugin/

```
blueprints-cc-plugin/
├── commands/             # Slash commands (/load-feature, /plan, /tdd, etc.)
├── agents/               # Specialized sub-agents (planner, reviewer, etc.)
├── rules/                # Always-on guidelines (security, testing, git)
├── contexts/             # Work modes (dev, research, review)
└── hooks/                # Pre-tool automations
```

---

## How They Work Together

1. **You run a command** like `/load-feature feat-journeys`

2. **The plugin** (from blueprints-cc-plugin) handles the command

3. **The plugin reads** the feature docs from the blueprints repo

4. **Claude gets context** — all the decisions, blockers, handoffs, and relevant skills

5. **You work** with full awareness of what's been done and what needs doing

6. **At session end**, `/checkpoint` saves your progress back to blueprints for the next person

---

## Getting Started

### VS Code Extension (Recommended for most users)

The VS Code extension requires **manual installation**:

```bash
# 1. Create the directories if they don't exist
mkdir -p ~/.claude/commands ~/.claude/agents ~/.claude/rules

# 2. Copy plugin files
cp /path/to/blueprints-cc-plugin/commands/*.md ~/.claude/commands/
cp /path/to/blueprints-cc-plugin/agents/*.md ~/.claude/agents/
cp /path/to/blueprints-cc-plugin/rules/*.md ~/.claude/rules/

# 3. Add blueprints repo to allowed directories in ~/.claude/settings.json
```

Example `~/.claude/settings.json`:

```json
{
  "allowedDirectories": [
    "/path/to/your/project",
    "/path/to/blueprints-cc-plugin",
    "/path/to/blueprints"
  ]
}
```

After copying the files, reload VS Code (`Cmd+Shift+P` → "Reload Window").

### CLI (`claude` command)

The CLI supports the plugin marketplace system. Add to `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "mindset-claude-code@blueprints-cc-plugin": true
  },
  "extraKnownMarketplaces": {
    "mindset-claude-code": {
      "source": {
        "source": "directory",
        "path": "/path/to/blueprints-cc-plugin"
      }
    }
  }
}
```

---

Once installed, the key commands are:

| Command | Purpose |
|---------|---------|
| `/load-feature {name}` | Start a session — loads all context for a feature |
| `/plan` | Create an implementation plan before coding |
| `/tdd` | Write tests first, then implement |
| `/checkpoint` | End a session — save handoff for next time |
