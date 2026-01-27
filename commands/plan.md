---
description: Restate requirements, assess risks, and create step-by-step implementation plan. WAIT for user CONFIRM before touching any code.
---

# Plan Command

This command invokes the **planner** agent to create a comprehensive implementation plan before writing any code.

## What This Command Does

1. **Restate Requirements** - Clarify what needs to be built
2. **Read Feature Context** - Load context from `blueprints/pods/[pod-name]/features/[feature-name]/FEAT.md`
3. **Identify Risks** - Surface potential issues and blockers
4. **Create Step Plan** - Break down implementation into phases
5. **Update Feature** - Add the plan to the feature's "What's Next" section
6. **Wait for Confirmation** - MUST receive user approval before proceeding

## Integration with Mindset Blueprints

Features use a simplified 2-file structure:

```
blueprints/pods/[pod-name]/features/[feature-name]/
├── FEAT.md         # THE living document - everything in one place
├── TRACKING.md     # Cross-repo branches, deployment order, files changed
└── archive/        # Historical docs (analysis, old plans, reviews)
```

When creating a plan:

1. **Read FEAT.md** to understand:
   - Current state: what's working, blockers, decisions
   - Patterns to follow and common issues
   - File locations across repositories
   - Existing "What's Next" items

2. **Check TRACKING.md** for:
   - Active branches per repository
   - Deployment order and dependencies
   - Files already changed

3. **Update the "What's Next" section** in `FEAT.md` with the approved plan

## When to Use

Use `/plan` when:
- Starting a new feature or epic
- Adding capabilities to AI agents
- Creating new widget types or components
- Making significant architectural changes
- Working on complex refactoring across repos
- Requirements are unclear or ambiguous

## How It Works

The planner agent will:

1. **Analyze the request** and restate requirements in clear terms
2. **Read feature context** from blueprints if a feature is specified
3. **Break down into phases** with specific, actionable steps
4. **Identify dependencies** between components (Python backend, Flutter frontend, protos)
5. **Assess risks** and potential blockers
6. **Estimate complexity** (High/Medium/Low)
7. **Present the plan** and WAIT for your explicit confirmation
8. **Update context.md** with the approved plan in "What's Next" section

## Example Usage

```
User: /plan I need to add a new tool to the AI agent that allows it to search knowledge bases

Agent (planner):
# Implementation Plan: Knowledge Base Search Tool for AI Agent

## Feature Context
Reading from: blueprints/pods/wip/features/feat-widget-mcp/FEAT.md
- Related work: Widget creator MCP already implements tool patterns
- Key decision: Tools use gRPC for communication

## Requirements Restatement
- Add a new tool to the AI agent that searches knowledge bases
- Tool should accept search queries and return relevant content chunks
- Support filtering by knowledge base ID and content type
- Integrate with existing LangGraph tool execution flow

## Implementation Phases

### Phase 1: Proto Definitions (protos/)
- Define KnowledgeSearchRequest message with query, kb_id, filters
- Define KnowledgeSearchResponse with results array and metadata
- Add search method to AIAgent service definition
- Run `buf lint` and `buf generate` to update generated code

### Phase 2: Python Backend (pythonia/)
- Create knowledge_search_tool.py in cloud-functions/monkee.ai_CR_aiagent_v3/tools/
- Implement FirestoreKnowledgeSearcher class
- Add vector similarity search using existing embeddings
- Register tool in LangGraph agent configuration
- Add unit tests for search logic

### Phase 3: Integration
- Update agent tool registry to include knowledge search
- Add tool configuration in agent settings (Firestore)
- Implement rate limiting and quota checks
- Add telemetry/logging for tool usage

### Phase 4: Testing & Documentation
- Add integration tests with test knowledge base
- Update sdk2-docs with tool documentation
- Test via AMS admin interface

## Dependencies
- Existing embedding service (already deployed)
- Firestore vector search indexes
- LangGraph tool execution framework

## Risks
- HIGH: Vector search performance with large knowledge bases
- MEDIUM: Embedding model compatibility between indexing and search
- MEDIUM: Rate limiting to prevent abuse
- LOW: Proto breaking changes affecting existing clients

## Cross-Repo Impact
| Repo | Changes Required |
|------|------------------|
| protos | New message types and service method |
| pythonia | New tool implementation |
| mindset_v2 | None (uses existing gRPC client) |
| sdk2-docs | Documentation updates |

## Estimated Complexity: MEDIUM
- Proto definitions: 1-2 hours
- Backend implementation: 4-6 hours
- Integration: 2-3 hours
- Testing: 2-3 hours
- Total: 9-14 hours

**WAITING FOR CONFIRMATION**: Proceed with this plan? (yes/no/modify)

Once confirmed, I will update:
- blueprints/pods/wip/features/feat-widget-mcp/context.md "What's Next" section
```

## Important Notes

**CRITICAL**: The planner agent will **NOT** write any code until you explicitly confirm the plan with "yes" or "proceed" or similar affirmative response.

**Plans are stored in FEAT.md**: After confirmation, the plan will be added to the feature's `FEAT.md` file in the "What's Next" section. This ensures:
- Persistence across Claude sessions
- Visibility for other team members
- Single source of truth for the feature

If you want changes, respond with:
- "modify: [your changes]"
- "different approach: [alternative]"
- "skip phase 2 and do phase 3 first"

## Specifying the Feature

You can specify which feature the plan relates to:

```
/plan feat-widget-mcp: Add export capability for generated widgets
/plan feat-journeys: Implement conditional branching in journey steps
```

If no feature is specified, the planner will ask which feature context to use or create a standalone plan.

## Integration with Other Commands

After planning:
- Use `/tdd` to implement with test-driven development
- Use `/build-and-fix` if build errors occur
- Use `/code-review` to review completed implementation
- Update `FEAT.md` as items are completed

## Related Agents

This command invokes the `planner` agent located at:
`~/.claude/agents/planner.md`
