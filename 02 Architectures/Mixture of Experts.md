# Mixture of Experts

Mixture of Experts (MoE) is a [[Neural Networks|neural network]] architecture where only a subset of the model's parameters are activated for each input token. This allows models to have massive total parameter counts while keeping per-token compute manageable.

## The Core Idea

In a dense model, every parameter participates in every computation. In an MoE model, a **router** selects which "expert" sub-networks process each token:

```
Token → Router → Expert 3, Expert 7 (selected from 16 experts)
                 ↓
            Combined output
```

Only 2 out of 16 experts fire per token. The model has the *knowledge capacity* of all 16 experts but the *compute cost* of 2.

## Architecture

### Standard Transformer Layer
```
Input → Attention → Feed-Forward Network (FFN) → Output
```

### MoE Transformer Layer
```
Input → Attention → Router → [Expert FFN 1] → Output
                           → [Expert FFN 2]
                           → (other experts idle)
```

The attention layer stays shared (dense). Only the Feed-Forward layers are replaced with expert modules. The router is typically a small learned linear layer that produces probability scores for each expert.

### Key Parameters

| Term | Definition |
|---|---|
| **Total parameters** | Sum of all expert parameters (e.g., 132B) |
| **Active parameters** | Parameters used per token (e.g., 12B) |
| **Experts** | Number of parallel FFN modules (e.g., 8, 16, 64) |
| **Top-k** | How many experts are selected per token (typically 2) |
| **Router** | Learned gating network that selects experts |

## Notable MoE Models

| Model | Total Params | Active Params | Experts | Top-k |
|---|---|---|---|---|
| Mixtral 8x7B | 47B | ~12B | 8 | 2 |
| Mixtral 8x22B | 141B | ~39B | 8 | 2 |
| DeepSeek V3 | 671B | ~37B | 256 | 8 |
| GPT-4 (rumored) | ~1.8T | ~280B | 16 | 2 |
| LLaMA 4 | Various | Various | MoE variants | — |
| Qwen 3 Coder | 480B | ~48B | — | — |

## Why MoE Matters

### More Capability Per FLOP
An MoE model with 132B total params and 12B active performs closer to a 70B dense model while running at the speed of a 12B model. You get "big model quality at small model cost."

### Efficient [[Inference Optimization|Inference]]
- **Lower latency** — Only active parameters are computed
- **Higher throughput** — Less compute per token means more tokens per second
- **Better for serving** — Production systems care about per-request cost

### Scaling Without Proportional Cost
[[Scaling Laws]] show performance improves with parameters. MoE lets you scale parameters without proportionally scaling compute. DeepSeek V3's 671B parameters would be impractical as a dense model — but with only 37B active, it's servable.

## Challenges

### Load Balancing
If the router sends most tokens to the same few experts, the others are wasted:
- **Auxiliary loss** — Penalty term encouraging balanced expert utilization
- **Expert capacity** — Hard limits on how many tokens each expert can process
- **Still imperfect** — Some experts tend to specialize and dominate

### Memory
All expert weights must be in memory, even if most are idle:
- A 47B MoE model needs ~47B parameters in memory (not just the 12B active)
- This limits MoE's advantage on memory-constrained devices
- **Expert offloading** — Keep inactive experts in CPU RAM, load to GPU on demand

### Training Instability
Router training can be unstable:
- Experts can "collapse" (all tokens route to one expert)
- Requires careful initialization and auxiliary losses
- More complex training infrastructure than dense models

### Expert Specialization
Ideally, different experts learn different domains (one for code, one for math, one for language). In practice, specialization is partial and hard to control.

## MoE vs Dense: When to Use Which

| Scenario | Better Choice |
|---|---|
| Maximum quality per FLOP | MoE |
| Memory-constrained deployment | Dense (smaller active footprint) |
| Simple training pipeline | Dense |
| Very large models (>100B) | MoE (only practical option) |
| Local inference on consumer hardware | Dense (quantized) |
| Cloud serving at scale | MoE |

## Related

- [[Neural Networks]]
- [[Transformers]]
- [[Scaling Laws]]
- [[Inference Optimization]]
- [[Large Language Models]]
- [[Open Source AI Models]]
