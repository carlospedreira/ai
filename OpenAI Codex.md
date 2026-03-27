# OpenAI Codex

OpenAI Codex is OpenAI's [[AI Coding Harnesses|AI coding harness]] — a lightweight, open-source coding agent that runs in your terminal. Not to be confused with the original Codex model from 2021, this is the agent product launched in 2025.

## Overview

| Property | Value |
|---|---|
| Developer | OpenAI |
| Open Source | Yes (Apache 2.0) |
| Repository | `openai/codex` on GitHub |
| Built With | Rust |
| Models | OpenAI models (o3, o4-mini, GPT-4, and successors) |
| Platforms | Terminal (CLI), VS Code, Cursor, Windsurf extensions |
| Pricing | OpenAI API usage-based; included in ChatGPT Plus/Pro/Business plans |

## Two Products, One Brand

OpenAI uses "Codex" for two related but distinct products:

### Codex CLI (Local)
The open-source terminal agent. Runs locally, calls the OpenAI API for inference, executes tools on your machine inside a sandbox.

### Codex Cloud
An autonomous agent accessible through ChatGPT. Runs in a cloud sandbox with a full copy of your repository. You assign it tasks (like Jira tickets), and it works asynchronously, creating PRs when done.

## How It Works

Codex CLI implements the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] as a state machine:

1. **Prompt assembly** — Combines user input, system instructions, AGENTS.md contents, available tools (including MCP), and local environment info
2. **Inference** — Tokens are sent to the OpenAI API; the model streams back reasoning steps and tool calls
3. **Tool execution** — File reads, writes, and shell commands run inside a [[Approval Flows and Sandboxing|sandbox]]
4. **Loop** — Results feed back to the model for the next iteration

Key technical details:
- **Stateless request handling** for Zero Data Retention compliance
- **Prompt caching** optimization for linear (not quadratic) performance
- **Automatic context compaction** when approaching window limits
- Robust handling of multi-turn conversations across hundreds of iterations

## Sandboxing

Codex's most distinctive feature is its approach to [[Approval Flows and Sandboxing|sandboxing]]. Every command runs in a constrained environment:

### Sandbox Modes

| Mode | Description |
|---|---|
| `workspace-write` | Can read files, edit within the project directory, run local commands. Network disabled. **Default.** |
| `danger-full-access` | No sandbox restrictions. Full filesystem and network access. Use with caution. |

In `workspace-write` mode:
- **Network is disabled** — No outbound connections during command execution
- **Filesystem restricted** — Can only write within the project directory
- **Configurable writable roots** — `sandbox_workspace_write.writable_roots`

### Approval Policies

| Policy | Description |
|---|---|
| `on-request` | Asks when it needs to go beyond sandbox boundaries |
| `never` | Never asks (only with `danger-full-access`) |

The `/permissions` slash command allows switching modes mid-session.

## Configuration

### AGENTS.md
Like [[Claude Code]]'s CLAUDE.md, Codex uses `AGENTS.md` files for project-specific instructions:
- How to navigate the codebase
- Which commands to run for testing
- Project conventions and standards

### Config Files
- `codex.json` — Configuration reference for sandbox_mode, approval_policy, writable_roots
- Environment variables for API key and model selection

### Models
Default model varies by plan; users can select from:
- `o3`, `o4-mini` — Optimized for coding tasks
- `gpt-4` and successors
- Model selection via `--model` flag or config

## Key Features

- **Open source** — Full source code available, community contributions welcome
- **Rust-based** — Fast startup, low memory footprint
- **Multimodal input** — Can accept images (screenshots, diagrams) as part of prompts
- **IDE extensions** — Available for VS Code, Cursor, and Windsurf
- **GitHub Action** — Integrate into CI/CD workflows
- **GitHub integration** — Cloud Codex can be triggered from issues and PRs
- **MCP support** — Connect external tools via [[Model Context Protocol]]
- **Slash commands** — Built-in commands for common operations

## Strengths

- Open source — inspect, modify, self-host the agent logic
- Strong sandboxing by default — safe for autonomous operation
- Fast Rust implementation
- Cloud version for asynchronous, long-running tasks

## Limitations

- Locked to OpenAI models (no Anthropic, Google, etc.)
- Sandboxing limits some workflows (e.g., tools needing network access)
- Cloud Codex requires ChatGPT subscription

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[OpenCode]]
- [[Approval Flows and Sandboxing]]
- [[Model Context Protocol]]
