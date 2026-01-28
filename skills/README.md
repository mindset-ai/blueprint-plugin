# Skills

The Mindset-specific Claude Code skills are located in:

```
blueprints/.claude/skills/
```

## Available Skills

| Skill | Description |
|-------|-------------|
| **flutter** | Flutter/Dart development patterns and best practices |
| **python** | Python development for cloud functions and AI agents |
| **react** | React SDK v3 development patterns |
| **grpc** | gRPC and Protocol Buffer patterns |
| **firebase** | Firebase/Firestore patterns and best practices |
| **mcp** | MCP server development and integration |
| **testing** | Test-driven development workflows |
| **architecture** | System architecture and design patterns |

## Usage

Load skills using the `/load-skill` command:

```
/load-skill python
/load-skill dart grpc firebase
```

The command supports aliases like `dart` → `flutter`, `proto` → `grpc`, `firestore` → `firebase`.

Skills are also loaded automatically by `/load-feature` when declared in a feature's FEAT.md.
