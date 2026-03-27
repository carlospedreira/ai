# AI Coding Landscape

The AI coding tool ecosystem has exploded since 2023. This note maps the major players beyond the three terminal-based harnesses ([[Claude Code]], [[OpenAI Codex]], [[OpenCode]]).

## Categories

### Terminal-Based (CLI/TUI)

| Tool | By | Open Source | Model Support | Key Differentiator |
|---|---|---|---|---|
| [[Claude Code]] | Anthropic | No | Claude only | Deepest Claude integration, hooks, agent teams |
| [[OpenAI Codex]] | OpenAI | Yes | OpenAI only | OS-level sandboxing, cloud agent |
| [[OpenCode]] | SST/Community | Yes (MIT) | 1,000+ models | Provider-agnostic, persistent sessions |
| **Aider** | Paul Gauthier | Yes | Multi-model | Repository map (tree-sitter), auto-git-commits |
| **Gemini CLI** | Google | Yes (Apache 2.0) | Gemini | Free tier (60 req/min), 1M token context, `GEMINI.md` |
| **Amp** | Sourcegraph | No | Multi-model | Code graph intelligence, team thread sharing |

### IDE-Based (Full Editor)

| Tool | Base | Key Differentiator |
|---|---|---|
| **Cursor** | VS Code fork | 8 parallel sub-agents, custom Composer model, codebase embedding index |
| **Windsurf** | VS Code fork | Cascade agentic flow with persistent memory. Acquired by OpenAI. |
| **Zed** | Custom (Rust) | Open-source, high-perf, co-created Agent Client Protocol (ACP) with JetBrains |
| **AWS Kiro** | VS Code fork | Spec-driven development — generates requirements before code |
| **Trae** | VS Code fork | By ByteDance |
| **Void** | VS Code fork | Fully open-source |

### IDE Extensions

| Tool | IDE | Key Differentiator |
|---|---|---|
| **GitHub Copilot** | VS Code, JetBrains, Eclipse, Xcode | Agent mode + autonomous "Coding Agent" (assigns issues to AI) |
| **Cline** | VS Code (+ ACP for others) | Open-source, 5M+ installs, step-by-step approval, browser control |
| **Roo Code** | VS Code | Fork of Cline with role-based modes (Architect, Code, Debug, Ask) |
| **Continue.dev** | VS Code, JetBrains | Open-source, self-hostable, local model support via Ollama |
| **JetBrains Junie** | JetBrains IDEs | Uses JetBrains' full static analysis stack, not just LLM generation |
| **Supermaven** | VS Code, JetBrains | Fastest autocomplete (300K token context) |

### Cloud / Autonomous Agents

| Tool | By | How It Works |
|---|---|---|
| **Codex Cloud** | OpenAI | Runs in ChatGPT, isolated sandbox, async PR creation |
| **Devin** | Cognition Labs | Full cloud IDE (editor + terminal + browser), $500/mo |
| **Copilot Coding Agent** | GitHub | Assign issues to Copilot, returns completed PRs |
| **Augment Code** | Augment | 400K+ file indexing, ISO 42001 + SOC 2 certified |
| **Factory.ai** | Factory | Autonomous "Droids" for specific engineering tasks |

## Cursor Deep Dive

Cursor deserves special attention as the most popular IDE-based harness:

- **VS Code fork** with deep AI integration
- **Composer 2.0** — Multi-agent architecture with up to **8 parallel sub-agents**
- Each sub-agent uses the best model for its subtask
- **Custom embedding model** trained specifically for code recall
- **Autonomy slider** — Tab complete → Cmd+K edit → full agent mode
- **DOM reading** — Agents can read the browser DOM and run end-to-end frontend tests inside the editor
- **4x faster** completion than comparable models (Cursor benchmarks)
- **Agent Client Protocol (ACP)** — Now available in JetBrains IDEs too
- Multi-model: GPT-5, Claude, Gemini, and Cursor's own models

## GitHub Copilot Evolution

