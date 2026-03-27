# OpenAI Codex

OpenAI Codex is OpenAI's [[AI Coding Harnesses|AI coding harness]] — available as both an open-source terminal agent and a cloud-based autonomous coding agent in ChatGPT. Not to be confused with the original Codex model from 2021, this is the agent product launched in April 2025.

## Overview

| Property | Value |
|---|---|
| Developer | OpenAI |
| Open Source | Yes (Apache 2.0) — CLI only |
| Repository | `openai/codex` on GitHub (66K+ stars) |
| Built With | Rust (rewritten from TypeScript mid-2025) |
| Models | GPT-5.x series, codex-1 (cloud), codex-mini-latest |
| Platforms | Terminal CLI, VS Code, Cursor, Windsurf, JetBrains, Xcode, Desktop App |
| Pricing | ChatGPT Plus ($20/mo), Pro ($200/mo), Business ($30/user/mo), or API pay-per-token |
| Cloud Users | 2M+ weekly active users (March 2026) |

## Two Products, One Brand

### Codex CLI (Local)
Open-source terminal agent. Runs locally, calls the OpenAI API for inference, executes tools on your machine inside an OS-level sandbox. Originally TypeScript/Node.js, fully rewritten in Rust mid-2025 for zero-dependency installation and native sandboxing.

### Codex Cloud
Autonomous agent accessible through ChatGPT. Runs in isolated cloud sandbox containers preloaded with your Git repository. Tasks run asynchronously (1–30 minutes), with internet access disabled by default. Powered by **codex-1** — a version of o3 fine-tuned for software engineering via reinforcement learning. Returns completed pull requests.

### Codex Security (March 2026)
Specialized application security agent. Analyzes repository structure, generates threat models, finds vulnerabilities, and validates findings in sandboxed environments. Scanned 1.2M+ commits across external repos, identified 792 critical findings. Available in research preview.

## How It Works

Codex CLI implements the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] as a state machine:

1. **Prompt assembly** — User input + system instructions + AGENTS.md + tool schemas + MCP servers + environment info. Static content goes at the start (maximizes prompt cache hits).
2. **Inference** — Tokens sent to OpenAI API; model streams back reasoning steps and tool calls
3. **Tool execution** — File reads/writes and shell commands run inside the [[Approval Flows and Sandboxing|sandbox]]
4. **Tool response** — Results appended to conversation history
5. **Loop** — Continues until the model produces a text-only response

Key technical details:
- **Stateless request handling** — Full conversation sent each request (enables Zero Data Retention)
- **Prompt caching** — Preserves exact prefix matches for cache reuse, yielding linear not quadratic cost
- **Automatic context compaction** — Calls `/responses/compact` endpoint for encrypted compressed summaries when approaching window limits
- **~90% of Codex's own codebase was written by Codex** — internal releases 3–4 times daily

## The Rust Rewrite

The original CLI was TypeScript/React (Ink)/Node.js. Mid-2025 rewrite to Rust (now 95.6% of codebase):

- **Zero-dependency installation** — Single binary, no Node.js v22+ required
- **Native security** — Landlock/seccomp bindings for Linux sandboxing built directly in
- **Performance** — No GC overhead, lower memory consumption
- **Wire protocol** — Extensible protocol for building extensions in other languages
- **Library crate** — `codex-rs/` is reusable for building Rust-native tools on top of Codex

## Sandboxing

Codex's most distinctive feature. Implemented at the **OS level** using platform-native enforcement:

| Platform | Mechanism |
|---|---|
| Linux | Landlock (filesystem) + seccomp (syscall filtering) |
| macOS | Apple Sandbox (seatbelt) |
| Windows | Experimental (WSL recommended) |

### Sandbox Modes

| Mode | Description |
|---|---|
| `read-only` | File reading only; all modifications require approval |
| `workspace-write` | Read/edit within workspace, run local commands. **Network disabled.** Default. |
| `danger-full-access` | No restrictions — full filesystem and network access |

The sandbox constrains not just Codex but all spawned subprocesses (git, package managers, test runners). Protected paths (`.git`, `.agents`, `.codex`) remain read-only regardless of mode.

