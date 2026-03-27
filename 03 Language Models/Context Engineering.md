# Context Engineering

Context engineering is the discipline of designing and optimizing what information goes into a [[Large Language Models|Large Language Model's]] context window. While [[Prompt Engineering]] focuses on how to phrase instructions, context engineering focuses on **what information the model needs to see** and **how to organize it** for maximum effectiveness.

## Why It Matters

The context window is the model's entire world. Everything the model knows about the current task comes from:
1. System prompt and instructions
2. Conversation history
3. Tool results (file contents, command output, search results)
4. Retrieved documents ([[Retrieval-Augmented Generation|RAG]])
5. Memory and project instructions (CLAUDE.md, AGENTS.md)

**Garbage in, garbage out.** A model with a 1M token window filled with irrelevant code will perform worse than one with 10K tokens of the right context.

## Context Engineering vs Prompt Engineering

| Aspect | Prompt Engineering | Context Engineering |
|---|---|---|
| Focus | How you phrase the request | What information you include |
| Scope | The instruction itself | The entire context window |
| Analogy | Asking the right question | Giving the right briefing materials |
| Scale | Single prompt | System-level design |

## Key Principles

### 1. Relevance Over Volume

More context is not always better. Research shows that model performance often degrades when the context window is more than ~60% full with information of mixed relevance.

```
Better: 5 relevant files (2K tokens)
Worse:  50 files "just in case" (100K tokens)
```

### 2. Structure and Organization

How information is organized matters:
- Put the most important instructions first (primacy effect)
- Put the current task and recent context last (recency effect)
- Group related information together
- Use clear section headers and separators

### 3. Stable Prefix, Variable Suffix

Many [[AI Coding Harnesses]] split context into:
- **Stable prefix** — System prompt, tool definitions, project instructions (rarely changes)
- **Variable suffix** — Current conversation, latest tool results (changes every turn)

This pattern maximizes [[Inference Optimization|prompt cache]] hit rates — the stable prefix is computed once and reused.

### 4. Progressive Disclosure

Don't load everything upfront. Let the agent discover information as needed:
```
Turn 1: Agent reads high-level architecture overview
Turn 2: Agent searches for specific module
Turn 3: Agent reads the relevant file
Turn 4: Agent reads the test file
```

Each turn adds precisely the context needed for the next step.

## Context Engineering in Coding Agents

### CLAUDE.md / AGENTS.md Design

The project instruction file is the highest-leverage context engineering you can do:

**Good CLAUDE.md:**
```markdown
# Project: PaymentService
- TypeScript monorepo, pnpm workspaces
- Run tests: `pnpm test`
- Run lint: `pnpm lint`
- Key abstraction: all payment providers implement `PaymentAdapter` interface
- NEVER modify files in `src/generated/` — they're auto-generated
- Use `zod` for runtime validation, not manual type guards
```

**Bad CLAUDE.md:**
```markdown
This is a project that does payment processing. We use various technologies
and frameworks. Please be careful when making changes. The team follows
best practices and we value clean code...
```

The good version gives actionable, verifiable facts. The bad version is vague and wastes tokens.

### Tool Result Filtering

Smart harnesses filter tool results before adding them to context:
- Truncate very long file contents to relevant sections
- Summarize verbose command output
- Omit binary files and generated code from search results

### Context Compaction

When context fills up, compress older information:
- [[Claude Code]] uses `/compact` to summarize earlier conversation
- [[OpenAI Codex]] calls `/responses/compact` for encrypted summaries
- Key decisions and file changes are preserved; verbose tool output is discarded

## The Context Engineering Stack

For building AI applications, context engineering becomes a full discipline:

```
┌─────────────────────────────────────┐
│         Context Window              │
├─────────────────────────────────────┤
│ System prompt (instructions, role)  │ ← Stable
│ Tool definitions (schemas)          │ ← Stable
│ Project instructions (CLAUDE.md)    │ ← Stable
├─────────────────────────────────────┤
│ Retrieved context (RAG results)     │ ← Per-query
│ File contents (tool results)        │ ← Per-turn
│ Conversation history               │ ← Growing
│ Current user message               │ ← Latest
└─────────────────────────────────────┘
```

Each layer requires design decisions:
- What goes in the system prompt?
- How many RAG results to include?
- When to compact conversation history?
- How much of each file to include?

## Anti-Patterns

| Anti-Pattern | Problem |
|---|---|
| Stuffing the entire codebase | Exceeds window, dilutes relevance |
| No project instructions | Agent lacks critical context, makes wrong assumptions |
| Never compacting | Context fills with stale information |
| Repeated information | Same content appears multiple times, wastes tokens |
| Instructions at the end | May be ignored (middle-of-context attention weakness) |

## Related

- [[Context Management]]
- [[Prompt Engineering]]
- [[Retrieval-Augmented Generation]]
- [[Tokens and Tokenization]]
- [[AI Coding Harnesses]]
- [[Claude Code]]
