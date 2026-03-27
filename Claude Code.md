# Claude Code

Claude Code is Anthropic's [[AI Coding Harnesses|AI coding harness]] — an agentic assistant that lives in your terminal, IDE, or browser and can read, write, search, and execute code autonomously using Claude models.

## Overview

| Property | Value |
|---|---|
| Developer | Anthropic |
| Open Source | No (proprietary) |
| Built With | TypeScript (Node.js) |
| Models | Claude Sonnet 4.6, Claude Opus 4.6 |
| Platforms | Terminal (CLI), VS Code, JetBrains, Desktop App (Mac/Win), Web (claude.ai/code) |
| Pricing | Claude Pro/Max subscription or Anthropic API (usage-based) |

## How It Works

Claude Code implements the [[Agentic Coding Paradigm#The Agent Loop|agent loop]]:

1. User provides a prompt
2. Claude reasons about the task (with extended thinking)
3. Claude calls tools — reading files, running commands, editing code
4. Tool results feed back into Claude's context
5. Loop continues until the task is complete

The model has access to a rich set of built-in tools: `Read`, `Edit`, `Write`, `Bash`, `Glob`, `Grep`, `WebSearch`, `WebFetch`, and more. Additional tools can be added via [[Model Context Protocol|MCP servers]].

## Key Features

### CLAUDE.md Files
Project-specific instruction files placed in the repository root (or subdirectories). Claude Code reads these automatically and uses them as persistent context — coding standards, project conventions, build commands, architecture notes.

Scoping:
- `CLAUDE.md` in project root — project-wide instructions
- `CLAUDE.md` in subdirectories — scoped to that path
- `~/.claude/CLAUDE.md` — global instructions for all projects

### Permissions Model
Claude Code uses a tiered [[Approval Flows and Sandboxing|permission system]]:
- **Allow** — Tool runs without asking
- **Ask** — User must approve each invocation
- **Deny** — Tool is blocked

Permissions can be configured globally, per-project, or per-session. Sensitive operations (file writes, bash commands) require approval by default.

### Hooks
Shell commands that execute in response to Claude Code events:
- Pre/post tool execution
- On session start/end
- On prompt submission

Hooks enable custom workflows: linting before commits, notifications, logging, automated validation.

### Slash Commands
Built-in commands like `/help`, `/clear`, `/compact`, `/permissions`, and user-defined commands in `.claude/commands/` for reusable workflows.

### Extended Thinking
Claude Code uses Claude's extended thinking capability — the model reasons internally before acting, producing more coherent multi-step plans. This is especially powerful for complex refactoring and debugging tasks.

### Sub-Agents
Claude Code can spawn sub-agents for parallelized work — exploring different parts of a codebase simultaneously, running searches in the background, or delegating specialized tasks.

## Configuration

| File | Purpose |
|---|---|
| `CLAUDE.md` | Project instructions for the model |
| `~/.claude/settings.json` | Global settings (permissions, defaults) |
| `.claude/settings.json` | Project-level settings |
| `~/.claude/keybindings.json` | Custom keyboard shortcuts |
| `.claude/commands/` | Custom slash commands |

## SDK and Programmatic Use

Claude Code can be used programmatically:
- **Headless mode** — Run without interactive UI for CI/CD and automation
- **Claude Code SDK** — Build custom agents on top of Claude Code's tool infrastructure
- **GitHub Actions** — Integrate into PR workflows

## MCP Integration

Claude Code was one of the first tools to fully support [[Model Context Protocol]] (MCP), allowing external tool servers to be connected:
- Database access
- API integration
- Custom search engines
- Jira, Confluence, Slack, and other services

MCP servers are configured in `settings.json` and their tools become available to Claude during the session.

## Strengths

- Deep integration with Claude's reasoning capabilities
- Rich permission model for enterprise use
- CLAUDE.md for persistent project context
- Hooks for custom automation
- Multi-platform: terminal + IDE + web + desktop

## Limitations

- Locked to Claude models (no model choice)
- Proprietary — cannot self-host or modify
- API costs can be significant for heavy use

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[Model Context Protocol]]
- [[Approval Flows and Sandboxing]]
- [[Context Management]]
