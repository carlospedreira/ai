# AI in CI/CD

Integrating [[AI Coding Harnesses|AI coding agents]] into Continuous Integration and Continuous Deployment pipelines enables automated code review, test generation, bug fixing, and even autonomous feature development — all triggered by commits, PRs, or scheduled jobs.

## Why AI in CI/CD?

Traditional CI/CD runs deterministic checks: build, lint, test. AI adds *judgment*:
- Is this PR introducing a security vulnerability?
- Are there edge cases the tests don't cover?
- Can the failing test be automatically fixed?
- What's the impact of this change on the broader system?

## Integration Points

```
Push/PR → Build → Lint → Test → AI Review → AI Fix → Deploy
                                    ↑            ↑
                              New with AI   New with AI
```

### PR Review (Post-Push)

Trigger AI review when a PR is opened or updated:

```yaml
# GitHub Actions - Claude Code review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  ai-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: AI Review
        run: |
          gh pr diff ${{ github.event.pull_request.number }} | \
          claude -p --bare "Review this PR for security issues, bugs, and style violations. Post findings as PR comments." \
          --allowedTools "Bash(gh *)"
```

### Automated Bug Fixing

When tests fail, let the agent fix them:

```yaml
on:
  workflow_run:
    workflows: ["Tests"]
    types: [completed]
    branches: [main]

jobs:
  auto-fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Fix failing tests
        run: |
          claude -p --bare "The CI tests are failing. Investigate and fix the issue. Run tests to verify." \
          --allowedTools "Read,Edit,Bash(npm test *),Bash(git *)"
      - name: Create fix PR
        run: |
          git checkout -b fix/auto-$(date +%s)
          git add -A && git commit -m "fix: auto-fix failing tests"
          gh pr create --title "fix: auto-fix CI failure" --body "Automated fix by AI agent"
```

### Issue-to-PR (Autonomous Agents)

Assign issues to AI and get PRs back:

| Tool | How |
|---|---|
| [[Claude Code]] | GitHub Actions integration, Slack triggers |
| [[OpenAI Codex]] | Cloud Codex assigns from issues, GitHub Action |
| [[OpenCode]] | Comment `/opencode` on issues |
| **GitHub Copilot** | "Coding Agent" — assign issues directly to Copilot |

### Test Generation

Generate tests for uncovered code as part of CI:

```bash
claude -p --bare \
  "Generate unit tests for all functions in src/payments/ that don't have tests yet. Use our existing test patterns from __tests__/." \
  --allowedTools "Read,Write,Bash(npm test *)"
```

### Documentation Generation

Auto-generate or update docs when code changes:

```bash
# Generate OpenAPI docs from code changes
git diff main --name-only -- 'src/api/**' | \
claude -p --bare "These API files changed. Update the OpenAPI spec in docs/api.yaml to match."
```

## Headless Mode Flags

All major harnesses support non-interactive execution for CI:

| Harness | Headless Command | Key Flags |
|---|---|---|
| [[Claude Code]] | `claude -p "prompt"` | `--bare`, `--allowedTools`, `--output-format json`, `--max-turns` |
| [[OpenAI Codex]] | `codex -p "prompt"` | `--full-auto`, `--model`, `--approval-mode` |
| [[OpenCode]] | `opencode run "prompt"` | `--auto`, `-m "prompt"` |
| **Aider** | `aider --message "prompt"` | `--yes`, `--auto-commits` |

Use `--bare` (Claude Code) or equivalent to skip interactive features, MCP discovery, and local config in CI environments.

## Safety Considerations

Running AI agents in CI/CD introduces risks:

### The Good
- Runs in disposable containers (natural sandboxing)
- Changes are reviewed via PRs (human-in-the-loop)
- Compute is bounded by CI timeout

### The Dangerous
- **Secret exposure** — CI environments have access tokens, API keys
- **[[Prompt Injection]]** — Malicious code in PRs could inject instructions into the agent
- **Unbounded actions** — Without `--max-turns`, an agent could loop indefinitely
- **Cost** — Token usage can spike unexpectedly on large diffs

### Best Practices
1. **Minimize secrets** — Only expose what the agent needs
2. **Use `--bare` mode** — Don't load local hooks or MCP servers
3. **Set `--max-turns`** — Bound the agent's iteration count
4. **Restrict tools** — Only allow necessary tools via `--allowedTools`
5. **Review outputs** — Always send changes through PR review, not direct merge
6. **Budget alerts** — Set spending limits on API keys used in CI

## Architecture Patterns

### Reviewer Agent
```
PR opened → Agent reviews → Posts comments → Human decides
```
Lowest risk. Agent only reads and comments.

### Fixer Agent
```
Tests fail → Agent investigates → Agent fixes → Creates PR → Human reviews
```
Medium risk. Agent can write code but not deploy.

### Autonomous Agent
```
Issue created → Agent implements → Agent tests → Agent opens PR → Auto-merge if tests pass
```
Highest risk. Requires strong test coverage and monitoring.

## Related

- [[AI Coding Workflows]]
- [[AI Code Review]]
- [[AI Coding Harnesses]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[Approval Flows and Sandboxing]]
- [[Prompt Injection]]
