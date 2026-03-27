# Residual Connections

Residual connections (skip connections) are a fundamental architectural component that allows information to bypass layers in a [[Neural Networks|neural network]]. They are essential to training deep [[Transformers]] and were one of the most important innovations in [[Deep Learning]].

## The Problem: Degradation

Naively stacking more layers should produce better models (more capacity to learn). In practice, very deep networks performed *worse* than shallower ones — not from overfitting, but from optimization difficulty. Gradients either vanished or exploded as they passed through dozens of layers, making learning impossible.

## The Solution: Skip Connections

Instead of learning a direct mapping F(x), learn the **residual** — the difference from identity:

```
Output = F(x) + x
```

The layer only needs to learn what to *add* to the input, not the entire transformation:

```
  x ─────────────────────┐
  │                       │ (skip / identity)
  ▼                       │
┌─────────┐               │
│ Layer   │               │
│ (learn  │               │
│ residual│               │
│  F(x))  │               │
└────┬────┘               │
     │                    │
     ▼                    ▼
   F(x)    +             x
            ▼
        F(x) + x  ← output
```

## Why It Works

### Gradient Highway
Gradients can flow directly through the skip connection during backpropagation:

```
∂Loss/∂x = ∂Loss/∂output · (∂F(x)/∂x + 1)
                                         ↑
                          This +1 from the skip connection
                          prevents gradients from vanishing
```

Even if ∂F(x)/∂x is small, the gradient is at least 1 — it never vanishes completely.

### Easy to Learn Identity
If a layer isn't useful, it can learn F(x) = 0, making the output just x (identity). The network can effectively "skip" layers it doesn't need, making depth risk-free.

### Ensemble Effect
A network with residual connections can be viewed as an ensemble of many shorter networks, since information can take paths of different lengths through the skip connections.

## In Transformers

Every [[Transformers|Transformer]] layer uses two residual connections:

```
x → [LayerNorm → Attention] + x → [LayerNorm → FFN] + x
                              ↑                        ↑
                          residual                 residual
```

Without these skip connections, training a 96-layer Transformer (like GPT-3) would be practically impossible.

## History: ResNet (2015)

Kaiming He et al. introduced residual connections in **ResNet** for [[Computer Vision]]:
- Won ImageNet 2015 with 152-layer network
- Previous best was ~20 layers
- Enabled training networks 10x deeper
- One of the most cited papers in deep learning

ResNet showed that with skip connections, adding more layers never hurts — performance either improves or stays the same.

## Variants

| Variant | Modification | Used By |
|---|---|---|
| **Pre-activation residual** | Norm → Activation → Layer → Add | ResNet v2, modern Transformers |
| **Dense connections** | Skip to *all* subsequent layers | DenseNet |
| **Weighted residual** | α · F(x) + x (learned scale) | Some Transformer variants |
| **Highway networks** | Gated skip connections | Precursor to residual connections |

## Scale Matters

At very large model scales (100B+ parameters), residual connection design becomes critical:
- **Residual scaling** — Scale F(x) by a small constant (e.g., 1/√depth) to prevent activations from growing with depth
- **Post-norm vs pre-norm** — Where [[Normalization Techniques|normalization]] goes relative to the residual affects training stability
- DeepSeek V3 uses careful residual scaling for its 671B MoE architecture

## Related

- [[Neural Networks]]
- [[Transformers]]
- [[Deep Learning]]
- [[Normalization Techniques]]
- [[Loss Functions and Optimization]]
- [[Attention Mechanism]]
