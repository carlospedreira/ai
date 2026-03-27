# AI for Documentation

AI-powered documentation uses [[Large Language Models]] and [[AI Coding Harnesses|coding agents]] to generate, update, and maintain software documentation. Documentation is one of the most neglected aspects of software development — and one where AI can have outsized impact because the source of truth (the code) already exists.

## Why Documentation Is Perfect for AI

1. **The code IS the spec** — AI reads the code and produces human-readable descriptions
2. **Mechanical transformation** — Code → docs is structured, not creative
3. **Staleness problem solved** — AI can regenerate docs whenever code changes
4. **Comprehensive** — AI doesn't get bored documenting the 200th function
5. **Multi-format** — Same understanding can produce API docs, README, tutorials, comments

## Documentation Types AI Can Generate

### API Documentation

```
Developer: "Generate OpenAPI documentation for all endpoints in src/api/"

Agent:
  1. Reads every route handler
  2. Extracts: HTTP method, path, parameters, request body, response schema, status codes
  3. Generates OpenAPI 3.0 YAML
  4. Cross-references with existing types
```

### Code Comments and Docstrings

```
Developer: "Add JSDoc comments to all exported functions in src/lib/"

Agent:
  1. Reads each function
  2. Infers parameter types, return types, purpose
  3. Generates JSDoc with @param, @returns, @throws, @example
  4. Preserves existing comments, adds missing ones
```

### README Generation

```
Developer: "/init"  (in Claude Code)

Agent:
  1. Analyzes project structure, dependencies, config files
  2. Identifies the tech stack, build system, test framework
  3. Generates CLAUDE.md with project overview, build commands,
     architecture notes, conventions
```

This is exactly what [[Claude Code]]'s `/init` command does — and the resulting CLAUDE.md serves as both AI context and human documentation.

### Architecture Documentation

```
Developer: "Document the architecture of the payment system.
            Include a diagram description and data flow."

Agent:
  1. Reads all payment-related files
  2. Traces the flow: API → Controller → Service → Repository → Database
  3. Identifies external integrations (Stripe, webhooks)
  4. Produces a written architecture overview
  5. Generates a Mermaid diagram
```

### Changelog Generation

```
Developer: "Generate a changelog from the last 20 commits"

Agent:
  1. Reads git log
  2. Groups by type (feat, fix, refactor, docs)
  3. Summarizes each change in user-facing language
  4. Formats as a CHANGELOG.md entry
```

### Migration Guides

```
Developer: "We're upgrading from v2 to v3. Generate a migration guide
            covering all breaking changes."

Agent:
  1. Diffs the two versions
  2. Identifies breaking changes (removed APIs, renamed params, changed behavior)
  3. For each breaking change, documents: what changed, why, and how to update
  4. Includes code examples
```

## The Freshness Problem

Documentation's eternal enemy is staleness — docs written today are wrong tomorrow when the code changes.

### AI Solutions

| Strategy | How |
|---|---|
| **Generate on demand** | Don't persist docs; generate from code when needed |
| **CI/CD integration** | Regenerate docs on every merge (see [[AI in CI-CD]]) |
| **Drift detection** | Compare existing docs to current code; flag mismatches |
| **Doc-as-code** | Store docs alongside code; AI updates both in the same PR |

### CI/CD Documentation Pipeline

```yaml
# Regenerate API docs on every merge to main
on:
  push:
    branches: [main]
    paths: ['src/api/**']

jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Regenerate API docs
        run: |
          claude -p --bare "Update docs/api.yaml to match the current
          state of src/api/. Only update what changed." \
          --allowedTools "Read,Write,Glob,Grep"
      - name: Commit if changed
        run: |
          git diff --quiet docs/ || \
          (git add docs/ && git commit -m "docs: auto-update API documentation")
```

## Documentation Quality

### What AI Does Well
- **Accuracy** — Reads the actual code, not stale memory
- **Completeness** — Documents every function, not just "important" ones
- **Consistency** — Same style and format throughout
- **Examples** — Can generate usage examples from test code

### What AI Does Poorly
- **"Why" documentation** — Doesn't know why a design decision was made
- **Audience awareness** — May write at the wrong level for the reader
- **Prioritization** — Treats everything as equally important
- **Narrative flow** — Tutorials need storytelling; AI produces reference docs more naturally

### The 80/20 Rule
AI generates the 80% that's mechanical (what the code does). Humans add the 20% that requires judgment (why, when to use, gotchas, conceptual explanations).

## CLAUDE.md / AGENTS.md as Documentation

The project instruction files used by [[AI Coding Harnesses]] serve a dual purpose:

```
CLAUDE.md = AI context file + human onboarding document
```

A well-written CLAUDE.md is valuable documentation even for developers who don't use AI tools — it captures project conventions, architecture decisions, and development workflow.

## Related

- [[AI Coding Workflows]]
- [[AI in CI-CD]]
- [[AI Coding Harnesses]]
- [[Claude Code]]
- [[Context Engineering]]
- [[AI Pair Programming]]
