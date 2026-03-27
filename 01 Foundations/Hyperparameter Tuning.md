# Hyperparameter Tuning

Hyperparameters are the settings chosen *before* training begins — they control the training process itself rather than being learned from data. Tuning them is both art and science, and getting them wrong can mean the difference between a breakthrough model and a waste of compute.

## Parameters vs Hyperparameters

| Type | What | Set By | Examples |
|---|---|---|---|
| **Parameters** | Model weights and biases | Learning (gradient descent) | Attention weights, FFN matrices |
| **Hyperparameters** | Training configuration | Human (or search algorithm) | Learning rate, batch size, layers |

## Key Hyperparameters

### Learning Rate

The single most important hyperparameter. Controls the step size during [[Loss Functions and Optimization|gradient descent]]:

```
weight = weight - learning_rate × gradient
```

| Too High | Just Right | Too Low |
|---|---|---|
| Loss oscillates or diverges | Loss decreases smoothly | Loss decreases but agonizingly slowly |
| NaN values | Converges to good minimum | May get stuck in suboptimal minimum |
| Wasted compute | Efficient training | Wasted compute |

Typical ranges:
- Pre-training LLMs: 1e-4 to 6e-4 (with warmup + cosine decay)
- Fine-tuning: 1e-5 to 5e-5
- LoRA fine-tuning: 1e-4 to 3e-4

### Batch Size

Number of examples processed before a weight update:

| Small Batch (32–256) | Large Batch (1K–4M) |
|---|---|
| Noisy gradients (regularizing) | Smooth gradients (stable) |
| Less GPU memory | Requires more GPUs |
| More weight updates per epoch | Fewer updates per epoch |
| Often generalizes better | Can get stuck in sharp minima |

LLM pre-training uses very large batches (millions of tokens per step) because:
- Gradient noise is already low with massive datasets
- Large batches maximize GPU utilization
- Communication overhead is amortized over more computation

### Number of Layers (Depth)

More layers = more capacity to learn complex patterns, but:
- Harder to train (vanishing gradients, though [[Residual Connections]] help)
- More memory and compute
- Diminishing returns past a point (see [[Scaling Laws]])

### Hidden Dimension (Width)

The size of the model's internal representations:
- Wider = more expressive per layer
- Trade-off with depth: same parameter count can be deep-and-narrow or shallow-and-wide
- [[Transformers]]: d_model (e.g., 4096 for LLaMA 3 8B, 8192 for 70B)

### Number of Attention Heads

How many parallel [[Attention Mechanism|attention]] computations per layer:
- More heads = more types of relationships captured
- Each head has dimension d_model / n_heads
- Typical: 32 heads for d_model=4096, 64 for d_model=8192

### Context Length

Maximum sequence length the model can process:
- Longer = more capable but quadratically more expensive (attention is O(n²))
- Affects [[Positional Encoding]] design
- Modern range: 4K to 1M tokens

### Dropout Rate

Fraction of neurons randomly disabled during training (see [[Dropout and Regularization]]):
- 0.0: No dropout (common in modern LLMs)
- 0.1: Light dropout (BERT-era Transformers)
- 0.5: Heavy dropout (classic CNNs)

### Weight Decay

L2 regularization strength (see [[Dropout and Regularization]]):
- Typical for LLMs: 0.01–0.1
- Applied via AdamW optimizer

## Tuning Methods

### Manual Tuning
The most common approach in LLM training:
- Start with published values from similar models (LLaMA paper, GPT-3 paper)
- Adjust based on training curves
- Requires experience and intuition

### Grid Search
Try all combinations of a predefined set:
```
learning_rate: [1e-5, 1e-4, 1e-3]
batch_size: [32, 64, 128]
→ 9 experiments
```
Exhaustive but expensive. Impractical for LLMs (each run costs millions).

### Random Search
Sample hyperparameters randomly from ranges:
```
learning_rate ~ LogUniform(1e-5, 1e-3)
batch_size ~ Choice([32, 64, 128, 256])
→ N random experiments
```
More efficient than grid search — **Bergstra & Bengio (2012)** showed random search finds good hyperparameters faster because not all hyperparameters are equally important.

### Bayesian Optimization
Use a probabilistic model to predict which hyperparameters will work best, then try those:
```
1. Try a few random experiments
2. Build a model predicting performance from hyperparameters
3. Choose the next experiment that maximizes expected improvement
4. Repeat
```
Tools: Optuna, Ray Tune, Weights & Biases Sweeps

### μP (Maximal Update Parameterization)
See [[Weight Initialization#The μP Paradigm]]. Tune hyperparameters on a small model, transfer them to the large model. This is the most practical approach for LLM pre-training, where each experiment costs millions.

## LLM Pre-Training Recipes

Published hyperparameters from major models:

| | LLaMA 3 8B | LLaMA 3 70B | Chinchilla 70B |
|---|---|---|---|
| Learning rate | 3e-4 | 1.5e-4 | 1e-4 |
| Batch size (tokens) | 4M | 4M | 1.5M |
| Warmup steps | 2,000 | 2,000 | — |
| LR schedule | Cosine decay | Cosine decay | Cosine decay |
| Weight decay | 0.1 | 0.1 | 0.1 |
| Dropout | 0.0 | 0.0 | 0.0 |
| Optimizer | AdamW | AdamW | AdamW |
| Adam β₁, β₂ | 0.9, 0.95 | 0.9, 0.95 | 0.9, 0.95 |
| Gradient clipping | 1.0 | 1.0 | 1.0 |

These are starting points, not gospel — each training run requires monitoring and adjustment.

## Monitoring Training

The training curve tells you if hyperparameters are working:

```
Good:           Concerning:        Bad:
Loss ↓          Loss ↓             Loss ↗ or NaN
  ╲               ╲╱╲               ╱
   ╲               ╲  ╲            ╱
    ╲___             ╲  ╲___     ╱
                                ╱
Smooth decay    Oscillating    Diverging
```

Key signals:
- **Loss spike then recovery** → Learning rate might be too high
- **Loss plateau** → Learning rate too low, or model capacity reached
- **NaN** → Learning rate way too high, or numerical instability
- **Training loss drops but validation doesn't** → Overfitting

## Related

- [[Training and Inference]]
- [[Loss Functions and Optimization]]
- [[Neural Networks]]
- [[Scaling Laws]]
- [[Weight Initialization]]
- [[Dropout and Regularization]]
- [[AI Development Frameworks]]