### Approval Policies

| Policy | Description |
|---|---|
| `untrusted` | Approval required for any non-trusted command |
| `on-request` | Autonomous within sandbox, asks to exceed boundaries. Default. |
| `never` | No prompts (only with `danger-full-access`) |

Auto-detection: version-controlled folders default to `workspace-write + on-request`; other folders default to read-only.

## Configuration

### AGENTS.md

Hierarchical discovery:
1. `~/.codex/AGENTS.override.md` (highest priority)
2. `~/.codex/AGENTS.md` (global personal defaults)
3. Walk from Git root to working directory, loading `AGENTS.md` at each level
4. `AGENTS.override.md` in nested directories hard-overrides parents

Combined limit: 32 KiB (configurable via `project_doc_max_bytes` in `config.toml`).

### config.toml

Three levels: system (`/etc/codex/`), user (`~/.codex/`), project (`.codex/`).

Key settings:
- **Model**: `model`, `model_reasoning_effort` (minimal/low/medium/high/xhigh)
- **Sandbox**: `sandbox_mode`, `approval_policy`, `permissions.<name>.filesystem/network`
- **Features**: `features.multi_agent`, `features.shell_snapshot`, `features.undo`
- **MCP**: `mcp_servers.<id>.command`, `enabled_tools`, `disabled_tools`
- **Profiles**: Named presets switchable via `--profile <name>`

### Multimodal Input
Accepts images via `--image` flag (PNG, JPEG, GIF, WebP). Paste images in the interactive composer. Use cases: implement from a UI mockup screenshot, debug from a stack trace image.

## Models

| Model | Use Case |
|---|---|
| GPT-5.4 | Default recommended model |
| GPT-5.3-Codex | Specialized coding model with frontier capabilities |
| GPT-5.3-Codex-Spark | Lower-latency variant (Pro subscribers) |
| GPT-5.4-mini | Faster and cheaper for lighter tasks |
| codex-1 | Cloud agent model (fine-tuned o3 for SE) |
| codex-mini-latest | Lightweight cloud workflows |

Switch models with `/model` slash command or `--model` flag.

## Pricing

### Subscription (per 5-hour rolling window)

| Plan | Price | GPT-5.4 Messages | GPT-5.3-Codex Messages |
|---|---|---|---|
| Plus | $20/mo | 33–168 | 45–225 |
| Pro | $200/mo | 223–1,120 | 300–1,500 |
| Business | $30/user/mo | Larger VMs, SSO, admin controls |

### API Pay-Per-Token

| Model | Input/MTok | Output/MTok |
|---|---|---|
| codex-mini-latest | $1.50 | $6.00 |
| GPT-5.1-Codex-Mini | $0.25 | $2.00 |
| GPT-5.3-Codex | $1.75 | $14.00 |

## Key Features

- **Open source (Apache 2.0)** — Full Rust source, community contributions
- **OS-level sandboxing** — Strongest safety defaults of any coding agent
- **Cloud agent** — Asynchronous, fire-and-forget task execution with PR output
- **Rust binary** — Fast, zero-dependency, low memory
- **Multimodal** — Images as prompt context
- **MCP support** — Extend via [[Model Context Protocol]] servers
- **GitHub Action** — CI/CD integration
- **Multi-agent** — Sub-agents with path-based naming (`/root/agent_a`)
- **Profiles** — Named config presets for different workflows
- **Codex Security** — Specialized security scanning agent

## Strengths

- Strongest sandboxing of any coding agent (OS-level enforcement)
- Open source — inspect, modify, contribute
- Cloud version for autonomous long-running tasks
- Fast Rust implementation
- Transparent — 90%+ of codebase written by Codex itself

## Limitations

- Locked to OpenAI models
- Sandboxing blocks some legitimate workflows needing network
- Cloud Codex requires ChatGPT subscription
- Windows support still experimental
- Model names have changed multiple times (naming instability)

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[OpenCode]]
- [[Approval Flows and Sandboxing]]
- [[Model Context Protocol]]
