# OpenCode

OpenCode is an open-source, provider-agnostic [[AI Coding Harnesses|AI coding harness]] built for the terminal. Originally created by SST (Serverless Stack), it has grown into one of the most popular open-source coding agents with 130K+ GitHub stars and 650K+ monthly active developers.

## Overview

| Property | Value |
|---|---|
| Developer | SST → anomalyco / opencode-ai community |
| Open Source | Yes (MIT) |
| Repository | `sst/opencode` → `opencode-ai/opencode` on GitHub |
| Built With | Go (server/TUI), TypeScript (docs/plugins) |
| Models | 75+ models from any provider |
| Platforms | Terminal (TUI), Desktop App (beta), IDE extensions, GitHub Actions |
| Pricing | Free and open-source; you pay your chosen AI provider |
| Stars | 130K+ on GitHub |

## What Makes It Different

OpenCode's defining feature is **provider agnosticism**. Unlike [[Claude Code]] (locked to Claude) or [[OpenAI Codex]] (locked to OpenAI), OpenCode works with virtually any LLM:

| Provider | Models |
|---|---|
| Anthropic | Claude Sonnet, Opus, Haiku |
| OpenAI | GPT-4, o3, o4-mini |
| Google | Gemini 2.5 Pro, Flash |
| AWS Bedrock | Claude, Titan, others |
| Azure OpenAI | GPT-4, o-series |
| Groq | LLaMA, Mixtral (fast inference) |
| OpenRouter | Access to 100+ models |
| Ollama / Local | Any local model |

## Architecture

OpenCode uses a **client-server architecture**:

```
┌──────────────┐     ┌──────────────┐
│  TUI Client  │────►│  Background  │
│  (Bubble Tea)│◄────│    Server    │
└──────────────┘     └──────────────┘
                           │
                     ┌─────┴─────┐
                     │  SQLite   │
                     │  Storage  │
                     └───────────┘
```

- **Background server** — Persists across terminal disconnects, SSH drops, and machine sleeps. Sessions survive crashes.
- **TUI client** — Built with Bubble Tea (Go TUI framework). Rich terminal interface with vim-like keybindings.
- **SQLite storage** — Session history, file changes, and context are stored persistently.

This architecture means you can disconnect from a session and reconnect later — the agent keeps working.

## Built-In Agents

OpenCode ships with two agents, switchable with the `Tab` key:

| Agent | Access Level | Purpose |
|---|---|---|
| **build** | Full access | Default development agent — reads, writes, executes |
| **plan** | Read-only | Analysis and code exploration — cannot modify files |

## Key Features

### TUI Interface
A full terminal UI with:
- Split panes for conversation and file changes
- Vim-like editor for input
- File change tracking and diffs
- Session management and switching

### LSP Integration
OpenCode integrates with Language Server Protocol, giving the model access to:
- Go-to-definition
- Find references
- Diagnostics and errors
- Symbol search

This provides the AI with the same code intelligence that IDEs offer.

### MCP Support
Extends capabilities via [[Model Context Protocol]] — connect databases, APIs, documentation, and custom tools.

### GitHub Integration
- Mention `/opencode` or `/oc` in GitHub comments
- OpenCode runs within GitHub Actions runners
- Automates PR creation and code review

### CLI Mode
Beyond the TUI, OpenCode can run in headless CLI mode for automation and CI/CD:
```bash
opencode -m "fix the failing tests" --auto
```

## Configuration

OpenCode is configured via `opencode.json` in the project root or `~/.config/opencode/config.json` globally:

```json
{
  "provider": "anthropic",
  "model": "claude-sonnet-4-6",
  "mcpServers": { ... }
}
```

Project instructions go in `AGENTS.md` (following the emerging convention shared with [[OpenAI Codex]]).

## Strengths

- **Provider-agnostic** — Use any model from any provider, switch freely
- **Open source (MIT)** — Full access to modify and self-host
- **Persistent sessions** — Survive disconnects thanks to client-server architecture
- **Fast** — Go-based server has noticeably faster startup than Node.js alternatives
- **Rich TUI** — Best-in-class terminal experience
- **Large community** — 130K+ stars, 828+ contributors, active development

## Limitations

- Younger project than Claude Code or Codex — less polished in some areas
- No built-in sandboxing (unlike Codex's network-disabled sandbox)
- Provider-agnostic means quality depends heavily on which model you choose
- Desktop app is still in beta

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[Model Context Protocol]]
- [[Context Management]]
