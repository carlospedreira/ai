# AI-Assisted Debugging

AI-assisted debugging uses [[Large Language Models]] and [[AI Coding Harnesses|coding agents]] to help developers find and fix bugs faster. Debugging is one of the highest-value applications of AI coding tools because it combines pattern recognition (AI's strength) with systematic investigation (where agents excel).

## Why AI Is Good at Debugging

1. **Pattern matching** — AI has seen millions of bugs in training data and recognizes common patterns
2. **Breadth** — Can read entire codebases quickly, tracing issues across files
3. **Tirelessness** — Systematically checks every possibility without getting bored
4. **Error message interpretation** — Trained on vast quantities of error messages and their solutions
5. **Multi-language knowledge** — Can debug across languages and frameworks simultaneously

## Debugging Workflows

### The Basic Flow

```
Error occurs
    ↓
Paste error message/stack trace to AI agent
    ↓
Agent reads relevant source files
    ↓
Agent identifies root cause
    ↓
Agent proposes (or implements) fix
    ↓
Agent runs tests to verify
```

### Workflow 1: Stack Trace Analysis

```
Developer: "Getting this error in production:
            TypeError: Cannot read property 'email' of undefined
            at UserService.getProfile (src/services/user.ts:42)
            at ProfileController.show (src/controllers/profile.ts:18)"

Agent:
  1. Reads src/services/user.ts:42
  2. Reads src/controllers/profile.ts:18
  3. Traces the data flow
  4. "The issue is that getUserById returns null when the user
      doesn't exist, but getProfile doesn't check for null.
      Here's the fix..." → implements null check
  5. Adds a test for the null case
```

### Workflow 2: Test Failure Investigation

```
Developer: "npm test is failing. Fix it."

Agent:
  1. Runs npm test → reads output
  2. Identifies which tests fail and the error messages
  3. Reads the failing test files
  4. Reads the source code under test
  5. Determines whether the test or the code is wrong
  6. Fixes the actual issue
  7. Reruns tests to verify
```

### Workflow 3: Reproduce and Fix

```
Developer: "Users report that search returns no results when they
            use special characters in the query."

Agent:
  1. Reads the search implementation
  2. Identifies the regex pattern that fails on special characters
  3. Writes a test that reproduces the bug
  4. Runs it → confirms failure
  5. Fixes the regex escaping
  6. Reruns test → passes
  7. Runs full test suite → all green
```

### Workflow 4: Performance Debugging

```
Developer: "The /api/orders endpoint is taking 5 seconds. It should be <200ms."

Agent:
  1. Reads the endpoint handler
  2. Identifies N+1 query pattern in the ORM
  3. Reads the database schema
  4. Proposes eager loading / join optimization
  5. Implements the fix
  6. "To verify the performance improvement, run:
      time curl localhost:3000/api/orders"
```

## AI Debugging Capabilities by Tool

| Tool | Debugging Strengths |
|---|---|
| [[Claude Code]] | Reads files + runs commands + extended thinking for complex traces. Sub-agents can investigate multiple hypotheses in parallel. |
| [[OpenAI Codex]] | Sandboxed execution means the agent can safely run and reproduce issues. Multimodal: paste error screenshots. |
| [[OpenCode]] | LSP diagnostics feed real-time type errors back to the AI. Provider-agnostic — use the best model for the task. |
| [[AI Coding Landscape\|Cursor]] | Inline error highlighting, auto-fix suggestions, full codebase context via embeddings. |
| [[AI Coding Landscape\|Cline]] | Browser control — can reproduce frontend bugs by navigating the UI. |

## What AI Catches Well

| Bug Type | AI Effectiveness | Why |
|---|---|---|
| **Null/undefined errors** | Excellent | Common pattern, easy to trace |
| **Type mismatches** | Excellent | Can read type definitions, LSP integration |
| **Off-by-one errors** | Good | Recognizes loop boundary patterns |
| **Missing error handling** | Excellent | Identifies unhandled promise rejections, uncaught exceptions |
| **SQL injection** | Excellent | Trained on security vulnerability patterns |
| **Race conditions** | Moderate | Can identify concurrent access but may miss subtle timing |
| **Memory leaks** | Moderate | Needs profiling data to identify |
| **Business logic errors** | Weak | Doesn't know the business rules |
| **Intermittent failures** | Weak | Needs runtime observation, not just code reading |

## The Rubber Duck Effect (Enhanced)

The classic "rubber duck debugging" — explaining a bug to an inanimate object helps you see the solution — becomes supercharged with AI:

```
Developer: "Let me explain what's happening. The request hits the middleware,
            then goes to the controller, which calls the service, which..."

AI: "I notice you said the middleware validates the token, but looking at
     the code, the validation middleware is only applied to /api/admin routes,
     not /api/orders. The request hits the controller without authentication."

Developer: "...oh."
```

The AI isn't just listening — it's cross-referencing your explanation against the actual code.

## Tips for AI-Assisted Debugging

1. **Provide the full error** — Stack traces, error messages, log output. Don't paraphrase.
2. **Describe what should happen** — "It should return a 200 with the user profile" helps the AI understand the expected behavior.
3. **Mention recent changes** — "This started happening after we upgraded the auth library" narrows the search.
4. **Ask for a reproduction test first** — Confirm the bug exists before fixing it.
5. **Run tests after the fix** — Always verify the fix doesn't break other things.

## Related

- [[AI Coding Workflows]]
- [[AI Pair Programming]]
- [[AI Coding Harnesses]]
- [[AI Code Review]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[Tool Use in AI]]
