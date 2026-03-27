# Prompt Caching

Prompt caching is an [[Inference Optimization]] technique where the computed key-value (KV) states for a shared prompt prefix are stored and reused across requests, avoiding redundant computation. It is one of the highest-impact optimizations for [[AI Coding Harnesses]] and any application that sends similar prompts repeatedly.

## The Problem

Every API call reprocesses the entire prompt from scratch:

```
Request 1: [System prompt (5K tokens)] + [User message A (200 tokens)]
           → Process all 5,200 tokens

Request 2: [System prompt (5K tokens)] + [User message B (300 tokens)]
           → Process all 5,300 tokens (recomputes the same 5K prefix!)
```

The system prompt, tool definitions, and CLAUDE.md contents are identical across turns — reprocessing them is pure waste.

## How It Works

```
Request 1:
  [System prompt] → Compute KV states → Cache them
  [User message A] → Compute remaining → Generate response

Request 2:
  [System prompt] → Cache HIT → Skip computation
  [User message B] → Compute only this part → Generate response
```

The provider stores KV states for exact prefix matches. Subsequent requests with the same prefix skip the expensive forward pass for those tokens.

## Cost Impact

### Anthropic Pricing

| Operation | Sonnet 4.6 | Savings vs Full Input |
|---|---|---|
| Input tokens (no cache) | $3.00 / MTok | — |
| Cache write (first request) | $3.75 / MTok | -25% (slightly more expensive) |
| Cache read (subsequent) | $0.30 / MTok | **90% cheaper** |

A [[Claude Code]] session where 80% of tokens are cached prefix:
```
Without caching: 200K tokens × $3.00/MTok = $0.60 per turn
With caching:    160K cached × $0.30/MTok + 40K new × $3.00/MTok = $0.168 per turn
Savings: 72%
```

### OpenAI
Similar concept with their "cached input tokens" — discounted rate for repeated prefixes.

## What Gets Cached

### In AI Coding Harnesses

| Component | Cached? | Why |
|---|---|---|
| System prompt | Yes — identical every turn | Largest single prefix component |
| Tool definitions | Yes — stable within a session | ~5K–20K tokens of JSON schemas |
| CLAUDE.md / AGENTS.md | Yes — loaded once | Project instructions don't change mid-session |
| Conversation history | Partially — prefix grows each turn | Earlier turns form stable prefix |
| Latest user message | No — different each time | This is the variable suffix |
| Tool results | No — different each time | File contents, command output |

### The Stable Prefix / Variable Suffix Pattern

[[OpenAI Codex]] explicitly structures prompts for maximum cache hits:

```
┌────────────────────────────────────┐
│ System prompt                      │ ← Stable prefix (cached)
│ Tool definitions                   │
│ AGENTS.md / CLAUDE.md              │
│ Conversation turns 1...N-1         │
├────────────────────────────────────┤
│ Latest tool results                │ ← Variable suffix (not cached)
│ Current user message               │
└────────────────────────────────────┘
```

Anything appended to the end doesn't break the cache for the prefix.

## Cache Behavior

### Cache Lifetime
- **Anthropic:** ~5 minutes for ephemeral cache; 1 hour for extended cache
- **OpenAI:** Automatic, managed by the platform
- Caches are per-organization, not shared across users

### Cache Invalidation
The cache is keyed on **exact token prefix**. Any change to the prefix invalidates the cache:
- Changing the system prompt → cache miss
- Reordering tools → cache miss
- Adding a new conversation turn → prefix grows, previous cache still valid for the shared portion

### Minimum Cacheable Length
- **Anthropic:** Minimum 1,024 tokens for cache eligibility (Sonnet), 2,048 for Opus
- Shorter prefixes aren't worth caching (overhead exceeds savings)

## Prompt Caching vs KV Caching

| Concept | Scope | Where |
|---|---|---|
| **[[Inference Optimization#KV Caching|KV caching]]** | Within a single generation | Inside the model, per-request |
| **Prompt caching** | Across multiple requests | At the API/infrastructure level |

KV caching avoids recomputing past tokens within one generation (every inference engine does this). Prompt caching avoids recomputing shared prefixes across different requests.

## Designing for Cache Efficiency

### Do
- Put stable content (system prompt, tools, instructions) at the beginning
- Keep the order of tool definitions consistent
- Load CLAUDE.md/AGENTS.md early in the prompt
- Use `/compact` to compress conversation history (reduces variable suffix)

### Don't
- Put timestamps or request IDs in the system prompt (breaks cache)
- Randomly reorder tool definitions between requests
- Include per-request metadata in the prefix
- Change system prompt wording unnecessarily between turns

## Impact on [[AI Coding Harnesses]]

| Harness | Caching Strategy |
|---|---|
| [[Claude Code]] | Automatic via Anthropic API; stable system prompt + tools as prefix |
| [[OpenAI Codex]] | Explicit prefix/suffix split for cache optimization; stateless design sends full context each request to maximize cache reuse |
| [[OpenCode]] | Uses Vercel AI SDK; caching depends on chosen provider |

For a typical coding session with 20 turns, prompt caching can reduce total cost by **50–70%** compared to no caching.

## Related

- [[Inference Optimization]]
- [[Context Management]]
- [[Context Engineering]]
- [[Tokenomics of AI]]
- [[Tokens and Tokenization]]
- [[AI Coding Harnesses]]
