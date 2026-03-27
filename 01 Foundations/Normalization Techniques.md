# Normalization Techniques

Normalization is a family of methods that stabilize and accelerate [[Neural Networks|neural network]] training by controlling the distribution of activations as data flows through layers. Without normalization, deep networks are notoriously difficult to train — gradients vanish or explode, and training becomes sensitive to initialization and learning rate.

## Why Normalization?

As data passes through many layers, the distribution of activations can shift dramatically (**internal covariate shift**). Each layer must constantly adapt to changing input distributions from the layer before it.

Normalization fixes this by re-centering and re-scaling activations at each layer:

```
x_normalized = (x - mean) / sqrt(variance + ε)
output = γ · x_normalized + β
```

Where γ (scale) and β (shift) are learned parameters that let the model undo the normalization if needed.

## Variants

### Batch Normalization (BatchNorm, 2015)

Normalize across the **batch dimension** — compute mean and variance over all examples in a mini-batch for each feature:

```
Batch of N examples, each with D features:
For each feature d: mean_d = average over N examples
                     var_d = variance over N examples
```

- **Impact:** Enabled much deeper networks, higher learning rates, faster convergence
- **Limitation:** Depends on batch size — small batches produce noisy statistics
- **Limitation:** Doesn't work well for sequence models (batch stats are meaningless across different sequence positions)
- **Mostly used in:** [[Computer Vision]] (CNNs)

### Layer Normalization (LayerNorm, 2016)

Normalize across the **feature dimension** — compute mean and variance over all features for each individual example:

```
Single example with D features:
mean = average over D features
var = variance over D features
```

- **Batch-independent** — Works the same whether batch size is 1 or 1000
- **Perfect for sequences** — Each token normalized independently
- **The standard in [[Transformers]]** — Used in GPT, BERT, Claude, LLaMA, etc.

### RMSNorm (Root Mean Square Normalization)

A simplified variant of LayerNorm that skips the mean subtraction:

```
x_normalized = x / RMS(x)
RMS(x) = sqrt(mean(x²) + ε)
```

- **~15% faster** than full LayerNorm (no mean computation)
- **Equal or better performance** in practice
- **Used by:** LLaMA, Mistral, Gemma, most modern open-source LLMs

### Group Normalization

Split features into groups, normalize within each group:
- Compromise between BatchNorm (entire batch) and LayerNorm (entire feature set)
- Useful for vision tasks with small batch sizes

### Instance Normalization

Normalize each feature map of each example independently:
- Mainly used in style transfer and image generation

## Where Normalization Goes in Transformers

### Pre-Norm (Modern Standard)
```
Input → LayerNorm → Attention → Residual → LayerNorm → FFN → Residual
```

Normalization happens **before** the attention/FFN sublayer. Used by GPT-2+, LLaMA, Claude, most modern models.

Benefits:
- More stable training at large scale
- Gradients flow more cleanly through residual connections

### Post-Norm (Original Transformer)
```
Input → Attention → Residual → LayerNorm → FFN → Residual → LayerNorm
```

Normalization happens **after** the sublayer. Used in the original "Attention Is All You Need" paper and BERT.

- Can achieve slightly better final performance
- But harder to train, especially for deep models

## Impact on Training

| Without Normalization | With Normalization |
|---|---|
| Learning rate must be very small | Can use much larger learning rates |
| Training is slow and unstable | Training is fast and stable |
| Deep networks often fail to train | Deep networks train reliably |
| Sensitive to weight initialization | Robust to initialization |
| Gradients vanish or explode | Gradients stay in a healthy range |

## Related

- [[Neural Networks]]
- [[Transformers]]
- [[Loss Functions and Optimization]]
- [[Training and Inference]]
- [[Deep Learning]]
