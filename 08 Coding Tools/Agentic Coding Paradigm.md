# Agentic Coding Paradigm

The agentic coding paradigm represents a fundamental shift in how developers interact with AI: from **autocomplete** (passive suggestions) to **agency** (autonomous task execution). Rather than completing single lines of code, [[AI Coding Harnesses|coding harnesses]] execute multi-step workflows, observe results, and iterate.

## Evolution of AI-Assisted Coding

### Generation 1: Autocomplete (2021–2023)
- GitHub Copilot's initial release, TabNine
- Model suggests the next few lines as you type
- Developer stays fully in control
- Single-turn: no iteration, no context beyond the current file

### Generation 2: Chat + Edit (2023–2024)
- ChatGPT, Cursor Chat, Copilot Chat
- Developer describes a task in natural language
- Model returns a code block, developer applies it manually
- Multi-turn conversation, but no tool execution

### Generation 3: Agentic (2024–present)
- [[Claude Code]], [[OpenAI Codex]], [[OpenCode]], Cursor Agent
- Model plans, reads files, writes code, runs commands, tests, and iterates
- Developer sets the goal; agent executes it
- Multi-step, tool-using, self-correcting

## The Agent Loop

The core architecture behind all [[AI Coding Harnesses]]:

```
┌─────────────┐
│  User Prompt │
└──────┬──────┘
       ▼
┌──────────────┐
│  LLM Reasons │◄──────────────────┐
└──────┬───────┘                   │
       ▼                           │
┌──────────────┐    ┌────────────┐ │
│  Tool Call   │───►│Tool Execute│─┘
│  (read, edit,│    │(filesystem,│
│   bash, grep)│    │ shell, LSP)│
└──────────────┘    └────────────┘
       │
       ▼ (when done)
┌──────────────┐
│   Response   │
└──────────────┘
```

Each cycle of "LLM reasons → calls tool → gets result" is one **iteration**. A single user prompt may trigger dozens or hundreds of iterations — the agent exploring the codebase, making changes, running tests, fixing failures, and verifying the result.

## Core Tools

Every coding harness provides a standard set of tools to the LLM:

| Tool | Purpose |
|---|---|
| **File Read** | Read file contents, search by glob/grep |
| **File Write/Edit** | Create or modify files |
| **Shell/Bash** | Execute arbitrary commands (build, test, lint) |
| **Search** | Find files, symbols, or text across the codebase |
| **Web** | Fetch documentation, search the web |
| **LSP** | Go-to-definition, find references, diagnostics |

Additional tools can be connected via [[Model Context Protocol]] (MCP).

## What Makes It Work

### Extended Thinking
Modern models can "think" before acting — internal chain-of-thought reasoning that plans the approach before executing. This is especially effective for multi-step tasks.

### Context Management
See [[Context Management]]. The harness must give the LLM enough context to understand the codebase without exceeding the context window.

### Self-Correction
When a command fails or a test breaks, the agent can read the error, reason about the cause, and try a different approach — without human intervention.

### Permission Systems
See [[Approval Flows and Sandboxing]]. Since agents can execute commands, they need guardrails to prevent unintended damage.

## Impact on Development

- **Task scope** — Developers describe *what* they want, not *how* to do it
- **Iteration speed** — Agents can explore and fix problems in seconds
- **Knowledge gap bridging** — Agents can work in unfamiliar codebases or languages
- **Code review shifts** — Developers review AI-generated changes rather than writing from scratch

## Related

- [[AI Coding Harnesses]]
- [[Context Management]]
- [[Approval Flows and Sandboxing]]
- [[Model Context Protocol]]
- [[Large Language Models]]
