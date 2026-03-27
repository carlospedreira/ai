# AI Code Migration

AI code migration uses [[AI Coding Harnesses|coding agents]] to systematically transform codebases between languages, frameworks, APIs, or versions. Migrations are one of the highest-ROI applications of AI — they are tedious, error-prone, and mechanical for humans, but agents can execute them systematically at scale.

## Types of Migrations

### Language Migration
Convert an entire codebase from one language to another:
```
Python Flask app → TypeScript Express app
Java Spring Boot → Kotlin Spring Boot
JavaScript → TypeScript (adding types)
```

### Framework Migration
Switch frameworks within the same language:
```
Express → Fastify
React class components → React hooks
Vue 2 Options API → Vue 3 Composition API
Angular → React
jQuery → vanilla JavaScript
```

### API/Library Migration
Replace deprecated or outdated dependencies:
```
Moment.js → Day.js
Request → Axios/Fetch
Lodash → native JavaScript methods
Webpack → Vite
Jest → Vitest
```

### Version Upgrade
Handle breaking changes between major versions:
```
Python 2 → Python 3
React 17 → React 18 (concurrent mode)
Next.js 13 → Next.js 14 (app router)
Express 4 → Express 5
Node.js 16 → Node.js 22
```

### Database Migration
Change the data layer:
```
MongoDB → PostgreSQL
Raw SQL → ORM (Prisma, Drizzle)
REST API → GraphQL
Sequelize → Prisma
```

## The AI Migration Workflow

### Phase 1: Audit
```
Agent (read-only):
  1. Scan the codebase for all usages of the old API/framework
  2. Categorize changes by type and complexity
  3. Identify edge cases and custom workarounds
  4. Produce a migration report with effort estimates
```

### Phase 2: Reference Implementation
```
Agent:
  1. Migrate ONE representative file/module completely
  2. Run tests to verify correctness
  3. Developer reviews the approach
  4. This becomes the pattern for the rest
```

### Phase 3: Bulk Migration
```
Agent:
  1. Apply the same pattern across all remaining files
  2. Run tests after each file/batch
  3. Handle edge cases that differ from the reference
  4. Track progress against the audit report
```

### Phase 4: Cleanup
```
Agent:
  1. Remove old dependencies from package.json
  2. Delete compatibility shims and wrappers
  3. Update documentation and README
  4. Run full test suite
  5. Run linter on the entire codebase
```

## Using Sub-Agents for Migration

Large migrations benefit from [[AI Agents and Sub-Agents|parallel sub-agents]]:

```
Parent Agent: "Migrate from Moment.js to Day.js"
    │
    ├── Explore Agent: Find all Moment.js imports and usages
    │
    ├── Plan Agent: Create mapping of Moment APIs → Day.js equivalents
    │
    ├── Agent 1: Migrate src/utils/ (in worktree A)
    ├── Agent 2: Migrate src/services/ (in worktree B)
    ├── Agent 3: Migrate src/controllers/ (in worktree C)
    │
    └── Test Agent: Run full suite, report failures
```

[[Claude Code]] supports this natively with worktree isolation — each sub-agent works in its own git worktree, preventing conflicts.

## What AI Handles Well

| Migration Type | AI Quality | Why |
|---|---|---|
| **API rename** (A.foo() → B.bar()) | Excellent | Mechanical find-and-replace with context |
| **Syntax transformation** | Excellent | Patterns are consistent |
| **Type addition** (JS → TS) | Good | Can infer types from usage patterns |
| **Framework equivalent** | Good | Knows both frameworks from training data |
| **Idiomatic rewrite** | Moderate | Can translate literally but may miss idioms |
| **Architecture change** | Weak | Requires design judgment |

## What AI Struggles With

| Challenge | Example |
|---|---|
| **Semantic equivalence** | Old library's edge case behavior doesn't match new library |
| **Implicit dependencies** | Code relies on undocumented behavior of the old framework |
| **Configuration migration** | Build tool configs are highly project-specific |
| **Data migration** | Schema changes need careful planning beyond code |
| **Custom patches** | The team patched the old library; the new one doesn't support it |

## Codemods vs AI

Traditional codemods (jscodeshift, ts-morph) use AST transformations:
```javascript
// Codemod: Replace require() with import
// Operates on the AST, guaranteed syntactic correctness
```

AI migration operates on *meaning*, not just syntax:
```
// AI: Replace the Express middleware pattern with Fastify's plugin pattern
// Understands the semantic difference, not just syntax
```

**Best approach:** Combine both. Use codemods for mechanical syntax transforms; use AI for semantic migrations that require understanding.

## Safety Net

Migrations are high-risk. The safety net:

1. **Tests before starting** — If test coverage is low, [[AI-Powered Testing|generate tests first]]
2. **Incremental migration** — Migrate one module at a time, verify each
3. **Git checkpoints** — Commit after each successful batch
4. **Dual running** — Keep old and new code paths; switch gradually
5. **Feature flags** — Toggle between old and new implementations
6. **Rollback plan** — If something goes wrong, `git revert` to the last good checkpoint

## Real-World Examples

| Migration | Approach |
|---|---|
| **Stripe: Python 2 → 3** | Automated codemods + AI for complex cases |
| **Large codebases: CJS → ESM** | AI handles import/export conversion, dynamic requires |
| **React class → hooks** | AI understands lifecycle mapping (componentDidMount → useEffect) |
| **REST → GraphQL** | AI generates schema from existing endpoints, creates resolvers |

## Related

- [[AI Coding Workflows]]
- [[AI-Assisted Refactoring]]
- [[AI Agents and Sub-Agents]]
- [[AI Coding Harnesses]]
- [[AI-Powered Testing]]
- [[AI and Version Control]]
- [[Claude Code]]
