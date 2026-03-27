# Scaling Laws

Scaling laws are empirical relationships showing that [[Large Language Models|LLM]] performance improves predictably as you increase compute, data, and model parameters. They are the theoretical foundation for why AI labs train ever-larger models.

## The Core Insight

Performance (measured as loss) follows a **power law** with respect to three variables:

```
Loss ∝ 1/N^α · 1/D^β · 1/C^γ
```

Where:
- **N** = number of model parameters
- **D** = dataset size (tokens)
- **C** = compute budget (FLOPs)
- **α, β, γ** = scaling exponents

As any of these increase, loss decreases smoothly and predictably — no sudden jumps, no plateaus (until diminishing returns set in).

## Key Papers

### Kaplan et al. (2020) — OpenAI
First systematic study. Found that:
- Performance scales as a power law with model size, dataset size, and compute
- Larger models are more sample-efficient (learn more from less data)
- Recommended spending most of the budget on larger models, not more data

### Chinchilla (2022) — DeepMind
Revised the compute-optimal ratio. Found that Kaplan et al. over-invested in model size:
- **Chinchilla scaling law**: For compute-optimal training, parameters and training tokens should scale roughly equally
- A 70B model trained on 1.4T tokens outperformed a 280B model trained on 300B tokens
- Implication: many models were undertrained relative to their size

### Scaling Laws for Transfer (2024+)
More recent work shows scaling laws also apply to:
- Fine-tuning performance
- In-context learning ability
- Reasoning capabilities (with sufficient data diversity)

## Why Scaling Laws Matter

### For AI Labs
They make training predictable:
- Estimate performance of a $100M training run by extrapolating from $1M experiments
- Determine optimal model size for a given compute budget
- Justify massive infrastructure investments

### For Practitioners
They explain model behavior:
- Why larger models "unlock" emergent abilities
- Why [[Fine-Tuning and Alignment|fine-tuning]] a larger base model usually beats training a smaller model longer
- Why [[Context Management|more context]] generally helps (up to a point)

## Emergent Abilities

At sufficient scale, models develop capabilities not present in smaller versions:

| Capability | Approximate Scale |
|---|---|
| Basic instruction following | ~1B parameters |
| Few-shot learning | ~10B parameters |
| Chain-of-thought reasoning | ~100B parameters |
| Complex multi-step planning | ~100B+ with RLHF |
| Tool use and agentic behavior | Frontier models (2024+) |

There's debate about whether these are truly "emergent" or whether they're gradual improvements that cross a usefulness threshold.

## Diminishing Returns and the "Wall"

Scaling laws predict smooth improvement, but practical limits exist:

- **Data exhaustion** — We may be running out of high-quality text data
- **Compute costs** — Training frontier models costs $100M+, approaching $1B
- **Energy** — Power consumption of training clusters raises sustainability concerns
- **Inference costs** — Larger models are more expensive to serve per query

This has driven interest in:
- **[[Inference Optimization]]** — Making large models cheaper to run
- **Mixture of Experts (MoE)** — Only activating a subset of parameters per token
- **Distillation** — Training smaller models to match larger ones
- **Better data** — Quality over quantity (synthetic data, curated corpora)

## Scaling Compute at Inference (Test-Time Compute)

A newer scaling axis: instead of training a bigger model, give the existing model more compute at inference time:

- **Extended thinking** — [[Claude Code]]'s effort levels, OpenAI's o-series reasoning
- **Chain-of-thought** — More reasoning tokens = better answers
- **Iterative refinement** — Multiple passes over the problem
- **Search** — Explore multiple solution paths (tree of thought)

This is sometimes called "test-time compute scaling" and follows its own power laws.

## Related

- [[Large Language Models]]
- [[Training and Inference]]
- [[Inference Optimization]]
- [[AI Hardware and Compute]]
- [[Tokens and Tokenization]]
