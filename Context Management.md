# Context Management

Context management is one of the most critical challenges in [[AI Coding Harnesses]]. It determines how much of a codebase the [[Large Language Models|LLM]] can "see" at any given time, and directly impacts the quality of the agent's output.

## The Problem

LLMs have a fixed **context window** — the maximum amount of text they can process at once. Even with windows of 200K+ tokens, a typical codebase far exceeds this limit. The harness must decide:
- Which files to include
- How much of each file to show
- When to summarize vs. show full content
- When to discard old context

## Strategies

### 1. On-Demand File Reading

The agent reads files only when it needs them, using tools like `Read`, `Glob`, and `Grep` to explore the codebase.

**Used by:** [[Claude Code]], [[OpenAI Codex]], [[OpenCode]]

Pros:
- Efficient — only loads what's needed
- Works with any codebase size

Cons:
- Agent may miss relevant files it doesn't know about
- Multiple round trips to explore

### 2. Codebase Indexing

An embedding model processes the entire codebase upfront, creating a searchable index. The agent queries this index to find relevant code.

**Used by:** Cursor (custom embedding model), GitHub Copilot, Windsurf

Pros:
- Fast retrieval of relevant context
- Can find semantically similar code (not just keyword matches)

Cons:
- Requires upfront indexing time
- Index must be kept in sync with changes
- Embedding quality varies

### 3. LSP Integration

Using Language Server Protocol to get structured code intelligence: definitions, references, type information, diagnostics.

**Used by:** [[OpenCode]] (native LSP), Cursor, Windsurf

Pros:
- Precise — follows actual code relationships
- Type-aware navigation

Cons:
- Only works for languages with LSP support
- Setup complexity

### 4. Context Compaction

When the conversation grows too long, the harness summarizes earlier parts to free up space for new content.

**Used by:** [[Claude Code]] (`/compact` command), [[OpenAI Codex]] (automatic compaction)

Example: A 50-turn conversation is compressed to a summary of key decisions and changes, preserving the most recent turns in full.

## Project Instruction Files

All major harnesses support placing instructions in the repository that get automatically included in context:

| Harness | File | Scope |
|---|---|---|
| [[Claude Code]] | `CLAUDE.md` | Project, directory, or global |
| [[OpenAI Codex]] | `AGENTS.md` | Project root or subdirectories |
| [[OpenCode]] | `AGENTS.md` | Project root |
| Cursor | `.cursorrules` | Project root |

These files typically contain:
- Project architecture overview
- Build and test commands
- Coding conventions
- Important patterns and anti-patterns

## Context Window Sizes (2026)

| Model | Window |
|---|---|
| Claude Opus 4.6 | 1M tokens |
| Claude Sonnet 4.6 | 200K tokens |
| GPT-4 | 128K tokens |
| Gemini 2.5 Pro | 1M tokens |
| o3 / o4-mini | 200K tokens |

Larger windows help but don't eliminate the need for smart context management — filling the entire window with irrelevant code degrades performance.

## Best Practices (For Users)

1. **Write good CLAUDE.md / AGENTS.md files** — Give the agent a map of your codebase
2. **Keep related code close** — Modular architecture helps agents find what they need
3. **Use `/compact`** — Periodically compress context in long sessions
4. **Be specific in prompts** — Point the agent to relevant files or modules
5. **Small, focused tasks** — Break large tasks into smaller ones that fit in context

## Related

- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
- [[Large Language Models]]
- [[Transformers]]
