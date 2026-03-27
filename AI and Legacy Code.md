# AI and Legacy Code

Legacy code — old, poorly documented, often untested codebases that organizations depend on but fear to change — is one of the most impactful domains for [[AI Coding Harnesses]]. AI agents can read, understand, document, test, and modernize legacy systems that would take human developers weeks of archaeology to approach.

## Why AI Excels with Legacy Code

| Legacy Code Problem | AI Advantage |
|---|---|
| "Nobody knows what this does" | AI reads and explains code without institutional knowledge |
| "There are no tests" | AI generates tests from code behavior (see [[AI-Powered Testing]]) |
| "There's no documentation" | AI generates docs from code (see [[AI for Documentation]]) |
| "It's in a language nobody knows" | AI reads COBOL, Fortran, Perl, VB6 as easily as Python |
| "Nobody wants to touch it" | AI doesn't fear breaking things (with proper [[Approval Flows and Sandboxing\|safeguards]]) |
| "It's 500K lines" | AI can process entire codebases systematically |

## Workflows

### 1. Codebase Archaeology

```
Developer: "Explain what this system does. Start with the entry points
            and trace the major flows."

Agent:
  1. Identifies entry points (main(), HTTP handlers, CLI commands)
  2. Traces execution flow through the codebase
  3. Maps dependencies between modules
  4. Identifies external integrations (databases, APIs, files)
  5. Produces an architecture overview with diagrams
```

This alone can save days of manual code reading.

### 2. Test Harness Generation

The first step before any modification — create a safety net:

```
Agent:
  1. Reads the code to understand behavior
  2. Generates characterization tests (tests that capture current behavior,
     whether that behavior is "correct" or not)
  3. Runs tests to establish the baseline
  4. Now any change that breaks existing behavior is detected
```

Characterization tests don't assert correctness — they assert *consistency*. If the legacy system returns 42 for input X, the test asserts 42. This catches unintended changes during modernization.

### 3. Incremental Modernization

The strangler fig pattern, powered by AI:

```
Phase 1: AI wraps legacy code in a modern API layer
         (old code still runs, new interface on top)

Phase 2: AI implements modern replacements for each module
         (both old and new exist, feature-flagged)

Phase 3: AI migrates consumers to the new implementation
         (tests verify behavioral equivalence)

Phase 4: AI removes legacy code
         (new implementation is the only one)
```

See [[AI Code Migration]] for the detailed migration workflow.

### 4. Language Translation

```
Developer: "Convert this COBOL payroll system to Python"

Agent:
  1. Reads the COBOL source
  2. Identifies the business logic (calculations, rules, conditions)
  3. Translates to Python, preserving exact behavior
  4. Generates tests based on the COBOL behavior
  5. Verifies Python output matches COBOL for test cases
```

### 5. Dependency Modernization

```
Developer: "This system uses jQuery 1.x and Express 3.
            Bring it to current versions."

Agent:
  1. Audits all deprecated APIs being used
  2. Creates a migration plan ordered by dependency chain
  3. Upgrades one dependency at a time
  4. Runs tests after each upgrade
  5. Handles breaking changes per the library's migration guide
```

## The Context Challenge

Legacy codebases are often too large for a single [[Context Management|context window]]:

| Codebase Size | Approach |
|---|---|
| < 50K lines | May fit in context with selective reading |
| 50K–500K lines | Agent explores on-demand; [[Claude Code]] sub-agents parallelize |
| 500K+ lines | Module-by-module approach; index with embeddings ([[Semantic Search]]) |

### Strategies

- **Top-down exploration** — Start from entry points, trace inward
- **Grep-driven** — Search for specific patterns (error messages, function names)
- **Dependency graph** — Map imports/includes to understand module boundaries
- **Incremental context** — Work on one module at a time, compact between modules

## What AI Handles Well

| Task | AI Quality |
|---|---|
| **Reading and explaining code** | Excellent — even obscure languages |
| **Generating characterization tests** | Good — captures observable behavior |
| **Identifying dead code** | Good — traces reachability from entry points |
| **Simple refactoring** | Good — rename, extract, restructure (see [[AI-Assisted Refactoring]]) |
| **Documentation generation** | Excellent — reads code, produces docs |
| **Dependency updates** | Good — follows migration guides |

## What AI Struggles With

| Challenge | Why |
|---|---|
| **Undocumented business rules** | "Why does it multiply by 1.07 on Tuesdays?" — only a domain expert knows |
| **Environmental dependencies** | Legacy systems often depend on specific OS versions, file paths, network configs |
| **Data migration** | Schema changes require understanding data semantics |
| **Implicit behavior** | Side effects, global state, order-dependent initialization |
| **Build system archaeology** | Ancient Makefiles, custom build scripts, undocumented environment setup |

## Real-World Impact

Legacy modernization is one of the largest potential markets for AI coding:
- **$300B+** spent annually on legacy system maintenance (industry estimates)
- Major enterprises run critical systems on COBOL (banks, airlines, government)
- AI makes modernization economically viable for systems previously "too expensive to touch"
- Government agencies exploring AI-assisted COBOL → modern language translation

## Best Practices

1. **Don't change behavior first** — Understand and test before modifying
2. **Characterization tests before anything else** — Your safety net
3. **Incremental, not big bang** — Modernize one module at a time
4. **Keep the old code running** — Strangler fig, not full replacement
5. **Document as you go** — AI-generated docs are a valuable byproduct
6. **Involve domain experts** — AI reads code; humans know the business rules

## Related

- [[AI Code Migration]]
- [[AI-Assisted Refactoring]]
- [[AI-Powered Testing]]
- [[AI for Documentation]]
- [[AI Coding Workflows]]
- [[AI Coding Harnesses]]
- [[Context Management]]
- [[Semantic Search]]
