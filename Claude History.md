# Claude History

Claude is Anthropic's family of [[Large Language Models]], designed with safety as a core principle. The Claude lineage represents the most safety-focused approach among frontier AI labs, and Claude models power [[Claude Code]] — one of the most widely adopted [[AI Coding Harnesses]].

## Anthropic's Origins

Anthropic was founded in 2021 by Dario Amodei (CEO) and Daniela Amodei (President), along with several former OpenAI researchers. The founding motivation: build frontier AI *with safety as the primary focus*, not an afterthought.

Key founding principles:
- **[[AI Safety and Alignment Research|Constitutional AI]]** — Align models using explicit principles
- **Responsible scaling** — Match safety measures to capability level (ASL framework)
- **Interpretability** — Understand what models learn and why

## The Claude Timeline

### Claude 1 (March 2023)

| Property | Value |
|---|---|
| Context window | 9K tokens (later 100K) |
| Key feature | Constitutional AI alignment |
| Significance | Anthropic's first public model; demonstrated that safety training doesn't require sacrificing capability |

The 100K context window variant (May 2023) was groundbreaking — 10× larger than GPT-4's 8K window at the time. First model that could process an entire book or codebase in one pass.

### Claude 2 (July 2023)

| Property | Value |
|---|---|
| Context window | 100K tokens |
| Key improvements | Better coding, math, reasoning |
| Significance | Competitive with GPT-4 on many benchmarks |

Established Claude as a serious frontier model, not just a safety research project.

### Claude 3 Family (March 2024)

The first multi-tier family with models optimized for different use cases:

| Model | Parameters | Context | Positioned For |
|---|---|---|---|
| Claude 3 Haiku | Smallest | 200K | Speed and cost |
| Claude 3 Sonnet | Medium | 200K | Balance of speed and capability |
| Claude 3 Opus | Largest | 200K | Maximum capability |

Claude 3 Opus briefly claimed the #1 position on many LLM benchmarks. The family introduced Anthropic's tiered naming convention (Haiku/Sonnet/Opus) that continues today.

### Claude 3.5 Sonnet (June 2024)

| Property | Value |
|---|---|
| Context window | 200K tokens |
| Key feature | Coding breakthrough — best-in-class on coding benchmarks |
| Significance | A mid-tier model that matched or exceeded Claude 3 Opus on most tasks |

The model that sparked the [[AI Coding Harnesses]] revolution. Claude 3.5 Sonnet's coding ability was so strong that it became the model of choice for developers, directly leading to [[Claude Code]]'s development.

**Updated (October 2024):** A refreshed version further improved coding benchmarks, becoming the clear leader in code generation quality.

### Claude 3.5 Haiku (October 2024)

Fast, cheap, surprisingly capable. Used as the default for sub-agents and lightweight tasks in [[Claude Code]].

### Claude 4 (May 2025)

Released alongside Claude Code's general availability:

| Model | Key Improvement |
|---|---|
| Claude 4 Opus | Strongest reasoning, new capability tier |
| Claude 4 Sonnet | Coding quality rivaling Claude 3 Opus at Sonnet speeds |

### Claude 4.5 Haiku (2025)

Updated Haiku with improved capability, used internally by Claude Code for Explore sub-agents and fast tasks.

### Claude Opus 4.6 (February 2026)

| Property | Value |
|---|---|
| Context window | 1M tokens |
| Max output | 128K tokens |
| Key features | Agent teams, interleaved thinking, adaptive effort |

The current flagship. Key capabilities:
- **Agent teams** — Multiple Claude instances coordinating in parallel (see [[Multi-Agent Systems]])
- **Interleaved thinking** — Extended reasoning between tool calls, not just at the start
- **Adaptive thinking** — Dynamically adjusts compute based on task complexity
- **1M context** — Processes enormous codebases in a single pass

### Claude Sonnet 4.6 (February 2026)

| Property | Value |
|---|---|
| Context window | 1M tokens |
| Max output | 64K tokens |
| Key feature | Default for [[Claude Code]] on Pro plan |

The workhorse model — balances capability and cost for daily development.

## Constitutional AI

Claude's distinctive alignment approach (see [[AI Safety and Alignment Research]]):

1. **Define principles** — A "constitution" of rules the model should follow
2. **Self-critique** — Model evaluates its own responses against the constitution
3. **Self-revision** — Model improves its responses based on self-critique
4. **RLAIF** — Train on AI-generated preference data (not just human labels)

This produces a model that is:
- Helpful without being sycophantic
- Honest about uncertainty (says "I don't know" rather than hallucinating)
- Harmless without being overly restrictive

## Claude's Character

Anthropic explicitly designs Claude's personality:
- **Honest** — Acknowledges uncertainty, doesn't fabricate
- **Careful** — Errs toward safety but doesn't refuse reasonable requests
- **Thoughtful** — Considers implications before acting
- **Direct** — Provides clear answers without unnecessary hedging
- **Curious** — Engages genuinely with the user's problem

This "character training" is distinct from raw capability — it's about how Claude communicates, not what it can do.

## ASL Framework

Anthropic's Responsible Scaling Policy uses AI Safety Levels:

| Level | Capabilities | Safeguards |
|---|---|---|
| ASL-1 | No meaningful risk | Standard |
| ASL-2 | Some dangerous capabilities | Current Claude safeguards (current level) |
| ASL-3 | Substantially elevated risk | Enhanced monitoring, security |
| ASL-4+ | Hypothetical extreme risk | Triggers safety pause |

Each new model is evaluated against ASL criteria before deployment. Anthropic commits to not deploying models that exceed their current safeguard level.

## Claude and Coding

The Claude family's coding capability trajectory:

```
Claude 1:     Basic coding assistance
Claude 2:     Competitive code generation
Claude 3.5S:  Best-in-class coding → sparked Claude Code development
Claude 4:     Agent-grade coding (multi-file, tool use)
Claude 4.6:   Agent teams, autonomous coding at scale
```

Claude models are the foundation of [[Claude Code]], and their coding capabilities directly shape what the harness can do. The progression from "can write a function" to "can coordinate agent teams on complex refactoring" happened in under 3 years.

## Market Impact

- Anthropic surpassed **$2.5B annualized revenue** by early 2026
- **350K+ daily active users** of Claude Code
- **1M+ accepted pull requests** generated through Claude Code
- Claude Code rated **"most loved" by 46%** of developers in surveys

## Related

- [[Large Language Models]]
- [[Claude Code]]
- [[AI Safety and Alignment Research]]
- [[Reinforcement Learning from Human Feedback]]
- [[AI History and Timeline]]
- [[GPT Evolution]]
- [[Scaling Laws]]
- [[Instruction Tuning]]
- [[AI Coding Harnesses]]
