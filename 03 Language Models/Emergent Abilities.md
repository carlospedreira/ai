# Emergent Abilities

Emergent abilities are capabilities that appear in [[Large Language Models]] only at sufficient scale — they are absent in smaller models but suddenly appear as models grow larger in parameters, data, or compute. They are one of the most fascinating and debated phenomena in AI.

## What Counts as "Emergent"?

A capability is considered emergent if:
1. It is **absent** in smaller models (near-random performance)
2. It **appears** at a certain scale threshold
3. The transition is **sharp** — not a gradual improvement

```
Performance
    ↑
    │                    ╱──── (above threshold: capability present)
    │                   ╱
    │                  ╱
    │─────────────────╱  ← sharp transition
    │ (below threshold: random performance)
    └──────────────────────→ Model Scale
```

## Notable Emergent Abilities

### At ~1-10B Parameters
- Basic instruction following
- Simple summarization
- Translation between common language pairs

### At ~10-100B Parameters
- **Few-shot learning** — Learn from examples in the prompt
- **[[Chain of Thought]]** — Step-by-step reasoning
- **Code generation** — Write functional programs
- **Arithmetic** — Multi-digit addition and subtraction

### At ~100B+ Parameters
- **Complex reasoning** — Multi-step logical inference
- **Self-correction** — Identify and fix errors in own output
- **Theory of mind** — Reason about what other agents know or believe
- **Tool use** — Decide when and how to call external functions
- **Agentic behavior** — Plan, execute, and iterate toward goals

### Frontier (2024+)
- **Extended reasoning** — Sustained multi-step problem solving (o1, Claude with extended thinking)
- **Multi-agent coordination** — [[AI Agents and Sub-Agents|Agent teams]] working together
- **Complex code refactoring** — Cross-file, architectural-level changes
- **Research-level mathematics** — Olympiad-level problem solving

## The Debate

### The "Phase Transition" View
Some researchers argue emergent abilities represent genuine phase transitions — qualitative shifts in capability that cannot be predicted from smaller-scale experiments. This view supports the importance of [[Scaling Laws]] and continued investment in larger models.

### The "Mirage" View
Schaeffer et al. (2023) argued that many apparent emergent abilities are artifacts of **metric choice**, not true phase transitions:
- When using **nonlinear metrics** (exact match accuracy), performance looks like a sudden jump
- When using **linear metrics** (token-level accuracy, log-likelihood), performance improves smoothly and predictably
- The "emergence" is in the metric, not the model

Example:
```
Exact match accuracy on arithmetic:
  10B model: 0% (needs to get every digit right)
  100B model: 80% (gets most digits right)
  → Looks like a sudden jump!

Per-digit accuracy:
  10B model: 60% (gets most digits right, but some wrong → 0% exact match)
  100B model: 95% (gets nearly all digits right → 80% exact match)
  → Smooth, predictable improvement
```

### The Current Consensus
Both views have merit:
- Many reported emergent abilities are likely smooth improvements measured with coarse metrics
- But some capabilities (complex reasoning, tool use) do appear to have genuine threshold effects
- The truth matters for predicting what future models will be capable of

## Why It Matters

### For AI Labs
If abilities are truly emergent and unpredictable:
- Can't forecast what the next model will be able to do
- Safety evaluation is harder (capabilities might surprise us)
- Justifies investment in scale (bigger models = new abilities)

If abilities are smooth and predictable:
- Can extrapolate from small-scale experiments
- Safety evaluation can be planned in advance
- The "bitter lesson" (just scale up) has limits

### For [[AI Safety and Alignment Research]]
Emergent capabilities are a core safety concern:
- If a model suddenly develops the ability to deceive, that's dangerous
- [[AI Safety and Alignment Research#Deception and Situational Awareness|Deceptive alignment]] might emerge without warning
- This is why Anthropic's ASL framework evaluates capabilities at each scale

### For [[AI Coding Harnesses]]
Each model generation unlocks new agentic capabilities:
- GPT-3 couldn't reliably use tools → GPT-4 could
- Early Claude couldn't coordinate sub-agents → Claude Opus 4.6 can run agent teams
- The next generation may handle week-long autonomous tasks reliably

## Emergent Abilities in Coding

| Capability | Model Scale Needed | Impact |
|---|---|---|
| Single-line completion | Small (1-7B) | Autocomplete |
| Function generation | Medium (7-30B) | Copilot-style |
| Multi-file editing | Large (30-100B+) | Agent mode |
| Codebase-wide reasoning | Frontier (100B+) | Architecture-level refactoring |
| Autonomous task completion | Frontier with RLHF | [[Autonomous Coding Agents]] |
| Multi-agent coordination | Latest frontier | [[AI Agents and Sub-Agents\|Agent teams]] |

## Related

- [[Scaling Laws]]
- [[Large Language Models]]
- [[AI Safety and Alignment Research]]
- [[Chain of Thought]]
- [[AI Benchmarks]]
- [[AI Agents and Sub-Agents]]
- [[Agentic Coding Paradigm]]
