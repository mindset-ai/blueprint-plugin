# Code Review for Mindset Platform

Comprehensive security, quality, and standards review of uncommitted changes across the Mindset AI platform.

## Pre-Review Setup

1. **Get changed files**: `git diff --name-only HEAD`

2. **Identify file types and load relevant standards**:
   - Flutter/Dart files (`.dart`) - Load `blueprint/.claude/skills/flutter/SKILL.md`
   - Python files (`.py`) - Load `blueprint/.claude/skills/python/SKILL.md`
   - React/JS files (`.jsx`, `.js`, `.tsx`, `.ts`) - Load `blueprint/.claude/skills/react/SKILL.md`

3. **Check for feature CLAUDE.md**: Look for `llm/[story-number]/CLAUDE.md` in the relevant project directory for feature-specific decisions and constraints to follow

---

## Security Issues (CRITICAL - Block Commit)

- Hardcoded credentials, API keys, tokens, service account paths
- SQL injection or NoSQL injection vulnerabilities (Firestore queries)
- XSS vulnerabilities in React components
- Missing input validation on gRPC request handlers
- Insecure dependencies or outdated packages with known CVEs
- Path traversal risks in file operations
- Exposed secrets in logs (check `CF.log.*` and `temp_print` calls)
- Missing authentication checks in MCP server middleware
- Unsafe environment variable access without validation

---

## Platform-Specific Checks

### Flutter/Dart (mindset_v2)

**GetX Patterns (HIGH)**:
- [ ] Controllers extend `GetxController` properly
- [ ] Using `.obs` for observable state, not plain variables
- [ ] `Obx()` used for reactive UI, not `GetBuilder` when reactivity needed
- [ ] `Get.lazyPut` used in Bindings, not direct instantiation in widgets
- [ ] `super.onInit()` and `super.onClose()` called in controllers
- [ ] Not using BuildContext after async gaps (use `Get.back()` instead)

**Dart Best Practices (HIGH)**:
- [ ] Proper null handling - no unnecessary force unwrapping (`!`)
- [ ] Using `copyWith` patterns for immutable data classes
- [ ] Stateless widgets preferred over Stateful when using GetX
- [ ] Widgets are small and composable (< 100 lines per build method)

**Testing (MEDIUM)**:
- [ ] Widget tests use `MaterialApp` wrapper
- [ ] Controller tests call `Get.reset()` in tearDown
- [ ] Mock repositories registered with `Get.put`

### Python (pythonia)

**LangGraph Node Patterns (HIGH)**:
- [ ] Nodes extend `AiAgentNodeTemplate`
- [ ] `self.start_node()` called at beginning of `invoke()`
- [ ] `self.end_node()` called before ALL return paths (including errors)
- [ ] State updates returned as dict, NEVER mutating state directly
- [ ] Using `state.get('field', default)` not direct access
- [ ] Accessing `fb_db` from `agent_utilities`, not creating new clients

**Environment & Firebase Patterns (HIGH)**:
- [ ] Using standard environment detection pattern (`WEB_MOBILE_LOCAL`)
- [ ] Project ID from `DEBUG_PROJECT_ID` or `GOOGLE_CLOUD_PROJECT`
- [ ] Firestore accessed via `FirestoreWrapperClass`, not raw client in nodes
- [ ] Secrets accessed via `CF.getSecret()`, not hardcoded

**gRPC Service Patterns (HIGH)**:
- [ ] Services properly handle exceptions with `context.abort()`
- [ ] Logging via `CF.log.sysLog` and `CF.log.errLog`
- [ ] Request parameters validated before processing
- [ ] Streaming RPCs yield `is_final=True` on completion

**Testing (MEDIUM)**:
- [ ] Tests mock Firestore properly with `Mock()` objects
- [ ] gRPC tests use proper channel/stub pattern
- [ ] `temp_print` calls removed before commit

### React/JavaScript (mindset_v2/js)

**React Patterns (HIGH)**:
- [ ] Functional components with hooks, no class components
- [ ] `useCallback` for event handlers passed as props
- [ ] `useMemo` for expensive computations
- [ ] Context providers properly wrap consumers
- [ ] Custom hooks throw errors when used outside provider

**Flutter Integration (HIGH)**:
- [ ] Custom elements have required attributes (`app-uid`, `external-tenant-id`)
- [ ] Theme attributes properly JSON stringified
- [ ] SDK initialization checks before component loading

**Testing (MEDIUM)**:
- [ ] Tests use Vitest globals (`describe`, `it`, `expect`, `vi`)
- [ ] Mock functions cleared in `beforeEach`
- [ ] Hook tests use `renderHook` and `act`

---

## gRPC/Proto Compatibility (HIGH)

- [ ] Proto field changes are backwards compatible (no removed required fields)
- [ ] New fields have appropriate default values
- [ ] Enum values not renumbered (only append)
- [ ] Service method signatures match implementation
- [ ] Generated code regenerated after proto changes (`buf generate`)

---

## Code Quality (HIGH - Block Commit)

- Functions > 50 lines (break into smaller functions)
- Files > 800 lines (split into modules)
- Nesting depth > 4 levels
- Missing error handling (try/catch, error boundaries)
- `console.log` statements (use proper logging)
- `temp_print` calls (remove before commit)
- TODO/FIXME comments without linked issue
- Missing JSDoc/docstrings for public APIs

---

## Best Practices (MEDIUM)

- Direct state mutation (use immutable patterns)
- Emoji usage in code/comments (avoid unless explicitly requested)
- Missing tests for new code paths
- Accessibility issues in React components (a11y)
- Hardcoded strings that should be constants
- Unused imports or variables
- Copy-pasted code that should be refactored

---

## Report Format

Generate a structured report with:

```
## Code Review Summary

### Files Reviewed
- [list of changed files]

### Feature Context
- Feature CLAUDE.md found: [yes/no, path if found]
- Key decisions that must be followed: [list any from CLAUDE.md]

### Issues Found

#### CRITICAL (Must Fix)
| File | Line | Issue | Suggested Fix |
|------|------|-------|---------------|
| ... | ... | ... | ... |

#### HIGH (Should Fix)
| File | Line | Issue | Suggested Fix |
|------|------|-------|---------------|
| ... | ... | ... | ... |

#### MEDIUM (Consider Fixing)
| File | Line | Issue | Suggested Fix |
|------|------|-------|---------------|
| ... | ... | ... | ... |

### Platform Standards Compliance
- Flutter/GetX: [PASS/FAIL with details]
- Python/LangGraph: [PASS/FAIL with details]
- React: [PASS/FAIL with details]
- gRPC/Proto: [PASS/FAIL with details]

### Recommendation
[APPROVE / BLOCK with summary]
```

---

## Blocking Rules

**Block commit if ANY of the following:**
- CRITICAL security issues found
- HIGH severity issues found
- Feature CLAUDE.md decisions violated
- Proto compatibility broken
- LangGraph node patterns violated (missing start_node/end_node)
- GetX anti-patterns that will cause runtime issues

**Never approve code with security vulnerabilities!**
