# Verification Command

Run comprehensive verification on current codebase state for the Mindset platform.

## Instructions

First, determine which project(s) were modified by checking git status or the feature context. Then execute verification for each affected project.

### 1. Identify What to Verify

Check the feature context (if available) to understand:
- Which projects are affected (mindset_v2, pythonia, protos, react SDK)
- Which specific packages or modules changed
- Any test requirements from the implementation plan

### 2. Flutter Verification (mindset_v2)

```bash
cd /Users/barriehadfield/dev/active/mindset_v2

# Static analysis
melos run analyze

# Run all tests
melos run test

# Optional: Run tests for specific package
melos run test --scope=<package_name>

# Optional: Dart Code Metrics for changed files
dcm analyze <path_to_changed_file>
```

### 3. Python Verification (pythonia)

```bash
cd /Users/barriehadfield/dev/active/pythonia/cloud-functions/<function_name>

# Activate virtual environment
source venv/bin/activate

# Run tests
pytest

# Type checking (if configured)
mypy . --ignore-missing-imports

# Lint check (if configured)
ruff check . || pylint *.py
```

### 4. React SDK Verification (mindset_v2/packages/react_sdk_v3)

```bash
cd /Users/barriehadfield/dev/active/mindset_v2/packages/react_sdk_v3

# Run tests
npm test

# Lint check
npm run lint

# Type check (if TypeScript)
npm run typecheck || npx tsc --noEmit

# Build check
npm run build
```

### 5. Protocol Buffers Verification (protos)

```bash
cd /Users/barriehadfield/dev/active/protos

# Lint proto files
buf lint

# Check for breaking changes (against remote)
buf breaking --against buf.build/mindset-ai/protos
```

### 6. Cross-Project Checks

- **Console.log/print Audit**: Search for debug statements in source files
- **Git Status**: Show uncommitted changes across all affected projects
- **Integration Points**: If protos changed, verify dependent projects still compile

## Output

Produce a concise verification report:

```
VERIFICATION: [PASS/FAIL]

Projects Checked: [list of projects]

Flutter (mindset_v2):
  Analyze:  [OK/X issues]
  Tests:    [X/Y passed]

Python (pythonia):
  Pytest:   [OK/X failed]
  Types:    [OK/X errors]

React SDK:
  Tests:    [OK/X failed]
  Lint:     [OK/X issues]
  Build:    [OK/FAIL]

Protos:
  Lint:     [OK/X issues]
  Breaking: [OK/X changes]

Debug Statements: [OK/X found]
Git Status:       [clean/X uncommitted files]

Ready for PR: [YES/NO]
```

If any critical issues, list them with fix suggestions.

## Arguments

$ARGUMENTS can be:
- `quick` - Only analyze/lint (no tests)
- `full` - All checks including tests (default)
- `flutter` - Only Flutter verification
- `python` - Only Python verification
- `react` - Only React SDK verification
- `protos` - Only Protocol Buffers verification
- `pre-pr` - Full checks plus breaking change detection

## Feature Context Integration

If working on a story/feature:
1. Check `llm/<story-number>/` for implementation plan
2. Verify all files listed in the plan were properly modified
3. Ensure any required tests from the plan exist and pass
4. Cross-reference acceptance criteria with test coverage
