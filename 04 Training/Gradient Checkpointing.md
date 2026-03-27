# Gradient Checkpointing

Gradient checkpointing (activation checkpointing, rematerialization) is a memory optimization technique for training [[Neural Networks]] that trades compute time for GPU memory. It makes training large [[Transformers]] and [[Large Language Models]] possible on limited hardware.

## The Memory Problem

During [[Backpropagation]], the backward pass needs the intermediate activations computed during the forward pass. A standard approach stores *all* activations:

```
Forward pass: stores a₁, a₂, a₃, ..., a₉₆ (for a 96-layer model)
Backward pass: reads a₉₆, a₉₅, ..., a₁ to compute gradients
```

For a large Transformer:
- Each layer stores attention matrices, feed-forward activations, residual states
- A 70B model with a 4K sequence might need **200+ GB** of activation memory
- This is on top of the model weights, optimizer states, and gradients
- Often more memory than any single GPU has

## The Solution: Checkpoint, Don't Store

Instead of storing every activation, store only a subset (**checkpoints**) and **recompute** the rest during the backward pass:

```
Forward pass: store a₁, a₄, a₇ (every 3rd layer — checkpoints)
              discard a₂, a₃, a₅, a₆, a₈, a₉

Backward pass at layer 6:
  Need a₅, a₆ → recompute from checkpoint a₄:
  a₄ → forward through layers 5,6 → get a₅, a₆
  → compute gradients for layers 5,6
  → discard a₅, a₆
```

### The Trade-off

| Aspect | No Checkpointing | Full Checkpointing |
|---|---|---|
| Activation memory | O(L) — all layers | O(√L) — only checkpoints |
| Compute | 1× forward + 1× backward | 1× forward + ~1.33× backward |
| Total compute overhead | Baseline | ~33% more |
| Memory savings | None | 3–10× less activation memory |

The extra compute comes from recomputing activations between checkpoints. With √L checkpoints for L layers, the optimal strategy recomputes each activation at most once.

## Strategies

### Uniform Checkpointing
Checkpoint every k-th layer:
```
k=1: No checkpointing (store everything)
k=3: Store every 3rd layer → ~3× memory reduction, ~33% more compute
k=√L: Optimal balance → √L× memory reduction
```

### Selective Checkpointing
Checkpoint only the most memory-hungry operations:
- [[Attention Mechanism|Self-attention]] activations are the largest (O(n²) for sequence length n)
- Feed-forward activations are smaller
- Strategy: checkpoint attention, store feed-forward

### No Checkpointing for Cheap Operations
Some activations are cheap to recompute (e.g., [[Activation Functions|ReLU]]: just check if > 0). Don't bother storing these — always recompute.

## In Practice

### PyTorch

```python
from torch.utils.checkpoint import checkpoint

class TransformerLayer(nn.Module):
    def forward(self, x):
        # Wrap the expensive part in checkpoint()
        x = checkpoint(self.self_attention, x, use_reentrant=False)
        x = checkpoint(self.feed_forward, x, use_reentrant=False)
        return x
```

### DeepSpeed and FSDP
Distributed training frameworks apply checkpointing automatically:
```python
# DeepSpeed config
{
    "activation_checkpointing": {
        "partition_activations": true,
        "contiguous_memory_optimization": true,
        "number_checkpoints": null  // auto-optimal
    }
}
```

### Hugging Face Transformers
```python
model.gradient_checkpointing_enable()
```

One line enables checkpointing across all Transformer layers.

## Interaction with Other Techniques

| Technique | Saves | Complements Checkpointing? |
|---|---|---|
| **Mixed precision (FP16/BF16)** | 2× weight memory | Yes — reduces both weight and activation memory |
| **[[Parallel Processing in AI\|Tensor parallelism]]** | Splits layers across GPUs | Yes — checkpointing reduces per-GPU activation memory |
| **[[Inference Optimization\|Quantization]]** | Weight memory (inference only) | N/A — quantization is for inference, checkpointing for training |
| **[[Attention Mechanism#Flash Attention\|Flash Attention]]** | Attention memory | Synergistic — Flash doesn't store the full attention matrix |
| **CPU offloading** | GPU memory | Alternative — slower but saves more memory |

## Flash Attention + Checkpointing

Flash Attention already avoids materializing the O(n²) attention matrix. Combined with gradient checkpointing:
- Flash Attention: never stores the attention matrix forward *or* backward
- Checkpointing: avoids storing intermediate layer activations
- Together: enables training with very long sequences on modest hardware

This combination is what makes training models with 128K+ token context windows practical.

## The Memory Budget

Training a Transformer requires memory for:
```
Total = Weights + Gradients + Optimizer states + Activations
```

| Component | Memory (FP16, 70B model) |
|---|---|
| Weights | ~140 GB |
| Gradients | ~140 GB |
| Optimizer (Adam) | ~280 GB (2× weights for momentum + velocity) |
| Activations | 200+ GB (without checkpointing) → ~30 GB (with) |
| **Total** | ~760 GB → ~590 GB |

Checkpointing saves ~170 GB in this example — the difference between needing 10 vs 8 GPUs.

## Related

- [[Backpropagation]]
- [[Training and Inference]]
- [[Neural Networks]]
- [[Parallel Processing in AI]]
- [[AI Hardware and Compute]]
- [[Attention Mechanism]]
- [[AI Development Frameworks]]
