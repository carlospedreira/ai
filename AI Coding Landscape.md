# AI Coding Landscape

The AI coding tool ecosystem has exploded since 2023. This note maps the major players beyond the three terminal-based harnesses ([[Claude Code]], [[OpenAI Codex]], [[OpenCode]]).

## Categories

### Terminal-Based (CLI/TUI)

| Tool | By | Open Source | Model Support | Key Feature |
|---|---|---|---|---|
| [[Claude Code]] | Anthropic | No | Claude only | Best Claude integration, CLAUDE.md |
| [[OpenAI Codex]] | OpenAI | Yes | OpenAI only | Sandboxing, cloud agent |
| [[OpenCode]] | Community | Yes | 75+ models | Provider-agnostic, persistent sessions |
| **Aider** | Paul Gauthier | Yes | Multi-model | Git-native, auto-commits changes |
| **Amp** | Sourcegraph | No | Multi-model | Deep code search integration |

### IDE-Based (Full Editor)

| Tool | Base | Model Support | Key Feature |
|---|---|---|---|
| **Cursor** | VS Code fork | Multi-model | Parallel sub-agents, custom Composer model, codebase indexing |
| **Windsurf** | VS Code fork | Multi-model | Cascade flow, acquired by OpenAI |
| **Void** | VS Code fork | Multi-model | Fully open-source IDE |
| **Trae** | VS Code fork | Claude, GPT | By ByteDance |

### IDE Extensions

| Tool | IDE | Model Support | Key Feature |
|---|---|---|---|
| **GitHub Copilot** | VS Code, JetBrains, Neovim | Multi-model | Agent mode, deepest GitHub integration |
| **Cline** | VS Code | Multi-model | Open-source, high autonomy, MCP support |
| **Roo Code** | VS Code | Multi-model | Fork of Cline with enhanced features |
| **Continue.dev** | VS Code, JetBrains | Multi-model | Open-source, configurable pipeline |
| **Supermaven** | VS Code, JetBrains | Proprietary | Fastest autocomplete (300K token context) |

### Cloud / Autonomous Agents

| Tool | By | How It Works |
|---|---|---|
| **Codex (Cloud)** | OpenAI | Runs in ChatGPT, operates on repo clone asynchronously |
| **Devin** | Cognition | Autonomous SE agent with browser, terminal, editor |
| **GitHub Copilot Workspace** | GitHub | Plan → implement → test flow from issues |
| **Factory.ai** | Factory | Autonomous coding agents ("Droids") for specific tasks |

## Key Differentiators

### Terminal vs IDE

| Aspect | Terminal (Claude Code, Codex, OpenCode) | IDE (Cursor, Windsurf) |
|---|---|---|
| Interface | Text-based, keyboard-driven | Visual editor, click-and-point |
| Context | Explicit file reading | Automatic codebase indexing |
| Portability | Works over SSH, in containers | Requires desktop |
| Workflow | Command-driven | Editor-integrated |
| Best for | Power users, automation, CI/CD | Visual development, rapid prototyping |

### Open vs Closed

| Aspect | Open (OpenCode, Aider, Cline) | Closed (Claude Code, Cursor) |
|---|---|---|
| Model choice | Any provider, any model | Locked or limited |
| Customization | Full source access | Configuration only |
| Cost | Only API costs | Subscription + API costs |
| Support | Community | Commercial |

### Cursor Deep Dive

Cursor deserves special mention as the most popular IDE-based harness:
- **VS Code fork** with deep AI integration
- **Composer mode** for multi-file agentic changes
- **8 parallel sub-agents** that explore and modify code simultaneously
- **Custom embedding model** for codebase indexing
- **Agent Client Protocol (ACP)** — now available in JetBrains IDEs too
- Multi-model: GPT-4, Claude, Gemini, and Cursor's own models
- Benchmarks claim 4x faster than comparable models

### GitHub Copilot Evolution

Copilot has evolved through all three generations of AI coding:
1. **Copilot (2021)** — Autocomplete
2. **Copilot Chat (2023)** — Conversational coding
3. **Copilot Agent Mode (2025)** — Full agentic coding with tool use, terminal access, and multi-step execution

Agent mode includes:
- Automatic context gathering from workspace
- Terminal command execution
- Iterative error fixing
- MCP support for extensibility

## Protocols and Standards

| Protocol | Purpose | By |
|---|---|---|
| [[Model Context Protocol]] (MCP) | Tool/data source interoperability | Anthropic |
| **Agent Client Protocol (ACP)** | IDE ↔ agent communication | Cursor |

## The Market in 2026

The space is consolidating:
- OpenAI acquired Windsurf (Codeium)
- Terminal harnesses gaining traction alongside IDEs
- MCP becoming a de facto standard for tool integration
- Model providers building their own harnesses (Anthropic → Claude Code, OpenAI → Codex)
- Open source alternatives keeping pace (OpenCode, Aider, Cline)

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Model Context Protocol]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[OpenCode]]
