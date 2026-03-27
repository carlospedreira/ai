# Positional Encoding

Positional encoding is the mechanism that gives [[Transformers]] a sense of order. Unlike RNNs, which process tokens sequentially, the [[Attention Mechanism]] treats all positions equally — without positional encoding, "the cat ate the fish" and "the fish ate the cat" would be identical to the model.

## The Problem

Self-attention computes relationships between all pairs of tokens simultaneously. This parallelism is a strength (fast training), but it means the model has no inherent notion of which token comes first, second, or last.

Positional encoding injects position information so the model can learn that word order matters.

## Approaches

### Sinusoidal (Original Transformer, 2017)

The original "Attention Is All You Need" paper used fixed sinusoidal functions:

```
PE(pos, 2i)   = sin(pos / 10000^(2i/d))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d))
```

Where `pos` is the position and `i` is the dimension index. These vectors are added to the token embeddings before the first attention layer.

Properties:
- **Fixed** — Not learned, computed deterministically
- **Unique** — Each position gets a distinct pattern
- **Relative distances** — The dot product between positional encodings encodes relative distance
- **Extrapolation** — Can theoretically generalize to longer sequences than seen in training (in practice, poorly)

### Learned Positional Embeddings

Train a separate embedding for each position:

```
Position 0 → [0.12, -0.34, ...] (learned)
Position 1 → [0.45, 0.23, ...]  (learned)
...
Position N → [...]
```

Used by: GPT-2, BERT, early GPT-3

**Limitation:** Fixed maximum sequence length — can't handle positions beyond what was trained.

### Rotary Position Embeddings (RoPE)

The dominant method in modern [[Large Language Models]]. Encodes position by rotating the query and key vectors in 2D subspaces:

```
q_rotated = rotate(q, θ·position)
k_rotated = rotate(k, θ·position)
```

The attention score between two tokens then naturally depends on their relative distance — because the dot product of two rotated vectors depends on the angle between them (i.e., the difference in positions).

Properties:
- **Relative** — Encodes relative position, not absolute
- **Decays with distance** — Naturally reduces attention for distant tokens
- **Extrapolatable** — With extensions, can generalize beyond training length
- **Efficient** — Applied as a rotation during attention, minimal overhead

Used by: LLaMA, Mistral, Qwen, Gemma, Falcon, most modern open-source models, Claude

### ALiBi (Attention with Linear Biases)

Instead of modifying embeddings, add a linear bias to attention scores based on distance:

```
attention_score(i, j) = q_i · k_j - m · |i - j|
```

Where `m` is a head-specific slope. Closer tokens get higher attention scores.

Properties:
- **Simple** — Just a bias term, no modification to embeddings
- **Extrapolates well** — Generalizes to longer sequences than training
- **No extra parameters** — Slopes are fixed per head

Used by: BLOOM, MPT

## Why This Matters for Context Windows

The ability to handle long sequences — 200K, 1M, or more tokens — depends heavily on positional encoding:

### The Length Extrapolation Problem
Models trained with max 4K positions struggle at 8K+ positions because:
- Learned embeddings have no representation for position 8001+
- Sinusoidal encodings theoretically extrapolate but practically degrade
- Attention patterns learned at short distances don't transfer to long distances

### Solutions for Long Context

| Technique | Approach |
|---|---|
| **RoPE scaling** | Interpolate or extend RoPE frequencies for longer sequences |
| **YaRN** | Yet another RoPE extension — adjusts frequency bands |
| **NTK-aware scaling** | Scales the base frequency of RoPE |
| **Continued pre-training** | Train further on long documents with extended positions |
| **Sliding window attention** | Attend locally (nearby tokens) + globally (special tokens) |

Modern models like Claude (1M tokens) and Gemini (1M tokens) use combinations of these techniques to achieve practical long-context performance.

### Impact on [[AI Coding Harnesses]]

Long context windows enable:
- Reading entire codebases in one pass
- Maintaining long conversation histories
- Processing large diffs and log outputs

But [[Context Engineering]] still matters — filling 1M tokens with irrelevant content degrades attention quality regardless of positional encoding.

## Related

- [[Transformers]]
- [[Attention Mechanism]]
- [[Large Language Models]]
- [[Context Management]]
- [[Tokens and Tokenization]]
