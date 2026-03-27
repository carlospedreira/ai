# OpenCode

OpenCode is an open-source, provider-agnostic [[AI Coding Harnesses|AI coding harness]] built for the terminal. Created by the team behind SST (Serverless Stack), it has become the most popular open-source coding agent with 131K+ GitHub stars, 828+ contributors, and 650K+ monthly active developers.

## Overview

| Property | Value |
|---|---|
| Developer | SST (Jay V, Frank Wang, Dax Raad, Adam Elmore) → anomalyco / opencode-ai |
| Open Source | Yes (MIT) |
| Repository | `sst/opencode` on GitHub |
| Built With | TypeScript on Bun (server), Solid.js (web/desktop UI), Tauri (desktop) |
| Models | 75+ providers, 1,000+ models via Models.dev catalog |
| Platforms | Terminal TUI, Desktop App (Tauri), VS Code extension, Web UI, headless HTTP server |
| Pricing | Free and open-source; bring your own API keys |
| Stars | 131K+ (March 2026) |
| Launched | June 19, 2025 |

## Origin Story

SST was a successful open-source cloud infrastructure framework (25K GitHub stars, profitable). The founders — who met at the University of Waterloo — pivoted to OpenCode as their follow-on product. The v1.0 rewrite shipped in late December 2025, marking a major architectural maturation.

**The January 2026 surge:** When Anthropic blocked third-party tools from using Claude Pro/Max subscriptions (browser OAuth), developers rallied around OpenCode. It gained ~18K GitHub stars in two weeks. DHH (David Heinemeier Hansson) called Anthropic's move "very customer hostile." The controversy paradoxically accelerated OpenCode's growth.

## What Makes It Different

**Provider agnosticism.** Unlike [[Claude Code]] (locked to Claude) or [[OpenAI Codex]] (locked to OpenAI), OpenCode works with virtually any LLM. You can switch models mid-session.

| Provider | Examples |
|---|---|
| Anthropic | Claude Sonnet, Opus, Haiku (API key only since Jan 2026) |
| OpenAI | GPT-4/5, o3, o4-mini (API or ChatGPT Plus auth) |
| Google | Gemini 2.5 Pro/Flash via AI Studio or Vertex AI |
| AWS Bedrock | Claude, Titan, etc. via AWS credentials |
| Azure OpenAI | GPT-4/5, o-series |
| Groq / Cerebras | Fast inference (LLaMA, Qwen, Mixtral) |
| DeepSeek | DeepSeek R1, Coder |
| OpenRouter | Aggregator with 100+ models preloaded |
| Ollama / LM Studio / llama.cpp | Any local model, zero cloud dependency |
| GitHub Copilot | Pro+ subscriptions |
| GitLab Duo | Premium/Ultimate, including self-hosted |
| xAI | Grok models |

**OpenCode's own tiers:** Zen (curated models, pass-through pricing), Go (low-cost subscription for open models), Black ($200/mo enterprise).

## Architecture

**Client-server design** — the defining architectural choice:

```
┌──────────────┐     ┌──────────────┐
│  TUI Client  │────►│  Background  │
│  (Bubble Tea)│◄────│    Server    │
└──────────────┘     └──────┬───────┘
                            │ SSE (real-time updates)
┌──────────────┐     ┌──────┴───────┐     ┌───────────┐
│  Desktop App │────►│   Hono HTTP  │────►│  SQLite   │
│  (Tauri)     │     │    Server    │     │ (Drizzle) │
└──────────────┘     └──────────────┘     └───────────┘
┌──────────────┐            ▲
│   Web UI     │────────────┘
│  (Solid.js)  │
└──────────────┘
```

- **Background server** — Persists across terminal disconnects, SSH drops, and machine sleeps. Detachable like tmux for AI.
- **Multiple clients** — TUI, desktop, web, VS Code extension can all connect to the same running server.
- **SQLite + Drizzle ORM** — Session history, file changes, and context stored persistently.
- **Vercel AI SDK** — Provider abstraction layer for model communication.
- **Bun runtime** — TypeScript execution (not Go, despite some early reports).

Key commands:
- `opencode attach` — Connect new TUI to running server
- `opencode serve` — Launch headless HTTP API server
- `opencode web` — Add browser interface
- `opencode acp` — Launch Agent Client Protocol server
- `opencode run "prompt"` — Non-interactive execution

## Built-In Agents

Four agents, two primary and two sub-agents:

| Agent | Type | Access | Purpose |
|---|---|---|---|
| **build** | Primary (default) | Full access | Development — reads, writes, executes |
| **plan** | Primary | Read-only | Analysis and exploration, bash requires approval |
| **general** | Sub-agent | Full access | Complex multi-step research tasks |
| **explore** | Sub-agent | Read-only | Fast codebase exploration |

