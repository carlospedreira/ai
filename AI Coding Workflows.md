# AI Coding Workflows

This note covers practical workflows for using [[AI Coding Harnesses]] effectively in real software development. The best results come not from a single prompt, but from structured patterns that leverage the [[Agentic Coding Paradigm]].

## Workflow 1: Explore → Plan → Implement

The most reliable pattern for non-trivial tasks.

### 1. Explore
Ask the agent to understand the codebase before making changes:
```
"Read through the authentication module and explain how login flow works.
Don't make any changes yet."
```

The agent reads files, traces call chains, and builds a mental model. Using a read-only agent (like [[OpenCode]]'s `plan` agent) is ideal for this phase.

### 2. Plan
Have the agent outline its approach:
```
"Now plan how you'd add OAuth2 support. List the files you'd change
and what each change would be."
```

Review the plan. Catch architectural mistakes before code is written.

### 3. Implement
Execute the plan:
```
"Go ahead and implement the plan. Run the tests after each file change."
```

The agent writes code, runs tests, and self-corrects failures.

## Workflow 2: Test-Driven Development with AI

AI agents are exceptionally good at TDD because they can run tests in the loop.

```
"Write failing tests for a rate limiter that:
- Allows 100 requests per minute per user
- Returns 429 when exceeded
- Resets after the window expires

Then implement the rate limiter to make all tests pass."
```

The agent:
1. Writes test cases
2. Runs them (all fail)
3. Implements the feature
4. Runs tests again
5. Fixes failures iteratively
6. Reports when all tests pass

## Workflow 3: Bug Investigation

```
"Users report that the checkout flow fails intermittently with a 500 error.
The error logs show a NullPointerException in PaymentService.java.
Investigate the root cause and fix it."
```

The agent:
1. Reads the error logs / stack trace
2. Opens the relevant file at the error line
3. Traces the null reference back to its source
4. Identifies the race condition / missing null check
5. Implements a fix
6. Adds a test that reproduces the bug
7. Verifies the fix

## Workflow 4: Code Review Assist

Use the agent as a reviewer before submitting a PR:
```
"Review the changes in this branch compared to main. Look for:
- Security issues
- Performance problems
- Missing error handling
- Deviations from our coding standards (see CLAUDE.md)"
```

## Workflow 5: Large-Scale Refactoring

Break the refactoring into phases using [[AI Agents and Sub-Agents|sub-agents]]:

```
"Migrate all API endpoints from Express to Fastify. Do it in phases:
1. First, audit all existing routes and list them
2. Migrate one route as a reference implementation
3. Migrate the remaining routes following the same pattern
4. Update all tests
5. Verify everything passes"
```

The agent handles each phase, validating before moving to the next.

## Workflow 6: Documentation Generation

```
"Read through the src/api/ directory and generate OpenAPI documentation
for each endpoint. Include request/response schemas, status codes,
and example payloads."
```

Agents are fast at reading code and producing structured documentation — especially when the output format is well-defined.

## Workflow 7: Codebase Onboarding

For joining a new project:
```
"I'm new to this codebase. Give me a guided tour:
1. What does this project do?
2. What's the directory structure?
3. What are the key entry points?
4. How do I build and run it?
5. What are the main abstractions I should understand?"
```

## Workflow 8: CI/CD Pipeline Debugging

```
"The GitHub Actions build is failing. Here's the error output:
[paste error]

Look at the workflow file and the relevant code to figure out why
it's failing and fix it."
```

## Anti-Patterns to Avoid

| Anti-Pattern | Problem | Better Approach |
|---|---|---|
| "Fix everything" | Too vague, agent wanders | Break into specific tasks |
| Never reading the plan | Agent may go in wrong direction | Review plans before implementation |
| Ignoring test failures | Agent may patch symptoms | Require tests to pass before accepting |
| One giant session | Context degrades over time | Use `/compact` or start fresh sessions |
| Not using CLAUDE.md | Agent lacks project context | Write good project instructions |

## The Prompt-Code-Test Loop

The most effective meta-pattern across all workflows:

```
┌──────────┐
│  Prompt  │
│  (human) │
└────┬─────┘
     ▼
┌──────────┐
│   Code   │◄──┐
│  (agent) │   │
└────┬─────┘   │
     ▼         │
┌──────────┐   │
│   Test   │───┘ (fails → agent retries)
│  (agent) │
└────┬─────┘
     ▼ (passes)
┌──────────┐
│  Review  │
│  (human) │
└──────────┘
```

Humans provide intent and review. The agent handles the code-test iteration loop.

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[AI Agents and Sub-Agents]]
- [[Context Management]]
- [[Prompt Engineering]]
