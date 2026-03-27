# State Space Models

State Space Models (SSMs) are an emerging alternative to [[Transformers]] that combine the training parallelism of attention-based models with the efficient sequential inference of [[Recurrent Neural Networks]]. Mamba, the most prominent SSM, has shown competitive performance with Transformers while scaling linearly with sequence length.

## The Motivation

Transformers have a fundamental tension:
- **Training:** Parallel (fast) but O(n²) in sequence length
- **Inference:** Sequential token generation with growing KV cache

SSMs aim to get the best of both worlds:
- **Training:** Parallel (like Transformers)
- **Inference:** Constant memory per step (like RNNs), no KV cache growth

## What Is a State Space Model?

An SSM maps an input sequence to an output sequence through a hidden state:

```
x(t) → [State update: h(t) = Ah(t-1) + Bx(t)]  → [Output: y(t) = Ch(t) + Dx(t)]
```

Where:
- **A** — State transition matrix (how the hidden state evolves)
- **B** — Input projection (how input affects the state)
- **C** — Output projection (how state produces output)
- **D** — Direct feedthrough (skip connection)

This is a **linear recurrence** — the same structure used in control theory and signal processing for decades.

## The Dual View

The key insight: the same SSM can be computed two ways:

### As a Recurrence (Inference)
Process one token at a time, maintaining a fixed-size hidden state:
```
h₁ = A·h₀ + B·x₁
h₂ = A·h₁ + B·x₂
h₃ = A·h₂ + B·x₃
```
**O(1) memory per step** — no growing cache.

### As a Convolution (Training)
Unroll the recurrence into a convolution, process the entire sequence in parallel:
```
y = conv(x, kernel)  where kernel is derived from A, B, C
```
**Fully parallelizable** on GPUs, like Transformers.

## Mamba (Gu & Dao, 2023)

Mamba is the breakthrough SSM architecture. Its key innovation: **selective state spaces** — the matrices A, B, C are input-dependent (not fixed), allowing the model to selectively remember or forget information based on content.

### Why "Selective" Matters

Fixed SSM: treats every token the same way (like a fixed filter)
Selective SSM: decides per-token what to remember and what to ignore (like attention, but cheaper)

```
Input: "The capital of France is Paris. The weather today is sunny."
Query: "What is the capital of France?"

Selective SSM: strongly remembers "France" and "Paris", weakly stores "weather" and "sunny"
```

### Mamba Architecture

```
Input → Linear → Conv1D → Selective SSM → Linear → Output
         ↓                                   ↑
         └──────── SiLU gate ────────────────┘
```

No attention layers. No MLP blocks. Just SSM blocks stacked deep.

### Performance

| Aspect | Mamba | Transformer |
|---|---|---|
| Training speed | Comparable | Baseline |
| Inference speed | 5x faster at long sequences | Baseline |
| Memory (inference) | Constant (no KV cache) | Grows linearly with sequence |
| Quality (language) | Competitive at small scale | Superior at frontier scale |
| Long-range modeling | Excellent (linear complexity) | O(n²) cost |

## Mamba-2 and Beyond

**Mamba-2 (2024):** Showed that selective SSMs are mathematically related to a structured form of attention, unifying the two paradigms. 2–8x faster than Mamba-1.

**Hybrid architectures:** The most promising direction combines SSM layers with a few attention layers:
- **Jamba** (AI21) — Interleaves Mamba and Transformer layers + MoE
- **Zamba** — Shared attention layers with Mamba blocks
- **Griffin** (Google DeepMind) — Gated linear recurrence + local attention

These hybrids get SSM efficiency for most of the model while retaining attention's ability to perform precise retrieval when needed.

## SSMs vs Transformers vs RNNs

| Property | [[Recurrent Neural Networks\|RNNs]] | [[Transformers]] | SSMs (Mamba) |
|---|---|---|---|
| Training parallelism | No | Yes | Yes |
| Inference memory | O(1) | O(n) KV cache | O(1) |
| Long-range modeling | Weak (despite LSTM) | Strong (direct attention) | Strong (selective memory) |
| Compute per token | O(1) | O(n) attention | O(1) |
| Current frontier quality | Poor | Best | Competitive, improving |
| Hardware optimization | Minimal | Extensive (Flash Attention) | Growing |

## Why This Matters

### For Long Context
[[Context Management]] is expensive with Transformers because the KV cache grows linearly. SSMs maintain constant memory regardless of context length — a 1M token context costs the same per-token inference as 1K tokens.

### For Edge Deployment
No KV cache means SSMs can run on memory-constrained devices where Transformer inference is impossible.

### For [[AI Coding Harnesses]]
The [[Agentic Coding Paradigm#The Agent Loop|agent loop]] generates many tokens across many turns. SSMs' efficient inference could significantly reduce the cost and latency of long coding sessions.

### The Open Question
As of 2026, no pure SSM model has matched the quality of frontier Transformers (Claude Opus, GPT-5) at the largest scales. The question is whether this is a fundamental limitation or just a matter of scaling and engineering investment.

## Related

- [[Transformers]]
- [[Recurrent Neural Networks]]
- [[Attention Mechanism]]
- [[Sparse Attention]]
- [[Inference Optimization]]
- [[Context Management]]
- [[Neural Networks]]
