---
description: Restate requirements, assess risks, and create step-by-step implementation plan. WAIT for user CONFIRM before touching any code.
---

# Plan Command

This command invokes the **planner** agent to create a comprehensive implementation plan before writing any code.

## ⚠️ Critical: Planning Only - No Implementation

**This command is for PLANNING ONLY.** The planner agent will:
- Research the codebase
- Analyze requirements
- Create a detailed plan
- Write the plan to FEAT.md and TRACKING.md

**The planner will NOT write any code.** All implementation must be done:
1. In a **new session** (see end of this document)
2. Using **specialized sub-agents** (tdd-guide, build-error-resolver, code-reviewer, etc.)

## What This Command Does

1. **Determine Feature Location** - Identify or create the pod/feature structure
2. **Load Required Skills** - Read skill files declared in FEAT.md
3. **Restate Requirements** - Clarify what needs to be built
4. **Read Feature Context** - Load context from FEAT.md and TRACKING.md
5. **Identify Risks** - Surface potential issues and blockers
6. **Create Step Plan** - Break down implementation into phases
7. **Write to Feature Files** - Update FEAT.md and TRACKING.md with the plan
8. **Wait for Confirmation** - MUST receive user approval before writing
9. **Instruct New Session** - Tell user to start fresh session for implementation

## Feature Location: Pods and Structure

All plans are written to the blueprints pod/feature structure:

```
blueprints/pods/[pod-name]/features/[feature-name]/
├── FEAT.md         # THE living document - plan goes in "What's Next" section
├── TRACKING.md     # Cross-repo branches, deployment order, files changed
└── archive/        # Historical docs (analysis, old plans, reviews)
```

### Available Pods

| Pod | Purpose |
|-----|---------|
| `wip` | Work-in-progress, active development, experimental |
| `ps1` | Product sprint 1 |
| `ps2` | Product sprint 2 |
| `platform` | Infrastructure, cross-cutting concerns |

### Determining the Pod

When `/plan` is invoked:

1. **If feature specified** (e.g., `/plan feat-journeys: add X`):
   - Find the feature at `blueprints/pods/*/features/feat-journeys/`
   - Use that pod location

2. **If new feature needed**:
   - Ask the user which pod this belongs to
   - Create the feature folder using the template structure
   - Then proceed with planning

3. **If unclear**:
   - Ask: "Which pod should this feature belong to? (wip/ps1/ps2/platform)"

## Loading Skills Before Planning

Before creating any plan, the planner MUST load relevant skills:

1. **If feature exists** - Parse the `## Skills` section from FEAT.md:
   ```markdown
   ## Skills
   - python
   - grpc
   - flutter
   ```
   Read each skill from `blueprints/.claude/skills/{skill}/SKILL.md`

2. **If new feature** - Ask user which skills are relevant, or infer from request:
   - Python backend work → load `python`, `grpc`
   - Flutter UI work → load `flutter`, `firebase`
   - React SDK work → load `react`
   - MCP server work → load `mcp`, `python`

3. **Always load `architecture`** - For cross-repo understanding

Available skills: `python`, `flutter`, `react`, `grpc`, `firebase`, `mcp`, `testing`, `architecture`

## Where the Plan Gets Written

**IMPORTANT**: Plans are NOT written to separate files. They go directly into the feature documents:

### FEAT.md - The "What's Next" Section

The implementation plan is added to the "What's Next" section with:
- Prioritized phases
- Specific steps with file paths
- Dependencies and risks
- Estimated complexity

```markdown
## What's Next

### Prioritized

1. **Phase 1: Proto Definitions** (protos/)
   - Prerequisites: None
   - Steps:
     - Define KnowledgeSearchRequest in `protos/ai/v1/agent.proto`
     - Run `buf lint` and `buf generate`
   - Risk: Low

2. **Phase 2: Python Backend** (pythonia/)
   - Prerequisites: Phase 1 complete
   - Steps:
     - Create `cloud-functions/.../tools/knowledge_search.py`
     - Add unit tests
   - Risk: Medium
```

### TRACKING.md - Branches and Files

Update these sections with the planned changes:

```markdown
## Branches

| Repository | Branch | Base | Status | PR |
|------------|--------|------|--------|-----|
| protos | `feature/knowledge-search` | `main` | Planned | - |
| pythonia | `feature/knowledge-search` | `main` | Planned | - |

## Files Changed

### protos
**Added:**
- `ai/v1/knowledge_search.proto` - Search request/response types

### pythonia
**Added:**
- `cloud-functions/.../tools/knowledge_search.py` - Search tool implementation

## Deployment Order

1. **protos** - Proto definitions must be published first
2. **pythonia** - Backend implementation depends on protos
```

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

