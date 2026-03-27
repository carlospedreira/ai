# Activation Functions

Activation functions introduce non-linearity into [[Neural Networks]], allowing them to learn complex patterns. Without activations, stacking layers would be equivalent to a single linear transformation — no matter how many layers you add.

## Why Non-Linearity?

A linear function of a linear function is still linear:
```
f(x) = 3x + 2
g(x) = 5x - 1
g(f(x)) = 5(3x + 2) - 1 = 15x + 9  ← still linear!
```

Real-world patterns (language, images, code) are non-linear. Activation functions after each layer let the network model these complex relationships.

## Common Activation Functions

### ReLU (Rectified Linear Unit)

```
ReLU(x) = max(0, x)
```

```
Output
  │      /
  │     /
  │    /
  │   /
──┼──/─────── Input
  │
```

- **The default** for most hidden layers since 2012
- Simple, fast, effective
- **Dying ReLU problem** — Neurons that output 0 for all inputs stop learning

### Leaky ReLU

```
LeakyReLU(x) = x if x > 0, else 0.01x
```

- Small negative slope prevents dying neurons
- Almost as fast as ReLU
- Often used as a drop-in replacement

### GELU (Gaussian Error Linear Unit)

```
GELU(x) = x · Φ(x)   where Φ is the cumulative normal distribution
```

- **Used in [[Transformers]]** — GPT, BERT, Claude, LLaMA
- Smoother than ReLU — provides better gradient flow
- Slightly more expensive to compute, but performance gains justify it

### SiLU / Swish

```
SiLU(x) = x · σ(x)   where σ is sigmoid
```

- Similar to GELU, smoother than ReLU
- Used in LLaMA, Mistral, and other modern LLMs
- Self-gated — the input gates itself

### Sigmoid

```
σ(x) = 1 / (1 + e^(-x))
```

```
Output
 1 │        ────────
   │      /
0.5│    /
   │  /
 0 │────────
   └──────────────── Input
```

- Maps to (0, 1) — interpretable as probability
- Used in: binary classification output, gating mechanisms
- **Problem:** Saturates at extremes → vanishing gradients → mostly replaced by ReLU in hidden layers

### Tanh

```
tanh(x) = (e^x - e^-x) / (e^x + e^-x)
```

- Maps to (-1, 1) — zero-centered (better than sigmoid)
- Used in: RNNs, some normalization layers
- Same saturation problem as sigmoid

### Softmax

```
softmax(x_i) = e^(x_i) / Σ e^(x_j)
```

- Converts a vector of raw scores into probabilities that sum to 1
- Used in: multi-class classification output, [[Attention Mechanism]] (attention weights)
- Not used in hidden layers — it's an output activation

## Activation Functions in Transformers

A typical [[Transformers|Transformer]] uses:

| Location | Activation | Why |
|---|---|---|
| Feed-forward layer | GELU or SiLU | Non-linearity between the two linear projections |
| Attention scores | Softmax | Convert raw scores to probability-like weights |
| Gating mechanisms | Sigmoid | Binary gates (used in GLU variants) |
| Output layer | Softmax | Probability over vocabulary for next token prediction |

### GLU Variants (Gated Linear Units)

Modern LLMs increasingly use gated activations in the feed-forward layer:

```
FFN_GLU(x) = (xW₁ · σ(xW_gate)) · W₂
```

Where σ is an activation function. Variants:
- **SwiGLU** — SiLU as the gate activation (LLaMA, Mistral)
- **GeGLU** — GELU as the gate activation
- **ReGLU** — ReLU as the gate activation

SwiGLU has become the standard for modern LLMs — it consistently outperforms standard GELU FFN layers at the same parameter count.

## Choosing an Activation

| Use Case | Recommended |
|---|---|
| Hidden layers (general) | ReLU or Leaky ReLU |
| [[Transformers]] FFN | GELU, SiLU, or SwiGLU |
| Binary classification output | Sigmoid |
| Multi-class output | Softmax |
| Gating mechanisms | Sigmoid |
| When in doubt | GELU (safe modern default) |

## Related

- [[Neural Networks]]
- [[Transformers]]
- [[Deep Learning]]
- [[Attention Mechanism]]
- [[Loss Functions and Optimization]]
