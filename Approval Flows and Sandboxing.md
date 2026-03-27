# Approval Flows and Sandboxing

Since [[AI Coding Harnesses]] can read files, write code, and execute shell commands, they need safety mechanisms to prevent unintended damage. Approval flows and sandboxing are the two main approaches.

## Approval Flows

Approval flows determine **when the agent must ask for permission** before acting.

### Common Patterns

Most harnesses offer a spectrum from fully supervised to fully autonomous:

| Level | Description | Risk |
|---|---|---|
| **Read-only** | Agent can only read files, no writes or commands | Lowest |
| **Suggest** | Agent proposes changes, user applies them | Low |
| **Auto-edit** | Agent writes files freely, asks before shell commands | Medium |
| **Full-auto** | Agent acts without asking | Highest |

### By Tool

| Harness | Approval Model |
|---|---|
| [[Claude Code]] | Per-tool permissions (allow/ask/deny) configurable in `settings.json`. User chooses which tools are auto-allowed. |
| [[OpenAI Codex]] | Policy-based: `on-request` (asks when it needs to exceed boundaries) or `never` (combined with danger-full-access). `/permissions` to switch mid-session. |
| [[OpenCode]] | Two agents: `build` (full access) and `plan` (read-only). User switches with Tab. |
| **Cursor** | Autonomy slider from Tab-complete to full agent mode |
| **Cline** | Per-action approval with "always allow" option |

### Best Practice: Progressive Trust

Start with more restrictive permissions, increase autonomy as you build confidence:
1. Begin in ask/suggest mode
2. Allow specific safe operations (file reads, grep)
3. Auto-allow file edits in your project
4. Only enable full-auto for well-understood, low-risk tasks

## Sandboxing

Sandboxing restricts **what the agent's tools can physically do**, regardless of approval.

### OpenAI Codex Sandboxing (Most Restrictive)

Codex takes the strongest stance on sandboxing:

```
┌─────────────────────────────────┐
│         Sandbox Boundary        │
│  ┌───────────────────────────┐  │
│  │  Project Directory (R/W)  │  │
│  └───────────────────────────┘  │
│  ┌───────────────────────────┐  │
│  │  System Files (Read-only) │  │
│  └───────────────────────────┘  │
│  ┌───────────────────────────┐  │
│  │  Network: DISABLED        │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

- **Default mode (`workspace-write`)**: File writes restricted to project directory. Network access is completely disabled during command execution.
- **`danger-full-access`**: Removes all restrictions. Explicit opt-in.

### Claude Code Sandboxing

Claude Code uses a permission-based model rather than a hard sandbox:
- File operations require approval (unless pre-allowed)
- Bash commands require approval (unless pre-allowed)
- No network isolation by default
- Hooks can add custom validation before tool execution

### OpenCode Sandboxing

OpenCode relies on the `build`/`plan` agent split:
- `plan` agent: strictly read-only
- `build` agent: full access (no hard sandbox)
- No built-in network isolation

## Comparison

| Feature | Claude Code | Codex | OpenCode |
|---|---|---|---|
| File write control | Permission-based | Sandbox directory | Agent-based |
| Network isolation | No | Yes (default) | No |
| Shell command control | Permission-based | Sandbox | Agent-based |
| Configurable | Highly (per-tool) | Moderate (modes) | Basic (two agents) |
| Custom validation | Yes (hooks) | Limited | Limited |

## Why It Matters

Without proper guardrails, a coding agent could:
- Delete important files
- Overwrite uncommitted changes
- Execute destructive commands (`rm -rf`, `git push --force`)
- Leak secrets via network requests
- Install malicious packages

The right balance depends on the use case:
- **Learning / exploring**: Read-only or suggest mode
- **Active development**: Auto-edit with command approval
- **CI/CD automation**: Full-auto in a disposable environment
- **Production systems**: Maximum restrictions

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[AI Ethics and Safety]]