1. **Determine feature location** - Find existing or create new pod/feature
2. **Load skills** - Read skill files for domain knowledge
3. **Analyze the request** and restate requirements in clear terms
4. **Read feature context** from FEAT.md and TRACKING.md
5. **Break down into phases** with specific, actionable steps
6. **Identify dependencies** between components (Python backend, Flutter frontend, protos)
7. **Assess risks** and potential blockers
8. **Estimate complexity** (High/Medium/Low)
9. **Present the plan** and WAIT for your explicit confirmation
10. **Write to FEAT.md** - Update "What's Next" section with the approved plan
11. **Write to TRACKING.md** - Update branches, files, deployment order
12. **Instruct new session** - Tell user to start fresh for implementation

## Example Usage

```
User: /plan I need to add a new tool to the AI agent that allows it to search knowledge bases

Agent (planner):
# Implementation Plan: Knowledge Base Search Tool

## Feature Location
This appears to be new functionality. Which pod should this belong to?
- wip (work-in-progress)
- ps1 (product sprint 1)
- ps2 (product sprint 2)
- platform (infrastructure)

User: wip

Agent (planner):
Creating feature at: blueprints/pods/wip/features/feat-knowledge-search/

## Skills Loaded
- python (LangGraph, cloud functions)
- grpc (Protocol Buffers, service patterns)
- architecture (cross-repo patterns)

## Requirements Restatement
[... detailed requirements ...]

## Implementation Phases
[... detailed phases ...]

## Risks & Dependencies
[... risk assessment ...]

---

**WAITING FOR CONFIRMATION**: Approve this plan? (yes/no/modify)

Once approved, I will:
1. Write the plan to `blueprints/pods/wip/features/feat-knowledge-search/FEAT.md`
2. Update `TRACKING.md` with branches and file changes
3. You should then START A NEW SESSION to implement
```

## Important Notes

**CRITICAL**: The planner agent will **NOT** write any code. It only:
- Researches the codebase
- Creates the plan
- Writes to FEAT.md and TRACKING.md

If you want changes to the plan, respond with:
- "modify: [your changes]"
- "different approach: [alternative]"
- "skip phase 2 and do phase 3 first"

## Specifying the Feature

You can specify which feature the plan relates to:

```
/plan feat-widget-mcp: Add export capability for generated widgets
/plan feat-journeys: Implement conditional branching in journey steps
```

If no feature is specified, the planner will ask which feature context to use or help create a new one.

## ⚠️ CRITICAL: Start a New Session for Implementation

After the plan is approved and written to FEAT.md/TRACKING.md:

**DO NOT continue implementation in this session. DO NOT write any code.**

---

### In Your New Session, You MUST:

#### 1. Load the Feature Context and Skills
```
/load-feature {feature-name}
```
This loads:
- The implementation plan from FEAT.md "What's Next" section
- The required skills declared in FEAT.md (e.g., python, grpc, flutter)
- Branch and file tracking from TRACKING.md

#### 2. Use Sub-Agents for ALL Implementation

**Implementation MUST be done via sub-agents. Do NOT write code directly.**

| Command | Sub-Agent | Use For |
|---------|-----------|---------|
| `/tdd` | tdd-guide | Write tests FIRST, then minimal code to pass |
| `/build-fix` | build-error-resolver | Fix build, type, or lint errors |
| `/code-review` | code-reviewer | Review completed code for quality |
| `/refactor-clean` | refactor-cleaner | Clean up dead code, consolidate |
| `/verify` | security-reviewer | Check for vulnerabilities |

**Why sub-agents?**
- They have specialized expertise for each task
- They follow established patterns (TDD, security, etc.)
- They produce higher quality, more consistent code
- Direct coding bypasses quality gates

#### 3. Follow the Implementation Phases
Work through the phases in FEAT.md "What's Next" section in order.

---

### Why a New Session?

- **Context efficiency**: Planning sessions accumulate research context that consumes tokens
- **Clean slate**: Implementation works better focused on the approved plan
- **Persistence**: The plan is saved in FEAT.md - it survives session boundaries
- **Sub-agent focus**: Implementation agents work best without planning context baggage

## Related Agents

This command invokes the `planner` agent located at:
`blueprints-cc-plugin/agents/planner.md`