Copilot has traversed all three generations of AI coding:

1. **Copilot (2021)** — Inline autocomplete
2. **Copilot Chat (2023)** — Conversational coding
3. **Agent Mode (Feb 2025)** — Full agentic coding: reads codebase, edits files, runs terminal commands, observes errors, self-corrects in a loop
4. **Coding Agent (2025)** — Autonomous background agent: assign GitHub Issues directly to Copilot, returns completed PRs

Agent mode supports MCP, multi-model selection (Claude, Gemini, GPT), and automatic error recovery.

## Aider Deep Dive

The most established open-source terminal agent:

- **Repository map** — tree-sitter parses the entire codebase, extracts function signatures and class structures. Provides structural context without loading every file.
- **Git-native** — Every change is auto-committed with descriptive messages. Built-in linting and testing after each change.
- **Three modes** — `code` (makes edits), `architect` (plans before editing), `ask` (consultation only)
- **Voice and multimodal** — Speak to Aider, attach images and web pages
- **Model-agnostic** — GPT-5, Claude 4, Grok-4, Gemini 2.5, DeepSeek R1, local models via Ollama

## Cline and Roo Code

Two popular open-source VS Code extensions, diverged from a common codebase:

**Cline** (5M+ installs):
- Plan/Act mode split — proposes strategy, then executes with approval
- Every file change and command requires explicit user approval by default
- Workspace snapshots at every step for rollback
- Can control a browser (navigate, click, fill forms, screenshot)
- CLI 2.0 for headless CI/CD use

**Roo Code** (fork of Cline):
- Role-based **specialized modes**: Architect, Code, Debug, Ask, Custom
- Targeted diffs across modules (faster for large refactors)
- Higher autonomy, fewer "half-finished edits"
- Community reputation: "the tool you reach for when other agents break down"

## Key Protocols and Standards

| Protocol | Purpose | By | Adoption |
|---|---|---|---|
| [[Model Context Protocol]] (MCP) | Tool/data source interoperability | Anthropic | Industry-wide (every major tool) |
| **Agent Client Protocol (ACP)** | IDE ↔ agent communication | Cursor + JetBrains + Zed | Growing |

## Key Differentiators

### Terminal vs IDE

| Aspect | Terminal | IDE |
|---|---|---|
| Interface | Text-based, keyboard-driven | Visual editor |
| Context | Explicit file reading, grep | Automatic codebase indexing |
| Portability | Works over SSH, containers, CI/CD | Requires desktop |
| Best for | Power users, automation, large codebases | Visual development, frontend work |

### Open vs Closed

| Aspect | Open (OpenCode, Aider, Cline) | Closed (Claude Code, Cursor) |
|---|---|---|
| Model choice | Any provider | Locked or curated |
| Customization | Full source access | Configuration only |
| Cost | Only API costs | Subscription + API costs |
| Privacy | Full control over data | Trust the vendor |

### Autonomy Spectrum

```
Most supervised ◄──────────────────────────────────► Most autonomous

  Cline       Claude Code     Cursor        Codex Cloud      Devin
  (approve    (permission     (autonomy     (async, PR       (full
   every       modes)         slider)        review)         autonomy)
   step)
```

## The Market in 2026

- **Anthropic** surpassed $2.5B annualized revenue, largely driven by Claude Code
- **OpenAI** acquired Windsurf (Codeium) — consolidation of IDE + cloud
- **MCP** is the de facto standard for tool interoperability
- Developers use an average of **2.3 AI coding tools** simultaneously
- **46%** of developers rate Claude Code "most loved" (developer surveys)
- AI is integrated into **60%** of developer work (Anthropic 2026 report)
- ~**27%** of AI-assisted work consists of tasks that wouldn't have been attempted without agents (net-new productivity)
- **Stripe** internally generates and merges 1,000+ AI-created PRs per week

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Model Context Protocol]]
- [[Claude Code]]
- [[OpenAI Codex]]
- [[OpenCode]]
- [[Approval Flows and Sandboxing]]
