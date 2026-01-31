# Skills

The Mindset-specific Claude Code skills are located in:

```
blueprint/.claude/skills/
```

## Available Skills

| Skill | Description |
|-------|-------------|
| **ai-agent** | LangGraph-based AI Agent v3 patterns and node development |
| **flutter** | Flutter/Dart development patterns and best practices |
| **python** | Python development for cloud functions and cloud run containers |
| **react** | React SDK v3 development patterns |
| **grpc** | gRPC and Protocol Buffer patterns |
| **firebase** | Firebase/Firestore patterns and best practices |
| **mcp** | MCP server development and integration |
| **testing** | Test-driven development workflows |
| **architecture** | System architecture and design patterns |
| **deployment** | Cloud Run deployment for MCP servers |
| **snowflake** | Snowflake database setup and best practices |
| **blueprint** | Blueprint workflow, skills, and commands |
| **brand** | Brand voice, copywriting, and terminology |
| **ui** | UI patterns and design system (load with brand) |
| **tinybird** | TinyBird ClickHouse real-time analytics |
| **n8n** | n8n workflow automation patterns for Mindset AI integrations |

## Usage

Load skills using the `/load-skill` command:

```
/load-skill python
/load-skill dart grpc firebase
```

The command supports aliases like `dart` → `flutter`, `proto` → `grpc`, `firestore` → `firebase`.

Skills are also loaded automatically by `/load-feature` when declared in a feature's FEAT.md.
