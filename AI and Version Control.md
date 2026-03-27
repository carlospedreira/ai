# AI and Version Control

Version control — primarily Git — intersects with AI coding at every level: [[AI Coding Harnesses|agents]] create commits, manage branches, and submit pull requests; AI enhances diff review and merge conflict resolution; and version control itself needs new practices to handle AI-generated code effectively.

## How AI Agents Use Git

### Automatic Commits

[[AI Coding Landscape|Aider]] auto-commits every AI change with descriptive messages:
```
git log --oneline:
a1b2c3d Add rate limiting to login endpoint
f4e5d6c Extract PaymentGateway from OrderService
7890abc Fix null pointer in getUserById
```

Each commit is atomic and reversible — if the AI makes a mistake, `git revert` cleanly undoes it.

### Branch Management

Autonomous agents create branches for their work:
```
main
├── fix/auto-ci-failure-1711234567    (created by CI agent)
├── feat/issue-42-add-search          (created by Copilot coding agent)
└── refactor/payment-strategy         (created by Claude Code agent team)
```

### Pull Request Creation

All major harnesses can create PRs:
```bash
# Claude Code
claude -p "Implement the feature described in issue #42, create a PR"

# OpenAI Codex Cloud
# Assign issue → agent works → PR appears automatically

# OpenCode
# Comment /opencode on issue → PR created via GitHub Actions
```

### Worktree Isolation

[[Claude Code]] uses **git worktrees** for parallel agent work:
```
Main worktree:  /project/          (developer's working copy)
Agent worktree: /tmp/worktree-abc/ (agent's isolated copy)
```

The agent works in an isolated copy of the repo — its changes don't interfere with the developer's work until merged.

## AI-Enhanced Git Operations

### Commit Message Generation

```
Developer: "Commit these changes"

Agent reads the diff, generates:
  "fix(auth): handle expired JWT tokens gracefully

   Previously, expired tokens caused a 500 error. Now returns
   a 401 with a clear error message and prompts re-authentication.

   Closes #127"
```

Conventional commits format, derived from the actual diff — not a generic "update code."

### Merge Conflict Resolution

```
Developer: "Resolve the merge conflicts"

Agent:
  1. Reads both sides of the conflict
  2. Reads the git log to understand what each branch was doing
  3. Determines the correct resolution (keep both, pick one, combine)
  4. Resolves the conflict
  5. Runs tests to verify the resolution is correct
```

This is one of the most time-saving AI applications — merge conflicts are tedious for humans but mechanical for AI.

### Interactive Rebase Assistance

```
Developer: "Clean up the last 5 commits — squash the fixups, reorder logically"

Agent:
  1. Reads the commit history
  2. Identifies which commits are fixups/WIP
  3. Proposes a clean commit sequence
  4. Executes the rebase
```

### Blame and History Analysis

```
Developer: "When and why was this function changed to use async?"

Agent:
  1. Runs git blame on the function
  2. Reads the commit that introduced the change
  3. Reads the PR description and linked issue
  4. "This was changed in PR #89 by @alice on 2025-06-15 to fix
      a deadlock issue when multiple requests hit the endpoint
      simultaneously. The linked issue is #85."
```

## New Practices for AI-Generated Code

### Tagging AI-Generated Commits

Most harnesses add a co-author tag:
```
Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
```

This enables:
- Filtering AI commits in git log: `git log --grep="Co-Authored-By: Claude"`
- Tracking AI contribution percentage
- Audit trail for compliance (see [[AI Regulation]])

### Review Practices

AI-generated PRs need different review focus:

| Human-Written PR | AI-Generated PR |
|---|---|
| Review logic and approach | Verify the approach is correct for the requirement |
| Check for typos/style | Check for [[Hallucination and Grounding\|hallucinated]] APIs or imports |
| Ensure tests exist | Verify tests actually test the right thing |
| Standard review | Extra scrutiny on security-sensitive code |

### .gitignore for AI

Files that AI tools create but shouldn't be committed:

```gitignore
# AI session files
.claude/settings.local.json
.claude/memory/

# AI-generated temporary files
*.ai-draft
```

But DO commit:
```
# Team-shared AI configuration
.claude/settings.json
.claude/commands/
CLAUDE.md
AGENTS.md
```

## Git Hooks and AI

[[Claude Code]]'s hooks integrate with git workflows:

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash(git push *)",
      "hooks": [{
        "type": "command",
        "command": "echo 'Blocking push — use PR workflow instead' >&2; exit 2"
      }]
    }]
  }
}
```

Prevent agents from pushing directly; force the PR workflow.

## Measuring AI Contribution

```bash
# Count AI-authored commits
git log --grep="Co-Authored-By: Claude" --oneline | wc -l

# Percentage of AI commits
total=$(git log --oneline | wc -l)
ai=$(git log --grep="Co-Authored-By" --oneline | wc -l)
echo "$((ai * 100 / total))% of commits are AI-assisted"
```

Industry data: at some organizations, 30–50% of merged code is now AI-generated (see [[AI and Developer Productivity]]).

## Related

- [[AI Coding Harnesses]]
- [[AI Coding Workflows]]
- [[AI Code Review]]
- [[AI in CI-CD]]
- [[Autonomous Coding Agents]]
- [[Claude Code]]
- [[AI and Developer Productivity]]
