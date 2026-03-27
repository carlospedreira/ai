# AI-Powered Testing

AI-powered testing uses [[Large Language Models]] and [[AI Coding Harnesses|coding agents]] to generate, maintain, and improve software tests. Testing is one of the most natural fits for AI — it's structured, verifiable, and often mechanical work that developers routinely skip.

## Why AI + Testing Is a Natural Fit

1. **Verifiable** — Tests either pass or fail; no subjective judgment needed
2. **Pattern-heavy** — Test structure is repetitive across a codebase
3. **Self-correcting** — The agent can run the test and fix it if it fails
4. **High ROI** — Most codebases are under-tested; AI closes the gap cheaply
5. **Spec-like** — Tests serve as living documentation of expected behavior

## AI Testing Workflows

### Test Generation from Code

```
Developer: "Generate unit tests for src/services/payment.ts"

Agent:
  1. Reads the source file
  2. Identifies public functions and their signatures
  3. Reads existing test patterns in the project
  4. Generates tests covering happy path, edge cases, error conditions
  5. Runs the tests
  6. Fixes any failures
  7. Reports coverage
```

### Test Generation from Specification

```
Developer: "Write tests for this feature:
            - Users can reset their password via email
            - Token expires after 1 hour
            - Token can only be used once
            - Invalid token returns 400"

Agent: Generates integration tests covering each requirement
```

### Test-Driven Development (TDD) with AI

The most powerful pattern — see [[AI Coding Workflows#Workflow 2 Test-Driven Development with AI]]:

```
1. Developer describes the feature
2. Agent writes failing tests first
3. Agent runs tests (all fail — correct)
4. Agent implements the feature
5. Agent runs tests again
6. Agent fixes code until all tests pass
7. Developer reviews both tests and implementation
```

### Test Maintenance

Existing tests break when code changes. AI can fix them:

```
Developer: "I refactored the auth module. Fix the broken tests."

Agent:
  1. Runs test suite → identifies 12 failures
  2. Reads the refactored code to understand what changed
  3. Updates tests to match new interfaces
  4. Distinguishes between: tests that need updating vs bugs exposed by tests
  5. Runs suite → all green
```

### Mutation Testing

Intentionally introduce bugs, verify tests catch them:

```
Agent:
  1. Reads src/calculator.ts
  2. Mutates: change "+" to "-" on line 15
  3. Runs tests → do they fail? If no → test gap identified
  4. Reverts mutation
  5. Reports which mutations survived (uncaught by tests)
  6. Generates tests to catch surviving mutants
```

## Types of Tests AI Can Generate

| Test Type | AI Quality | Notes |
|---|---|---|
| **Unit tests** | Excellent | Most common AI testing use case |
| **Integration tests** | Good | Can set up fixtures and mock services |
| **API tests** | Excellent | Generate from OpenAPI specs or code |
| **Property-based tests** | Good | Generate invariants and random input generators |
| **Snapshot tests** | Easy | Capture and compare output |
| **E2E tests** | Moderate | Needs browser control (Cline/Playwright) |
| **Performance tests** | Moderate | Can write benchmarks; interpreting results is harder |
| **Security tests** | Good | Generates tests for OWASP patterns |

## What AI Gets Right

- **Happy path** — Standard input → expected output
- **Null/empty cases** — Empty strings, null values, empty arrays
- **Boundary conditions** — Zero, negative numbers, max values
- **Type validation** — Invalid types, missing required fields
- **Error scenarios** — Network errors, timeout, permission denied
- **Following patterns** — Matches existing test style in the project

## What AI Gets Wrong

- **Business logic edge cases** — Doesn't know domain rules ("VIP customers get free shipping on orders over $100 but not during flash sales")
- **Flaky tests** — May generate tests with timing dependencies
- **Over-mocking** — Tests that mock everything test nothing
- **Testing implementation instead of behavior** — Brittle tests that break on any refactor
- **Missing the "why"** — Tests without clear purpose or intent

## The Coverage Problem

AI can achieve high code coverage quickly, but coverage ≠ quality:

```
High coverage, low quality:
  test("processPayment works", () => {
    const result = processPayment(mockData);
    expect(result).toBeDefined();  // Passes even if result is wrong!
  });

Lower coverage, high quality:
  test("processPayment charges correct amount after discount", () => {
    const result = processPayment({ amount: 100, discount: 0.2 });
    expect(result.charged).toBe(80);
    expect(result.discount_applied).toBe(20);
  });
```

Best practice: have the AI generate tests, then **review the assertions** — not just whether tests pass.

## Tools

### Built Into Harnesses

Every [[AI Coding Harnesses|coding agent]] can generate and run tests:

```bash
# Claude Code
claude -p "Generate comprehensive tests for src/auth/ and run them"

# OpenCode
opencode run "Write tests for the payment module following our test patterns"

# Aider
aider --message "Add tests for the new rate limiter"
```

### Specialized Tools

| Tool | Focus |
|---|---|
| **CodiumAI / Qodo** | AI-powered test generation, VS Code extension |
| **Diffblue Cover** | AI unit test generation for Java |
| **EvoSuite** | Search-based test generation |
| **Playwright + AI** | AI-generated E2E browser tests |

## Metrics

| Metric | What It Measures |
|---|---|
| **Line coverage** | % of code lines executed by tests |
| **Branch coverage** | % of conditional branches exercised |
| **Mutation score** | % of intentional bugs caught by tests |
| **Test-to-code ratio** | Lines of test code per line of source code |
| **Assertion density** | Assertions per test (higher = more thorough) |
| **Flakiness rate** | % of tests that intermittently fail |

## The AI Testing Loop

The ideal workflow combines AI generation with human review:

```
AI generates tests → AI runs tests → AI fixes failures →
Human reviews test quality → Human adds domain-specific cases →
AI maintains tests as code evolves
```

## Related

- [[AI Coding Workflows]]
- [[AI-Assisted Debugging]]
- [[AI Code Review]]
- [[AI Pair Programming]]
- [[AI Coding Harnesses]]
- [[AI in CI-CD]]
- [[Evaluation and Metrics]]
