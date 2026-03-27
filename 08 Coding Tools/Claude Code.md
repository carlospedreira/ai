# Claude Code

Claude Code is Anthropic's [[AI Coding Harnesses|AI coding harness]] — an agentic assistant that lives in your terminal, IDE, or browser and can read, write, search, and execute code autonomously using Claude models. It launched as a research preview in February 2025 and reached general availability in May 2025.

## Overview

| Property | Value |
|---|---|
| Developer | Anthropic |
| Open Source | No (source-available on GitHub, proprietary license) |
| Built With | TypeScript (Node.js) |
| Models | Claude Opus 4.6, Sonnet 4.6, Haiku 4.5 |
| Context Window | Up to 1M tokens (Opus and Sonnet) |
| Platforms | Terminal CLI, VS Code, JetBrains, Desktop App (Mac/Win), Web (claude.ai/code) |
| Pricing | Pro ($20/mo), Max ($100–$200/mo), Team, Enterprise, or API pay-per-token |
| Adoption | 350K+ daily active users, 1M+ accepted PRs (early 2026) |

## How It Works

Claude Code implements the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] as a single-threaded master loop (internally codenamed "nO"):

1. Call Claude with current context (prompt + tool definitions + conversation history)
2. If response contains tool calls → execute them
3. Feed results back into context
4. Repeat until Claude produces a text-only response (no tool calls)

A typical task runs 5–50 iterations. A real-time steering system allows users to inject new instructions mid-loop without restarting.

### Three Phases Per Task

Every task blends through:
1. **Gather context** — Read files, search patterns, examine git state
2. **Take action** — Edit files, run shell commands, create commits
3. **Verify results** — Run tests, check output, re-read modified files

### Context Management

- Context compression triggers automatically at ~92% utilization
- `/compact` manually compresses conversation history into summaries
- `/context` shows what is consuming context window space
- Older tool outputs are cleared first, then conversation is summarized

## Built-In Tools

| Category | Tools |
|---|---|
| File I/O | `Read`, `Edit`, `Write`, `Glob` |
| Search | `Grep`, symbol search |
| Execution | `Bash` (with command risk classification) |
| Web | `WebSearch`, `WebFetch` |
| Agents | `Agent` (spawn sub-agents), `AskUserQuestion` |
| Tasks | `TaskCreate`, `TaskUpdate` |

Additional tools via [[Model Context Protocol|MCP servers]].

## Key Features

### CLAUDE.md Files

Project-specific instruction files loaded automatically at session start. They walk up the directory tree from the working directory.

| Scope | Location |
|---|---|
| Managed (IT-deployed) | `/Library/Application Support/ClaudeCode/CLAUDE.md` (macOS) |
| Project (team, git-committed) | `./CLAUDE.md` or `./.claude/CLAUDE.md` |
| User (personal, all projects) | `~/.claude/CLAUDE.md` |

Path-scoped rules can be placed in `.claude/rules/*.md` with YAML frontmatter specifying `paths:` globs — these load only when Claude works with matching files.

Best practice: keep CLAUDE.md under 200 lines. Longer files reduce adherence.

### Auto Memory

Claude writes its own notes to `~/.claude/projects/<project>/memory/MEMORY.md` across sessions. The first 200 lines load at every session start. Contains build commands, debugging insights, coding patterns. Toggle with `/memory`.

### Permission Modes

Five modes, cycled with `Shift+Tab`:

| Mode | Behavior |
|---|---|
| `default` | Prompts for file edits and shell commands |
| `acceptEdits` | Auto-approves file edits; still asks for commands |
| `plan` | Read-only — Claude plans but cannot modify or execute |
| `auto` | AI safety classifier auto-approves routine actions (Team+ only, research preview) |
| `bypassPermissions` | Skips all prompts except writes to `.git`, `.claude`, `.vscode`, `.idea` |

Permission rules use `Tool(specifier)` syntax with glob support:
```
Bash(npm run test *)     → allows test commands
Read(./.env)             → blocks reading .env
mcp__github__*           → allows all GitHub MCP tools
```

**Deny beats Ask beats Allow.** A deny rule cannot be overridden by an allow anywhere.

### Hooks

User-defined commands that run at deterministic lifecycle points — guaranteed enforcement that doesn't rely on the LLM.

**Key hook events:**

| Event | When |
|---|---|
| `PreToolUse` | Before a tool call — can block it |
| `PostToolUse` | After a tool call succeeds |
| `UserPromptSubmit` | When you submit a prompt |
| `Stop` | When Claude finishes responding |
| `SessionStart` / `SessionEnd` | Session lifecycle |
| `SubagentStart` / `SubagentStop` | Sub-agent lifecycle |
| `PreCompact` / `PostCompact` | Context compaction |
| `FileChanged` | When a watched file changes on disk |
| `TeammateIdle` | Agent team coordination |

**Hook types:** `command` (shell), `http` (POST to URL), `prompt` (single-turn LLM eval via Haiku), `agent` (full sub-agent with tool access).

