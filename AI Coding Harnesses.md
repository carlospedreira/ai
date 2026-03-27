# AI Coding Harnesses

An AI coding harness (also called a coding agent) is a system that wraps a [[Large Language Models|Large Language Model]] in a loop with developer tools — file reading/writing, shell execution, code search, and more — enabling the model to autonomously perform software engineering tasks.

Unlike simple code completion, harnesses give the LLM agency: the ability to observe the codebase, plan changes, execute them, verify results, and iterate. This is the [[Agentic Coding Paradigm]].

## The Big Three Terminal Harnesses

| Tool | By | Open Source | Language | Model Support |
|---|---|---|---|---|
| [[Claude Code]] | Anthropic | No | TypeScript | Claude models |
| [[OpenAI Codex]] | OpenAI | Yes (Apache 2.0) | Rust | OpenAI models |
| [[OpenCode]] | SST / Community | Yes (MIT) | Go + TypeScript | 75+ models, any provider |

## IDE-Based Harnesses

See [[AI Coding Landscape]] for a broader overview.

| Tool | Type | Key Feature |
|---|---|---|
| **Cursor** | VS Code fork | Parallel sub-agents, custom Composer model |
| **Windsurf** | VS Code fork | Cascade agentic flow |
| **GitHub Copilot** | IDE extension | Agent mode, deep GitHub integration |
| **Cline / Roo Code** | VS Code extension | Open-source, model-agnostic |

## How They Work

All coding harnesses share a common architecture — the [[Agentic Coding Paradigm#The Agent Loop|agent loop]]:

```
User Prompt → LLM reasons → LLM calls tool → Tool returns result → LLM reasons → ... → Final response
```

The key differentiators are:
- **[[Context Management]]** — How the harness feeds relevant code to the model
- **[[Approval Flows and Sandboxing]]** — How much autonomy the agent gets
- **[[Model Context Protocol]]** — How tools are connected and extended
- **Model choice** — Locked to one provider vs. model-agnostic

## Related

- [[Agentic Coding Paradigm]]
- [[Large Language Models]]
- [[Prompt Engineering]]
- [[AI Coding Landscape]]
