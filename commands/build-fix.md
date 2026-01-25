# Build and Fix

Incrementally fix build and lint errors across the Mindset platform.

## Project Detection

Identify the project type and use the appropriate commands:

| Project | Location | Detection |
|---------|----------|-----------|
| Flutter (mindset_v2) | `mindset_v2/` | `melos.yaml`, `pubspec.yaml` |
| Python (pythonia) | `pythonia/` | `requirements.txt`, `pyproject.toml` |
| React SDK | `mindset_v2/packages/react_sdk/` | `package.json` with React deps |
| Protos | `protos/` | `buf.yaml`, `*.proto` files |
| Documentation | `sdk2-docs/` | `mintlify.json` |

## Build Commands by Project

### Flutter (mindset_v2)
```bash
cd mindset_v2

# Analysis and linting
melos run analyze                    # Run dart analyze across all packages
dcm analyze <file_or_directory>      # Dart Code Metrics (run after edits)

# Testing
melos run test                       # Run all tests

# Build verification
melos run build:check                # Verify builds compile
```

### Python (pythonia)
```bash
cd pythonia/cloud-functions/<function-name>

# Linting
python -m flake8 .                   # Style checking
python -m pylint *.py                # Code analysis
python -m mypy .                     # Type checking

# Testing
python -m pytest                     # Run all tests
python -m pytest -v                  # Verbose output
python -m pytest --tb=short          # Short tracebacks
```

### React SDK
```bash
cd mindset_v2/packages/react_sdk

# Build
npm run build                        # Production build
npm run build:dev                    # Development build

# Linting
npm run lint                         # ESLint
npm run lint:fix                     # Auto-fix lint issues

# Type checking
npm run typecheck                    # TypeScript validation

# Testing
npm run test                         # Run tests
```

### Protos
```bash
cd protos

# Linting
buf lint                             # Lint proto files
buf breaking --against .git#branch=main  # Breaking change detection

# Generation
buf generate --include-imports --include-wkt  # Generate source code

# Push (after validation)
buf push                             # Push to buf.build/mindset-ai
```

### Documentation (sdk2-docs)
```bash
cd sdk2-docs

# Validation
mintlify dev                         # Local dev server (catches errors)
```

## Execution Process

1. **Detect project type** from current directory or file being edited

2. **Run appropriate build/lint command**:
   - Flutter: `melos run analyze` first, then `dcm analyze` on changed files
   - Python: `python -m flake8` then `python -m pytest`
   - React: `npm run lint` then `npm run build`
   - Protos: `buf lint` then `buf generate`

3. **Parse error output**:
   - Group errors by file
   - Sort by severity (errors before warnings)
   - Identify error patterns

4. **For each error**:
   - Show error context (5 lines before/after)
   - Explain the issue clearly
   - Propose fix with rationale
   - Apply fix
   - Re-run relevant check
   - Verify error resolved before moving on

5. **Stop if**:
   - Fix introduces new errors
   - Same error persists after 3 attempts
   - User requests pause
   - Build/test goes from passing to failing

6. **Show summary**:
   - Errors fixed (count and list)
   - Errors remaining
   - New errors introduced (if any)
   - Warnings that may need attention

## Safety Guidelines

- Fix ONE error at a time
- Always verify fix before proceeding
- For Flutter: run `dcm analyze` after each significant change
- For Python: run tests after fixing lint errors
- For React: type-check before building
- For Protos: regenerate after any proto changes

## Cross-Project Considerations

When changes span multiple projects:

1. **Proto changes** require regeneration in consuming projects:
   ```bash
   cd protos && buf generate
   # Then rebuild Flutter and Python consumers
   ```

2. **Python backend changes** may require Flutter/React SDK updates

3. Run relevant tests in all affected projects before considering complete
