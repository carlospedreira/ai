# AI Code Review

AI code review is the use of [[Large Language Models]] and [[AI Coding Harnesses]] to automatically review code changes — finding bugs, security issues, style violations, and design problems before a human reviewer sees them.

## Why AI Code Review

Traditional code review is:
- **Slow** — Waiting for reviewer availability is a top bottleneck
- **Inconsistent** — Different reviewers catch different things
- **Exhausting** — Reviewer fatigue leads to rubber-stamping large PRs

AI can provide instant, comprehensive, tireless first-pass review.

## How It Works

### Inline PR Review

The AI reads the diff (or full PR), then comments on specific lines:

```
PR Diff → AI reads changes → AI posts comments:
  - Line 42: SQL injection risk — use parameterized query
  - Line 87: This loop is O(n²), consider a hash map
  - Line 103: Missing null check, will crash on empty input
```

### Full-Context Review

More advanced approaches load the entire codebase for context, not just the diff:
- Understands how the changed code fits into the broader architecture
- Detects when a change breaks an invariant elsewhere
- Identifies missing test coverage

### Automated Fix Suggestions

Beyond flagging issues, AI can propose fixes:
```diff
- query = "SELECT * FROM users WHERE name = '" + name + "'"
+ query = "SELECT * FROM users WHERE name = $1"
+ params = [name]
```

## Tools for AI Code Review

### Built Into Coding Agents

| Tool | Review Capability |
|---|---|
| [[Claude Code]] | `/review` custom command, GitHub Actions integration, PR diff piping |
| [[OpenAI Codex]] | Cloud Codex runs code reviews; Codex Security for security scanning |
| [[OpenCode]] | GitHub comment triggers (`/opencode review`) |
| **GitHub Copilot** | PR review summaries, inline suggestions, "Coding Agent" for autonomous review |

### Standalone Review Tools

| Tool | Approach |
|---|---|
| **CodeRabbit** | AI-powered PR review bot for GitHub/GitLab |
| **Sourcery** | Python-focused AI review |
| **Codex Security** | OpenAI's specialized security review agent |
| **Greptile** | Codebase-aware AI review |

### DIY with Harnesses

```bash
# Claude Code: review a PR
gh pr diff 123 | claude -p "Review this PR for:
- Security vulnerabilities
- Performance issues
- Missing error handling
- Style violations per our CLAUDE.md guidelines"
```

## What AI Catches Well

- **Security vulnerabilities** — Injection, XSS, hardcoded secrets, insecure defaults
- **Common bugs** — Null dereferences, off-by-one errors, race conditions
- **Style violations** — Naming conventions, formatting, import order
- **Missing tests** — Identifies untested code paths
- **Documentation gaps** — Missing docstrings, outdated comments
- **Performance anti-patterns** — N+1 queries, unnecessary allocations

## What AI Misses

- **Business logic correctness** — AI doesn't know if the feature matches the spec
- **Architectural fitness** — Whether the approach fits the system's long-term design
- **Subtle concurrency bugs** — Complex timing issues in distributed systems
- **Context-dependent decisions** — Trade-offs that require domain expertise

## Best Practice: AI + Human Review

The emerging pattern is layered review:

```
1. AI automated review (instant)
   ├── Security scan
   ├── Style/lint check
   └── Bug pattern detection
2. Human review (focused)
   ├── Business logic correctness
   ├── Architecture fit
   └── Approval / merge decision
```

AI handles the tedious, comprehensive first pass. Humans focus on judgment calls.

## Metrics

- **Stripe** internally generates and merges 1,000+ AI-created PRs per week
- [[OpenAI Codex|Codex Security]] scanned 1.2M+ commits, identified 792 critical findings with 84% noise reduction
- AI review typically catches 60–80% of issues a human reviewer would find

## Related

- [[AI Coding Workflows]]
- [[AI Coding Harnesses]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[AI Ethics and Safety]]
