---
description: Enforce test-driven development workflow. Scaffold interfaces, generate tests FIRST, then implement minimal code to pass. Ensure 80%+ coverage.
---

# TDD Command

This command invokes the **tdd-guide** agent to enforce test-driven development methodology for the Mindset AI platform.

## What This Command Does

1. **Scaffold Interfaces** - Define types/interfaces first
2. **Generate Tests First** - Write failing tests (RED)
3. **Implement Minimal Code** - Write just enough to pass (GREEN)
4. **Refactor** - Improve code while keeping tests green (REFACTOR)
5. **Verify Coverage** - Ensure 80%+ test coverage

## When to Use

Use `/tdd` when:
- Implementing new features (AI agents, widgets, controllers)
- Adding new functions/components
- Fixing bugs (write test that reproduces bug first)
- Refactoring existing code
- Building critical business logic

## How It Works

The tdd-guide agent will:

1. **Define interfaces** for inputs/outputs
2. **Write tests that will FAIL** (because code doesn't exist yet)
3. **Run tests** and verify they fail for the right reason
4. **Write minimal implementation** to make tests pass
5. **Run tests** and verify they pass
6. **Refactor** code while keeping tests green
7. **Check coverage** and add more tests if below 80%

## TDD Cycle

```
RED -> GREEN -> REFACTOR -> REPEAT

RED:      Write a failing test
GREEN:    Write minimal code to pass
REFACTOR: Improve code, keep tests passing
REPEAT:   Next feature/scenario
```

## Related Skill

For comprehensive testing patterns and conventions, see the testing skill:
`blueprints/.claude/skills/testing/SKILL.md`

---

## Example: Flutter GetX Controller (Widget Engagement)

```
User: /tdd I need a controller to track widget engagement metrics

Agent (tdd-guide):
# TDD Session: Widget Engagement Controller

## Step 1: Define Interface (SCAFFOLD)

```dart
// lib/src/features/analytics/widget_engagement_controller.dart
import 'package:get/get.dart';

class WidgetEngagement {
  final String widgetId;
  final int viewCount;
  final int interactionCount;
  final Duration timeSpent;

  WidgetEngagement({
    required this.widgetId,
    required this.viewCount,
    required this.interactionCount,
    required this.timeSpent,
  });

  double get engagementScore {
    // TODO: Implementation
    throw UnimplementedError();
  }
}

class WidgetEngagementController extends GetxController {
  final RxList<WidgetEngagement> engagements = <WidgetEngagement>[].obs;
  final RxBool isLoading = false.obs;
  final Rx<String?> error = Rx<String?>(null);

  Future<void> fetchEngagements(String agentId) async {
    // TODO: Implementation
    throw UnimplementedError();
  }

  double calculateAverageEngagement() {
    // TODO: Implementation
    throw UnimplementedError();
  }
}
```

## Step 2: Write Failing Test (RED)

```dart
// test/src/features/analytics/widget_engagement_controller_test.dart
import 'package:core/services/mindset_sevice_provider.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:get/get.dart';
import 'package:mockito/mockito.dart';
import 'package:mockito/annotations.dart';

@GenerateMocks([EngagementRepository])
void main() {
  setUpAll(() {
    TestWidgetsFlutterBinding.ensureInitialized();
    MindsetServiceProvider.enableTestMode();
  });

  late WidgetEngagementController controller;
  late MockEngagementRepository mockRepository;

  setUp(() {
    mockRepository = MockEngagementRepository();
    Get.put<EngagementRepository>(mockRepository);
    controller = WidgetEngagementController(mockRepository);
  });

  tearDown(() {
    Get.reset();
  });

  group('WidgetEngagement', () {
    test('calculates high engagement score for active widget', () {
      final engagement = WidgetEngagement(
        widgetId: 'widget-123',
        viewCount: 100,
        interactionCount: 75,
        timeSpent: const Duration(minutes: 5),
      );

      final score = engagement.engagementScore;

      expect(score, greaterThan(0.7));
      expect(score, lessThanOrEqualTo(1.0));
    });

    test('calculates low engagement score for inactive widget', () {
      final engagement = WidgetEngagement(
        widgetId: 'widget-456',
        viewCount: 100,
        interactionCount: 5,
        timeSpent: const Duration(seconds: 10),
      );

      final score = engagement.engagementScore;

      expect(score, lessThan(0.3));
      expect(score, greaterThanOrEqualTo(0.0));
    });

    test('returns zero engagement score when no views', () {
      final engagement = WidgetEngagement(
        widgetId: 'widget-789',
        viewCount: 0,
        interactionCount: 0,
        timeSpent: Duration.zero,
      );

      final score = engagement.engagementScore;

      expect(score, equals(0.0));
    });
  });

  group('WidgetEngagementController', () {
    test('fetchEngagements updates engagements list on success', () async {
      final testEngagements = [
        WidgetEngagement(
          widgetId: 'widget-1',
          viewCount: 50,
          interactionCount: 30,
          timeSpent: const Duration(minutes: 2),
        ),
      ];
      when(mockRepository.getEngagements(any))
          .thenAnswer((_) async => testEngagements);

      await controller.fetchEngagements('agent-123');

      expect(controller.engagements, equals(testEngagements));
      expect(controller.isLoading.value, false);
      expect(controller.error.value, isNull);
      verify(mockRepository.getEngagements('agent-123')).called(1);
    });

    test('fetchEngagements sets error on failure', () async {
      when(mockRepository.getEngagements(any))
          .thenThrow(Exception('Network error'));

      await controller.fetchEngagements('agent-123');

      expect(controller.engagements, isEmpty);
      expect(controller.isLoading.value, false);
      expect(controller.error.value, contains('Network error'));
    });

    test('calculateAverageEngagement returns correct average', () {
      controller.engagements.value = [
        WidgetEngagement(
          widgetId: 'w1',
          viewCount: 100,
          interactionCount: 80,
          timeSpent: const Duration(minutes: 5),
        ),
        WidgetEngagement(
          widgetId: 'w2',
          viewCount: 100,
          interactionCount: 20,
          timeSpent: const Duration(minutes: 1),
        ),
      ];

      final average = controller.calculateAverageEngagement();

      expect(average, closeTo(0.5, 0.1));
    });
  });
}
```

## Step 3: Run Tests - Verify FAIL

```bash
cd mindset_v2
melos run test

FAIL test/src/features/analytics/widget_engagement_controller_test.dart
  x calculates high engagement score for active widget (2 ms)
    UnimplementedError

3 tests failed, 0 passed
```

Tests fail as expected. Ready to implement.

## Step 4: Implement Minimal Code (GREEN)

```dart
// lib/src/features/analytics/widget_engagement_controller.dart
class WidgetEngagement {
  final String widgetId;
  final int viewCount;
  final int interactionCount;
  final Duration timeSpent;

  WidgetEngagement({
    required this.widgetId,
    required this.viewCount,
    required this.interactionCount,
    required this.timeSpent,
  });

  double get engagementScore {
    if (viewCount == 0) return 0.0;

    // Interaction rate (0-1)
    final interactionRate = interactionCount / viewCount;

    // Time engagement (normalized to 5 minutes = 1.0)
    final timeScore = (timeSpent.inSeconds / 300).clamp(0.0, 1.0);

    // Weighted average
    return (interactionRate * 0.6 + timeScore * 0.4).clamp(0.0, 1.0);
  }
}

class WidgetEngagementController extends GetxController {
  final EngagementRepository _repository;
  final RxList<WidgetEngagement> engagements = <WidgetEngagement>[].obs;
  final RxBool isLoading = false.obs;
  final Rx<String?> error = Rx<String?>(null);

  WidgetEngagementController(this._repository);

  Future<void> fetchEngagements(String agentId) async {
    isLoading.value = true;
    error.value = null;

    try {
      engagements.value = await _repository.getEngagements(agentId);
    } catch (e) {
      error.value = e.toString();
      engagements.clear();
    } finally {
      isLoading.value = false;
    }
  }

  double calculateAverageEngagement() {
    if (engagements.isEmpty) return 0.0;
    final total = engagements.fold<double>(
      0.0,
      (sum, e) => sum + e.engagementScore,
    );
    return total / engagements.length;
  }
}
```

## Step 5: Run Tests - Verify PASS

```bash
cd mindset_v2
melos run test

PASS test/src/features/analytics/widget_engagement_controller_test.dart
  + calculates high engagement score for active widget (3 ms)
  + calculates low engagement score for inactive widget (2 ms)
  + returns zero engagement score when no views (1 ms)
  + fetchEngagements updates engagements list on success (5 ms)
  + fetchEngagements sets error on failure (3 ms)
  + calculateAverageEngagement returns correct average (2 ms)

6 tests passed
```

All tests passing!

## Step 6: Check Coverage

```bash
cd mindset_v2
melos run test -- --coverage

File                              | % Stmts | % Branch | % Funcs | % Lines
----------------------------------|---------|----------|---------|--------
widget_engagement_controller.dart |   100   |   100    |   100   |   100

Coverage: 100% (Target: 80%)
```

TDD session complete!
```

---

## Example: Python LangGraph Node (AI Agent Context Processing)

```
User: /tdd I need a LangGraph node to process agent context and determine response strategy

Agent (tdd-guide):
# TDD Session: Context Strategy Node

## Step 1: Define Interface (SCAFFOLD)

```python
# nodes/context_strategy.py
from dataclasses import dataclass
from enum import Enum
from typing import List, Optional

class ResponseStrategy(Enum):
    DIRECT_ANSWER = "direct_answer"
    RAG_RETRIEVAL = "rag_retrieval"
    CLARIFICATION = "clarification"
    ESCALATION = "escalation"

@dataclass
class ContextInput:
    user_query: str
    conversation_history: List[dict]
    available_knowledge_bases: List[str]
    confidence_threshold: float = 0.7

@dataclass
class StrategyOutput:
    strategy: ResponseStrategy
    confidence: float
    reasoning: str
    selected_knowledge_bases: Optional[List[str]] = None

def determine_response_strategy(context: ContextInput) -> StrategyOutput:
    """Analyze context and determine optimal response strategy."""
    # TODO: Implementation
    raise NotImplementedError()
```

## Step 2: Write Failing Test (RED)

```python
# test_context_strategy.py
import pytest
from nodes.context_strategy import (
    ContextInput,
    StrategyOutput,
    ResponseStrategy,
    determine_response_strategy,
)

class TestDetermineResponseStrategy:
    """Tests for context strategy determination."""

    def test_returns_direct_answer_for_simple_greeting(self):
        """Simple greetings should get direct answers."""
        context = ContextInput(
            user_query="Hello!",
            conversation_history=[],
            available_knowledge_bases=["general_kb"],
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.DIRECT_ANSWER
        assert result.confidence > 0.8

    def test_returns_rag_retrieval_for_knowledge_query(self):
        """Knowledge-seeking queries should trigger RAG retrieval."""
        context = ContextInput(
            user_query="What are the key features of the premium widget?",
            conversation_history=[],
            available_knowledge_bases=["product_kb", "features_kb"],
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.RAG_RETRIEVAL
        assert result.selected_knowledge_bases is not None
        assert len(result.selected_knowledge_bases) > 0

    def test_returns_clarification_for_ambiguous_query(self):
        """Ambiguous queries should request clarification."""
        context = ContextInput(
            user_query="How do I do that?",
            conversation_history=[],
            available_knowledge_bases=["general_kb"],
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.CLARIFICATION
        assert "ambiguous" in result.reasoning.lower() or "unclear" in result.reasoning.lower()

    def test_returns_escalation_when_no_knowledge_bases_available(self):
        """Should escalate when no relevant knowledge is available."""
        context = ContextInput(
            user_query="What is the company's financial forecast for Q4?",
            conversation_history=[],
            available_knowledge_bases=[],  # No knowledge bases
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.ESCALATION

    def test_uses_conversation_history_for_context(self):
        """Should consider conversation history for follow-up questions."""
        context = ContextInput(
            user_query="What about the pricing?",
            conversation_history=[
                {"role": "user", "content": "Tell me about the enterprise plan"},
                {"role": "assistant", "content": "The enterprise plan includes..."},
            ],
            available_knowledge_bases=["pricing_kb"],
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.RAG_RETRIEVAL
        assert "pricing_kb" in result.selected_knowledge_bases

    def test_handles_empty_query_gracefully(self):
        """Empty query should request clarification."""
        context = ContextInput(
            user_query="",
            conversation_history=[],
            available_knowledge_bases=["general_kb"],
        )

        result = determine_response_strategy(context)

        assert result.strategy == ResponseStrategy.CLARIFICATION


class TestContextInputValidation:
    """Tests for input validation."""

    def test_accepts_valid_confidence_threshold(self):
        """Should accept threshold between 0 and 1."""
        context = ContextInput(
            user_query="Test",
            conversation_history=[],
            available_knowledge_bases=[],
            confidence_threshold=0.5,
        )
        assert context.confidence_threshold == 0.5

    def test_default_confidence_threshold(self):
        """Default threshold should be 0.7."""
        context = ContextInput(
            user_query="Test",
            conversation_history=[],
            available_knowledge_bases=[],
        )
        assert context.confidence_threshold == 0.7
```

## Step 3: Run Tests - Verify FAIL

```bash
cd pythonia/cloud-functions/monkee.ai_CR_aiagent_v3
pytest test_context_strategy.py -v

FAILED test_context_strategy.py::TestDetermineResponseStrategy::test_returns_direct_answer_for_simple_greeting
    NotImplementedError

6 failed, 2 passed
```

Tests fail as expected. Ready to implement.

## Step 4: Implement Minimal Code (GREEN)

```python
# nodes/context_strategy.py
import re
from dataclasses import dataclass
from enum import Enum
from typing import List, Optional

class ResponseStrategy(Enum):
    DIRECT_ANSWER = "direct_answer"
    RAG_RETRIEVAL = "rag_retrieval"
    CLARIFICATION = "clarification"
    ESCALATION = "escalation"

@dataclass
class ContextInput:
    user_query: str
    conversation_history: List[dict]
    available_knowledge_bases: List[str]
    confidence_threshold: float = 0.7

@dataclass
class StrategyOutput:
    strategy: ResponseStrategy
    confidence: float
    reasoning: str
    selected_knowledge_bases: Optional[List[str]] = None

# Simple patterns for strategy determination
GREETING_PATTERNS = [
    r"^(hello|hi|hey|good morning|good afternoon|good evening)[\s!.]*$",
]
AMBIGUOUS_PATTERNS = [
    r"^(how|what|why|where|when).*\b(that|this|it)\b.*\?*$",
]

def determine_response_strategy(context: ContextInput) -> StrategyOutput:
    """Analyze context and determine optimal response strategy."""
    query = context.user_query.strip().lower()

    # Handle empty query
    if not query:
        return StrategyOutput(
            strategy=ResponseStrategy.CLARIFICATION,
            confidence=1.0,
            reasoning="Empty query requires clarification",
        )

    # Check for simple greetings
    for pattern in GREETING_PATTERNS:
        if re.match(pattern, query, re.IGNORECASE):
            return StrategyOutput(
                strategy=ResponseStrategy.DIRECT_ANSWER,
                confidence=0.95,
                reasoning="Simple greeting detected",
            )

    # Check for ambiguous queries (no context)
    if not context.conversation_history:
        for pattern in AMBIGUOUS_PATTERNS:
            if re.match(pattern, query, re.IGNORECASE):
                return StrategyOutput(
                    strategy=ResponseStrategy.CLARIFICATION,
                    confidence=0.8,
                    reasoning="Ambiguous query without conversation context",
                )

    # Check if knowledge bases are available
    if not context.available_knowledge_bases:
        return StrategyOutput(
            strategy=ResponseStrategy.ESCALATION,
            confidence=0.9,
            reasoning="No knowledge bases available to answer query",
        )

    # Default to RAG retrieval with knowledge base selection
    selected_kbs = _select_relevant_knowledge_bases(
        query, context.available_knowledge_bases, context.conversation_history
    )

    return StrategyOutput(
        strategy=ResponseStrategy.RAG_RETRIEVAL,
        confidence=0.85,
        reasoning="Knowledge query requiring RAG retrieval",
        selected_knowledge_bases=selected_kbs,
    )

def _select_relevant_knowledge_bases(
    query: str,
    available_kbs: List[str],
    history: List[dict],
) -> List[str]:
    """Select relevant knowledge bases based on query and history."""
    selected = []

    # Simple keyword matching for demo
    keywords_to_kb = {
        "pricing": "pricing_kb",
        "price": "pricing_kb",
        "cost": "pricing_kb",
        "feature": "features_kb",
        "product": "product_kb",
    }

    # Check query
    query_lower = query.lower()
    for keyword, kb in keywords_to_kb.items():
        if keyword in query_lower and kb in available_kbs:
            selected.append(kb)

    # Check history for context
    history_text = " ".join(
        msg.get("content", "") for msg in history
    ).lower()
    for keyword, kb in keywords_to_kb.items():
        if keyword in history_text and kb in available_kbs and kb not in selected:
            selected.append(kb)

    # Default to first available if nothing matched
    if not selected and available_kbs:
        selected.append(available_kbs[0])

    return selected
```

## Step 5: Run Tests - Verify PASS

```bash
cd pythonia/cloud-functions/monkee.ai_CR_aiagent_v3
pytest test_context_strategy.py -v

PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_returns_direct_answer_for_simple_greeting
PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_returns_rag_retrieval_for_knowledge_query
PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_returns_clarification_for_ambiguous_query
PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_returns_escalation_when_no_knowledge_bases_available
PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_uses_conversation_history_for_context
PASSED test_context_strategy.py::TestDetermineResponseStrategy::test_handles_empty_query_gracefully
PASSED test_context_strategy.py::TestContextInputValidation::test_accepts_valid_confidence_threshold
PASSED test_context_strategy.py::TestContextInputValidation::test_default_confidence_threshold

8 passed
```

All tests passing!

## Step 6: Check Coverage

```bash
pytest --cov=nodes.context_strategy --cov-report=term-missing

Name                        Stmts   Miss  Cover
-----------------------------------------------
nodes/context_strategy.py      45      0   100%

Coverage: 100% (Target: 80%)
```

TDD session complete!
```

---

## TDD Best Practices

**DO:**
- Write the test FIRST, before any implementation
- Run tests and verify they FAIL before implementing
- Write minimal code to make tests pass
- Refactor only after tests are green
- Add edge cases and error scenarios
- Aim for 80%+ coverage (100% for critical code)

**DON'T:**
- Write implementation before tests
- Skip running tests after each change
- Write too much code at once
- Ignore failing tests
- Test implementation details (test behavior)
- Mock everything (prefer integration tests where sensible)

## Test Types to Include

**Unit Tests** (Function-level):
- Happy path scenarios
- Edge cases (empty, null, max values)
- Error conditions
- Boundary values

**Integration Tests** (Component-level):
- gRPC endpoints
- Firestore operations
- LangGraph node chains
- GetX controller + repository interactions

**Widget Tests** (Flutter UI):
- Component rendering
- User interactions
- State changes
- Golden tests for visual regression

## Coverage Requirements

- **80% minimum** for all code
- **100% required** for:
  - AI agent decision logic
  - Widget configuration parsing
  - Authentication/authorization logic
  - Financial or billing calculations

## Test Commands

### Flutter (mindset_v2)
```bash
cd mindset_v2

# Run all tests
melos run test

# Run tests for specific package
cd packages/core && flutter test

# Run with coverage
melos run test -- --coverage

# Run single test file
flutter test test/path/to/test_file_test.dart
```

### Python (pythonia)
```bash
cd pythonia/cloud-functions/<function-name>

# Run all tests
pytest

# Run with verbose output
pytest -v

# Run with coverage
pytest --cov=. --cov-report=term-missing

# Run specific test file
pytest test_main.py

# Run tests matching pattern
pytest -k "test_strategy"
```

## Important Notes

**MANDATORY**: Tests must be written BEFORE implementation. The TDD cycle is:

1. **RED** - Write failing test
2. **GREEN** - Implement to pass
3. **REFACTOR** - Improve code

Never skip the RED phase. Never write code before tests.

## Integration with Other Commands

- Use `/plan` first to understand what to build
- Use `/tdd` to implement with tests
- Use `/build-and-fix` if build errors occur
- Use `/code-review` to review implementation
- Use `/test-coverage` to verify coverage

## Related Resources

- **Testing Skill**: `blueprints/.claude/skills/testing/SKILL.md`
- **tdd-guide Agent**: `~/.claude/agents/tdd-guide.md`
- **tdd-workflow Skill**: `blueprints/.claude/skills/tdd-workflow/`
