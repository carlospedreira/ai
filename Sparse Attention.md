# Sparse Attention

Sparse attention is a family of techniques that reduce the quadratic cost of the [[Attention Mechanism]] by having each token attend to only a subset of other tokens, rather than all of them. This is critical for enabling long context windows in [[Large Language Models]].

## The Problem: Quadratic Cost

Standard self-attention is O(n²) in sequence length:
- 4K tokens → 16M attention computations per layer
- 128K tokens → 16B attention computations per layer
- 1M tokens → 1T attention computations per layer

This quadratic scaling makes long-context models extremely expensive. Sparse attention patterns trade some modeling capacity for dramatically better efficiency.

## Attention Patterns

### Full (Dense) Attention
Every token attends to every other token. Maximum expressiveness, maximum cost.
```
■ ■ ■ ■ ■ ■ ■ ■
■ ■ ■ ■ ■ ■ ■ ■
■ ■ ■ ■ ■ ■ ■ ■
■ ■ ■ ■ ■ ■ ■ ■
■ ■ ■ ■ ■ ■ ■ ■
■ ■ ■ ■ ■ ■ ■ ■
```

### Sliding Window (Local) Attention
Each token only attends to its w nearest neighbors:
```
■ ■ ■ · · · · ·
■ ■ ■ ■ · · · ·
· ■ ■ ■ ■ · · ·
· · ■ ■ ■ ■ · ·
· · · ■ ■ ■ ■ ·
· · · · ■ ■ ■ ■
```

- **Complexity:** O(n × w) — linear in sequence length
- **Trade-off:** Cannot directly attend to distant tokens
- **Used by:** Mistral (window size 4096), Longformer (local component)

### Global + Local
Designate certain tokens as "global" (attend to everything), while others attend locally:
```
■ ■ ■ ■ ■ ■ ■ ■   ← global token
■ ■ ■ ■ · · · ·
■ ■ ■ ■ ■ · · ·
■ · ■ ■ ■ ■ · ·
■ · · ■ ■ ■ ■ ·
■ · · · ■ ■ ■ ■
```

- Global tokens (e.g., [CLS], first token) serve as information hubs
- **Used by:** Longformer, BigBird (Google), LED

### Dilated (Strided) Attention
Attend to every k-th token, covering a wider range with the same compute:
```
■ · ■ · ■ · ■ ·
· ■ · ■ · ■ · ■
■ · ■ · ■ · ■ ·
· ■ · ■ · ■ · ■
```

- Captures long-range patterns with gaps
- Often combined with local attention at different heads

### Block-Sparse Attention
Divide the sequence into blocks; each block attends to itself and selected other blocks:
```
■ ■ · · ■ ■ · ·
■ ■ · · ■ ■ · ·
· · ■ ■ · · ■ ■
· · ■ ■ · · ■ ■
■ ■ · · ■ ■ · ·
■ ■ · · ■ ■ · ·
```

- Hardware-friendly (block operations are efficient on GPUs)
- **Used by:** BigBird, Sparse Transformer (OpenAI)

## Multi-Scale Approaches

### Mixture of Attention Heads
Different heads in the same layer use different patterns:
- Head 1: local window (nearby context)
- Head 2: strided (medium range)
- Head 3: global (long range)

Combined, they cover all distances efficiently.

### Hierarchical Attention
Process at multiple resolutions:
```
Level 1: Token-level attention (local, fine-grained)
Level 2: Sentence-level attention (medium range)
Level 3: Paragraph-level attention (global, coarse)
```

## Notable Implementations

| Method | Pattern | Complexity | Used By |
|---|---|---|---|
| **Sliding Window** | Local only | O(n × w) | Mistral, Mixtral |
| **Longformer** | Local + global | O(n × (w + g)) | Longformer, LED |
| **BigBird** | Local + global + random | O(n × (w + g + r)) | BigBird |
| **Sparse Transformer** | Block-sparse (strided + local) | O(n√n) | OpenAI (2019) |
| **Flash Attention** | Dense but memory-efficient | O(n²) compute, O(n) memory | Most modern LLMs |
| **Ring Attention** | Distributed dense across devices | O(n²/d) per device | Research |

## Flash Attention: Not Sparse, But Solves the Memory Problem

[[Attention Mechanism#Flash Attention|Flash Attention]] doesn't reduce computation but eliminates the memory bottleneck:
- Standard: Materializes full n×n attention matrix in GPU HBM → O(n²) memory
- Flash: Tiles computation into GPU SRAM blocks, never materializes full matrix → O(n) memory

This is why most production LLMs use Flash Attention with dense patterns rather than true sparse attention — the memory savings enable long contexts without approximation.

## The Practical Landscape (2026)

| Context Length | Typical Approach |
|---|---|
| ≤ 8K | Dense attention + Flash Attention |
| 8K–128K | Dense + Flash + RoPE scaling |
| 128K–1M | Sliding window + global tokens, or dense + Flash on large GPUs |
| > 1M | Ring Attention, hierarchical, or sparse patterns required |

Most frontier models (Claude, GPT, Gemini) use **dense attention with Flash Attention** up to their context limit, relying on hardware improvements rather than sparse approximations. Sparse attention remains important for efficiency-focused and open-source models.

## Related

- [[Attention Mechanism]]
- [[Transformers]]
- [[Inference Optimization]]
- [[Positional Encoding]]
- [[Context Management]]
- [[Parallel Processing in AI]]
