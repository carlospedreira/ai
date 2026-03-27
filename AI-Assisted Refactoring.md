# AI-Assisted Refactoring

AI-assisted refactoring uses [[AI Coding Harnesses|coding agents]] to restructure existing code — improving its design, readability, and maintainability without changing external behavior. Refactoring is a sweet spot for AI: it requires understanding patterns across many files but follows well-known transformations.

## Why AI Excels at Refactoring

1. **Cross-file awareness** — Agents read entire modules, tracing dependencies
2. **Pattern recognition** — Identifies code smells and suggests established patterns
3. **Mechanical precision** — Rename-everywhere, extract-method, move-class without missing references
4. **Test verification** — Runs tests after each change to ensure behavior is preserved
5. **Scale** — Can refactor 50 files where a human might stop at 5

## Common AI Refactoring Tasks

### Rename and Move

```
Developer: "Rename UserManager to UserService throughout the codebase
            and move it from src/managers/ to src/services/"

Agent:
  1. Finds all references (imports, usages, type annotations, comments)
  2. Renames the class and file
  3. Updates all import paths
  4. Updates all usages
  5. Runs tests to verify
```

### Extract Pattern

```
Developer: "The payment logic in OrderService, SubscriptionService, and
            RefundService is duplicated. Extract it into a shared PaymentGateway."

Agent:
  1. Reads all three services
  2. Identifies the common payment code
  3. Creates PaymentGateway with the shared logic
  4. Updates all three services to delegate to PaymentGateway
  5. Runs tests
```

### Design Pattern Application

```
Developer: "We have a switch statement in NotificationSender that handles
            email, SMS, and push. Refactor to use the Strategy pattern."

Agent:
  1. Reads the switch statement
  2. Creates NotificationStrategy interface
  3. Creates EmailStrategy, SMSStrategy, PushStrategy implementations
  4. Creates StrategyFactory
  5. Replaces the switch with strategy dispatch
  6. Updates tests
```

### Modernization

```
Developer: "Migrate all callback-based async code in src/api/ to async/await"

Agent:
  1. Finds all callback patterns
  2. Converts each to async/await
  3. Handles error callbacks → try/catch
  4. Updates callers
  5. Runs tests for each converted file
```

### Dependency Upgrade

```
Developer: "Upgrade from Express 4 to Express 5. Handle all breaking changes."

Agent:
  1. Reads the Express 5 migration guide (via web search)
  2. Finds all Express usage in the codebase
  3. Updates each breaking change
  4. Runs tests after each batch of changes
  5. Reports what changed and why
```

## The Refactoring Safety Net

The critical requirement: **refactoring must not change behavior**. AI agents verify this through:

```
1. Run full test suite BEFORE refactoring → all pass
2. Make refactoring changes
3. Run full test suite AFTER → all must still pass
4. If any test fails → investigate: bug in refactoring or test needs updating?
```

Without tests, AI refactoring is risky — there's no way to verify correctness. This is why [[AI-Powered Testing|generating tests first]] is often the right first step.

## Refactoring with Sub-Agents

Large refactorings benefit from [[AI Agents and Sub-Agents|sub-agents]]:

```
Parent Agent: "Migrate the codebase from Moment.js to Day.js"

Sub-agent 1 (Explore): Find all Moment.js usages across the codebase
Sub-agent 2 (Plan):    Map each Moment API to its Day.js equivalent
Parent Agent:          Execute the migration file by file
Sub-agent 3 (Test):    Run tests after each batch
```

[[Claude Code]] supports this natively with Explore, Plan, and general-purpose sub-agents.

## Code Smells AI Can Detect and Fix

| Code Smell | AI Fix |
|---|---|
| **Duplicated code** | Extract shared function or class |
| **Long method** | Break into smaller, named methods |
| **Large class** | Split by responsibility (SRP) |
| **Long parameter list** | Introduce parameter object |
| **Feature envy** | Move method to the class it accesses most |
| **Primitive obsession** | Introduce value objects |
| **Switch statements** | Replace with polymorphism |
| **Dead code** | Remove unused functions, imports, variables |
| **Magic numbers** | Extract named constants |
| **Deep nesting** | Invert conditions, extract methods, early returns |

## What AI Struggles With

| Challenge | Why |
|---|---|
| **Architectural refactoring** | Requires understanding system-level trade-offs |
| **Performance refactoring** | Needs profiling data, not just code reading |
| **Breaking changes** | Must understand downstream consumers |
| **Refactoring without tests** | No safety net to verify correctness |
| **Knowing when to stop** | AI may over-refactor ("gold-plating") |

## Best Practices

1. **Have tests first** — Generate them with AI if they don't exist
2. **Small, incremental changes** — Refactor one pattern at a time
3. **Run tests after each change** — Catch regressions immediately
4. **Review the diff** — Ensure the refactoring is an improvement
5. **Use the plan agent** — Read-only analysis before making changes
6. **Git checkpoint** — Commit before each refactoring phase for easy rollback

## Related

- [[AI Coding Workflows]]
- [[AI-Powered Testing]]
- [[AI-Assisted Debugging]]
- [[AI Pair Programming]]
- [[AI Agents and Sub-Agents]]
- [[AI Coding Harnesses]]
- [[Agentic Design Patterns]]
