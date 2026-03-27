# Tokenomics of AI

Tokenomics of AI describes the economics of token-based pricing — how AI API costs work, what drives them, and how to optimize spending. Understanding tokenomics is essential for anyone building AI-powered applications or using [[AI Coding Harnesses]].

## How Pricing Works

AI APIs charge per token processed:

```
Cost = (Input tokens × Input price) + (Output tokens × Output price)
```

Output tokens are always more expensive than input tokens (generation is more compute-intensive than comprehension).

## Current Pricing (Early 2026)

### Anthropic (Claude)

| Model | Input / MTok | Output / MTok |
|---|---|---|
| Claude Opus 4.6 | $5.00 | $25.00 |
| Claude Sonnet 4.6 | $3.00 | $15.00 |
| Claude Haiku 4.5 | $1.00 | $5.00 |
| Prompt cache read (Sonnet) | $0.30 | — |
| Batch API | 50% discount | 50% discount |

### OpenAI

| Model | Input / MTok | Output / MTok |
|---|---|---|
| GPT-5.4 | ~$2.50 | ~$10.00 |
| GPT-5.3-Codex | $1.75 | $14.00 |
| GPT-5.4-mini | ~$0.40 | ~$1.60 |
| codex-mini-latest | $1.50 | $6.00 |

### Google (Gemini)

| Model | Input / MTok | Output / MTok |
|---|---|---|
| Gemini 2.5 Pro | $1.25 | $10.00 |
| Gemini 2.5 Flash | $0.15 | $0.60 |

### Open Source (Self-Hosted)
No per-token cost — only hardware and electricity. But capital expenditure for GPUs can be significant. See [[AI Hardware and Compute]].

## Cost Anatomy of a Coding Agent Session

A typical 30-minute [[Claude Code]] session:

| Component | Tokens | Cost (Sonnet) |
|---|---|---|
| System prompt + tool definitions | ~5,000 | ~$0.015 |
| CLAUDE.md loaded | ~2,000 | ~$0.006 |
| 20 file reads (avg 500 lines each) | ~100,000 | ~$0.30 |
| 10 grep/glob results | ~20,000 | ~$0.06 |
| 5 bash command outputs | ~10,000 | ~$0.03 |
| Conversation (user + assistant) | ~30,000 | ~$0.09 |
| Model output (edits, reasoning) | ~15,000 | ~$0.225 |
| Extended thinking tokens | ~20,000 | Varies |
| **Total** | **~200,000** | **~$0.75** |

But this compounds: each turn includes the full conversation history, so later turns process more tokens. A long session can cost $5–$50+.

### The Quadratic Problem

With each turn, the model re-reads the entire conversation:
```
Turn 1:  5K tokens processed
Turn 2:  10K tokens processed (5K old + 5K new)
Turn 3:  15K tokens processed
...
Turn 20: 100K tokens processed
Total:   1M+ tokens processed across all turns
```

This is why [[Inference Optimization|prompt caching]] and [[Context Management|compaction]] matter so much.

## Cost Optimization Strategies

### 1. Prompt Caching
Reuse computed results for repeated prefixes:
- System prompt, tool definitions, CLAUDE.md are the same every turn
- Cached reads cost ~10x less than fresh processing
- [[OpenAI Codex]] specifically structures prompts for cache optimization

### 2. Model Selection
Use the right model for each task:
- **Opus/GPT-5.4** for complex reasoning, architecture decisions
- **Sonnet/GPT-5.3** for daily coding tasks
- **Haiku/GPT-5.4-mini** for simple tasks, sub-agent exploration
- [[Claude Code]]'s `opusplan` mode: Opus for planning, Sonnet for execution

### 3. Context Management
Keep the context window lean:
- Use `/compact` before context fills up
- Write good CLAUDE.md (actionable, not verbose)
- Don't read entire large files — use targeted searches
- See [[Context Engineering]]

### 4. Batch API
For non-time-sensitive work, batch API offers 50% discount:
- Code review of many PRs
- Documentation generation
- Test generation

### 5. Smaller, Focused Tasks
Break large tasks into smaller sessions:
- Each session starts with a fresh, clean context
- Avoids the quadratic cost growth of long sessions
- Better results too (less context pollution)

## Subscription vs API

| Aspect | Subscription (Pro/Max) | API (Pay-per-token) |
|---|---|---|
| Predictability | Fixed monthly cost | Variable |
| Cost at low usage | Expensive | Cheap |
| Cost at high usage | Cheap (unlimited within limits) | Expensive |
| Rate limits | 5-hour rolling window | Tokens per minute |
| Best for | Daily interactive use | CI/CD, automation, apps |

## The Economics of AI Coding

### Cost per PR
Estimates for AI-assisted development:
- Simple bug fix: $0.50–$2
- Feature implementation: $2–$20
- Complex refactoring: $10–$100
- Autonomous agent session: $5–$50

### ROI Calculation
If an AI coding agent saves a developer 2 hours/day at $100/hour effective rate:
- Value: ~$200/day
- Cost: ~$20–$50/day in tokens
- ROI: 4–10x

### The "Infinite Intern" Framing
AI agents are like interns that:
- Work 24/7, never tired
- Cost $0.50–$5 per task
- Need review but not hand-holding
- Scale horizontally (run 10 agents in parallel)

## Related

- [[Tokens and Tokenization]]
- [[Inference Optimization]]
- [[AI Hardware and Compute]]
- [[Context Management]]
- [[Context Engineering]]
- [[AI Coding Harnesses]]
- [[Claude Code]]
