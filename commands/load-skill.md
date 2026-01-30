---
description: Load one or more skills for the current session. Supports aliases like 'dart' for 'flutter'.
---

# Load Skill Command

Load specific skills without loading a full feature context. Useful when you need domain knowledge for ad-hoc work.

## Usage

```
/load-skill python
/load-skill flutter grpc
/load-skill dart firebase mcp
```

## Skill Aliases

The command supports common aliases that map to the canonical skill names:

| Alias | Maps To | Reason |
|-------|---------|--------|
| `dart` | `flutter` | Dart development uses Flutter skill |
| `getx` | `flutter` | GetX is Flutter state management |
| `firestore` | `firebase` | Firestore is part of Firebase |
| `auth` | `firebase` | Firebase Auth patterns |
| `proto`, `protobuf`, `buf` | `grpc` | Protocol Buffers are part of gRPC skill |
| `langgraph`, `langchain` | `python` | LangGraph/LangChain covered in Python skill |
| `cloud-functions`, `cf` | `python` | Cloud functions are Python |
| `tdd`, `tests` | `testing` | Test-driven development |
| `js`, `javascript`, `typescript`, `ts` | `react` | JS/TS covered in React skill |
| `sdk`, `sdk3`, `sdk-v3` | `react` | React SDK v3 |
| `mcp-servers`, `mcp-server`, `tools` | `mcp` | MCP server development |
| `design`, `system-design` | `architecture` | System architecture |
| `deploy`, `cloud-run`, `cr` | `deployment` | Cloud Run deployment |
| `sf`, `data-warehouse`, `dwh` | `snowflake` | Snowflake database |
| `workflow`, `commands` | `blueprint` | Blueprint workflow system |
| `copywriting`, `copy`, `content`, `writing`, `voice` | `brand` | Brand voice and copywriting |
| `design-system`, `components`, `theming` | `ui` | UI patterns and design system |

## Available Skills

Located at `blueprint/.claude/skills/{skill}/SKILL.md`:

| Skill | Description |
|-------|-------------|
| `python` | Pythonia cloud functions, LangGraph, gRPC services |
| `flutter` | Flutter/Dart, GetX, ReactiveRecord, Admin Framework |
| `react` | React SDK v3 development |
| `grpc` | Protocol Buffers, Buf CLI, service patterns |
| `firebase` | Firestore, authentication, security rules |
| `mcp` | MCP server development and integration |
| `testing` | TDD workflow, test patterns |
| `architecture` | System design, cross-repo patterns |
| `deployment` | Cloud Run deployment for MCP servers |
| `snowflake` | Snowflake database setup and best practices |
| `blueprint` | Blueprint workflow, skills, and commands |
| `brand` | Brand voice, copywriting, and terminology |
| `ui` | UI patterns and design system (load with brand) |

## What This Command Does

1. **Parse requested skills** from arguments
2. **Resolve aliases** to canonical skill names
3. **Deduplicate** (if `dart` and `flutter` both requested, only load `flutter` once)
4. **Read each skill file** from `blueprint/.claude/skills/{skill}/SKILL.md`
5. **Output summary** of loaded skills

## Example

```
User: /load-skill dart grpc

Claude: [Resolves 'dart' → 'flutter']
        [Reads blueprint/.claude/skills/flutter/SKILL.md]
        [Reads blueprint/.claude/skills/grpc/SKILL.md]

# Skills Loaded

- **flutter** (requested as 'dart') - Flutter/Dart, GetX, ReactiveRecord
- **grpc** - Protocol Buffers, Buf CLI, service patterns

Ready to help with Flutter and gRPC development.
```

## Multiple Skills

You can load multiple skills in one command:

```
/load-skill python firebase testing
```

Skills are loaded in the order specified (after alias resolution and deduplication).

## When Skill Not Found

If a skill doesn't exist and has no alias mapping:

1. List available skills
2. Suggest the closest match if obvious
3. Continue loading other valid skills

```
User: /load-skill python rubyx

Claude: 
⚠️ Unknown skill: 'rubyx'
Available skills: python, flutter, react, grpc, firebase, mcp, testing, architecture

Loaded 1 of 2 requested skills:
- **python** - Pythonia cloud functions, LangGraph, gRPC services
```

## Alias Resolution Table

Use this table to resolve aliases before loading:

```javascript
const SKILL_ALIASES = {
  // Flutter/Dart
  'dart': 'flutter',
  'getx': 'flutter',
  
  // Firebase
  'firestore': 'firebase',
  'auth': 'firebase',
  
  // gRPC/Protobuf
  'proto': 'grpc',
  'protobuf': 'grpc',
  'buf': 'grpc',
  
  // Python
  'langgraph': 'python',
  'langchain': 'python',
  'cloud-functions': 'python',
  'cf': 'python',
  
  // Testing
  'tdd': 'testing',
  'tests': 'testing',
  
  // React/JS
  'js': 'react',
  'javascript': 'react',
  'typescript': 'react',
  'ts': 'react',
  'sdk': 'react',
  'sdk3': 'react',
  'sdk-v3': 'react',
  
  // MCP
  'mcp-servers': 'mcp',
  'mcp-server': 'mcp',
  'tools': 'mcp',
  
  // Architecture
  'design': 'architecture',
  'system-design': 'architecture',

  // Deployment
  'deploy': 'deployment',
  'cloud-run': 'deployment',
  'cr': 'deployment',

  // Snowflake
  'sf': 'snowflake',
  'data-warehouse': 'snowflake',
  'dwh': 'snowflake',

  // Blueprint
  'workflow': 'blueprint',
  'commands': 'blueprint',

  // Brand
  'copywriting': 'brand',
  'copy': 'brand',
  'content': 'brand',
  'writing': 'brand',
  'voice': 'brand',

  // UI
  'design-system': 'ui',
  'components': 'ui',
  'theming': 'ui',
};
```

## Integration with /load-feature

- `/load-feature` automatically loads skills declared in the feature's FEAT.md
- `/load-skill` is for loading skills independently, without a feature context
- Both commands read from the same skill files
- Skills loaded by either command remain in conversation history

## Notes

- Skills are read once and remain in conversation context
- Loading the same skill twice has no effect (already in history)
- Use this command for ad-hoc work not tied to a specific feature
- For feature work, prefer `/load-feature` which loads skills automatically