Hidden system agents: Compaction (context management), Title (auto-names sessions), Summary (session summaries).

**Custom agents:** Define as Markdown files in `~/.config/opencode/agents/` or `.opencode/agents/`.

Switch between build and plan with `Tab`.

## Built-In Tools (13)

| Tool | Purpose |
|---|---|
| `bash` | Execute shell commands |
| `edit` | Modify files via exact string replacement |
| `write` | Create or overwrite files |
| `read` | Read file contents |
| `grep` | Regex search across file contents |
| `glob` | Find files by pattern |
| `list` | List directory contents |
| `lsp` | Query LSP servers (experimental — currently diagnostics only) |
| `patch` | Apply diffs to files |
| `skill` | Load instruction files into context |
| `todowrite` | Manage a task list during session |
| `webfetch` | Fetch web page content |
| `websearch` | Web search (requires Exa AI key) |
| `question` | Ask the user clarifying questions |

All tools default to auto-allowed. Configurable to `ask`, `deny`, or `allow` using wildcard patterns.

## Key Features

### TUI Interface
Rich terminal UI built with Bubble Tea:
- Split panes for conversation and file changes
- Vim-like editor for input
- File change tracking and diffs
- Session management and switching

### LSP Integration (30+ Languages)
Auto-starts language servers when relevant file extensions are detected. Currently surfaces **diagnostics** (type errors, lint warnings) to the AI in real-time, enabling self-correcting behavior. Covers TypeScript, Python, Go, Rust, Java, C/C++, Ruby, Elixir, PHP, and many more.

### MCP Support
Full [[Model Context Protocol]] integration — both local (subprocess) and remote (HTTP) MCP servers. Tools appear alongside built-in tools with the same permission model.

### GitHub Integration
- Comment `/opencode` or `/oc` on any GitHub issue or PR
- Runs within GitHub Actions runners
- Can create branches, commit changes, open PRs
- Triggers: `issue_comment`, `pull_request`, `issues`, `schedule`, `workflow_dispatch`

### Skills System
Reusable instruction files loadable on demand — a library of task-specific prompts and context that persists across sessions.

### Plugin System
Plugins loaded from npm or local directories for structured community extensions.

### Change Tracking
Tracks all file changes per session. Full undo/redo with `/undo` and `/redo`. Snapshot system enabled by default.

### Session Sharing
Export/import sessions as JSON. Share with teammates via `/share`. Configurable: `manual`, `auto`, or `disabled`.

## Configuration

### Config Files

Merged in priority order (later overrides earlier):
1. Remote config (`.well-known/opencode` endpoint — for team sharing)
2. Global: `~/.config/opencode/opencode.json`
3. `OPENCODE_CONFIG` environment variable
4. Project: `opencode.json` in project root
5. `.opencode/` directory (agents, commands, plugins, themes)
6. `OPENCODE_CONFIG_CONTENT` env var (inline JSON)

Supports `{env:VARIABLE_NAME}` and `{file:path}` variable substitution.

```json
{
  "model": "anthropic/claude-sonnet-4-6",
  "small_model": "anthropic/claude-haiku-4-5",
  "provider": { "timeout": 30000 },
  "mcpServers": { ... },
  "tools": { "bash": "ask" },
  "compaction": "auto"
}
```

Project instructions go in `AGENTS.md` (convention shared with [[OpenAI Codex]]).

### TUI Config
Separate `tui.json` for themes, keybindings, scroll speed. Schema at `opencode.ai/tui.json`.

### Credentials
API keys configured via `/connect` stored in `~/.local/share/opencode/auth.json`.

## Strengths

- **Provider-agnostic** — 1,000+ models, switch mid-session, use local models
- **Open source (MIT)** — Full access, community-driven, 828+ contributors
- **Detachable sessions** — Client-server architecture survives disconnects
- **LSP integration** — 30+ languages, real-time diagnostics fed to AI
- **Rich TUI** — Best-in-class terminal experience
- **GitHub-native** — Comment-driven automation in Actions
- **Multiple interfaces** — TUI, desktop, web, VS Code, headless API

## Limitations

- No built-in sandboxing (unlike Codex's OS-level sandbox)
- Quality depends heavily on which model you choose
- Anthropic blocked OAuth (Jan 2026) — must use API keys for Claude
- LSP tool still experimental (only diagnostics, not go-to-definition for AI)
- Desktop app uses Tauri (less mature than Electron)

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[Model Context Protocol]]
- [[Context Management]]
- [[AI Coding Landscape]]