Example — auto-format after edits:
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{ "type": "command", "command": "npx prettier --write $(jq -r '.tool_input.file_path')" }]
    }]
  }
}
```

### Slash Commands

| Command | Purpose |
|---|---|
| `/model <name>` | Switch model mid-session |
| `/compact` | Compress context window |
| `/context` | View context usage |
| `/effort low\|medium\|high\|max` | Set reasoning effort |
| `/memory` | View/edit auto-memory |
| `/init` | Generate CLAUDE.md for project |
| `/mcp` | Manage MCP servers |
| `/permissions` | View permission rules |
| `/agents` | Manage sub-agents |
| `/btw` | Side question, discarded from history |

Custom slash commands: create `.claude/commands/` with `.md` files. Filename becomes command name.

### Extended Thinking and Effort

Claude's extended thinking produces internal reasoning before acting. Controlled via effort levels:

| Level | Behavior |
|---|---|
| `low` | Faster, cheaper, straightforward tasks |
| `medium` | Default — balanced |
| `high` | Deeper reasoning for complex problems |
| `max` | Unconstrained thinking budget (Opus only, doesn't persist) |

Including the word **"ultrathink"** in any prompt triggers high effort for that single turn.

### Sub-Agents

See [[AI Agents and Sub-Agents]] for the full deep dive.

Built-in sub-agents:

| Agent | Model | Access | Purpose |
|---|---|---|---|
| **Explore** | Haiku | Read-only | Fast codebase search |
| **Plan** | Inherited | Read-only | Research and planning |
| **General-purpose** | Inherited | Full | Complex multi-step tasks |

Custom sub-agents: define as `.md` files with YAML frontmatter in `.claude/agents/` (project) or `~/.claude/agents/` (global).

Sub-agents run in their own context window. Can run in background (`Ctrl+B`). Can use isolated git worktrees (`isolation: worktree`).

### Agent Teams (Opus 4.6)

Multiple Claude Code agents coordinating in parallel, each with fully independent sessions. Teammates communicate via `SendMessage` tool. Approximately 3x token consumption for 3-agent setups.

## Configuration

### Settings Hierarchy (highest to lowest priority)

| Scope | File | Shared? |
|---|---|---|
| Managed (IT) | `managed-settings.json` in system dirs | Yes |
| Local | `.claude/settings.local.json` | No (gitignored) |
| Project | `.claude/settings.json` | Yes (committed) |
| User | `~/.claude/settings.json` | No |

MCP servers: `~/.claude.json` (user) and `.mcp.json` (project).

## SDK and Programmatic Use

### Headless Mode (`-p` flag)
```bash
# Non-interactive execution
claude -p "Fix the failing tests" --allowedTools "Read,Edit,Bash"

# Structured JSON output
claude -p "Analyze auth.py" --output-format json --json-schema '{...}'

# Pipe integration
git diff main | claude -p "Review for security issues"

# Multi-step sessions
session_id=$(claude -p "Start review" --output-format json | jq -r '.session_id')
claude -p "Continue review" --resume "$session_id"
```

Use `--bare` in CI/CD to skip hooks, MCP, and auto-memory discovery.

### Claude Agent SDK

Python and TypeScript library for programmatic access to the full Claude Code engine:

```python
from claude_agent_sdk import query, ClaudeAgentOptions

async for message in query(
    prompt="Find and fix the bug in auth.py",
    options=ClaudeAgentOptions(allowed_tools=["Read", "Edit", "Bash"]),
):
    print(message)
```

Under the hood, spawns the `claude` CLI as a subprocess and communicates via JSON-lines.

## Platforms

| Platform | Key Features |
|---|---|
| **Terminal (CLI)** | Full features, all commands, piping support |
| **VS Code** | Inline diffs, plan review, checkpointing, `@`-mentions, multiple tabs |
| **JetBrains** | Native diff viewer, selection context sharing |
| **Desktop App** | Visual diff review, multiple sessions, scheduled tasks |
| **Web (claude.ai/code)** | No local setup, cloud VMs, long-running tasks, `/teleport` to terminal |
| **GitHub Actions** | Automated PR review, issue triage |
| **Slack** | `@Claude` creates PRs from bug reports |

## Pricing

| Plan | Price | Default Model | Usage |
|---|---|---|---|
| Pro | $20/mo | Sonnet 4.6 | ~44K tokens / 5-hour window |
| Max 5x | $100/mo | Opus 4.6 | ~88K tokens / 5-hour window |
| Max 20x | $200/mo | Opus 4.6 | ~220K tokens / 5-hour window |
| API | Pay-per-token | Any Claude model | Sonnet: $3/$15 per MTok in/out |

## Strengths

- Deep integration with Claude's reasoning (extended thinking, adaptive effort)
- Richest permission and hook system for enterprise governance
- CLAUDE.md + auto-memory for persistent project context
- Agent teams for parallel autonomous work
- Multi-platform: terminal + IDE + web + desktop + Slack + GitHub

## Limitations

- Locked to Claude models (no model choice)
- Proprietary — cannot self-host or deeply modify
- Token costs add up with agent teams and extended thinking
- Auto mode still in research preview

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[AI Agents and Sub-Agents]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[Model Context Protocol]]
- [[Approval Flows and Sandboxing]]
- [[Context Management]]
- [[AI Coding Workflows]]
