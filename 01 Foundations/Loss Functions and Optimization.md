# Loss Functions and Optimization

Loss functions measure how wrong a model's predictions are. Optimizers use the loss to update model weights. Together, they form the learning engine of [[Neural Networks]] and [[Large Language Models]].

## Loss Functions

A loss function (also called cost function or objective function) takes the model's prediction and the correct answer, and returns a number indicating how far off the prediction is. The goal of [[Training and Inference|training]] is to minimize this number.

### Cross-Entropy Loss

The standard loss for classification and language modeling:

```
Loss = -Σ y_true · log(y_pred)
```

For [[Large Language Models]], this becomes **next-token prediction loss**: at each position, the model predicts a probability distribution over the vocabulary, and cross-entropy measures how well that distribution matches the actual next token.

- **Lower loss = better predictions**
- A loss of 0 means perfect prediction (impossible in practice for language)
- Typical LLM training loss curves decrease from ~10 to ~2-3 over training

### Mean Squared Error (MSE)

Standard loss for regression (predicting continuous values):

```
Loss = (1/n) · Σ(y_pred - y_true)²
```

Used in: price prediction, temperature forecasting, embedding training.

### Contrastive Loss

Used for learning embeddings (see [[Embeddings and Vector Search]]):

```
Pull similar items closer, push dissimilar items apart
```

Variants:
- **Triplet loss** — Anchor, positive, negative triplets
- **InfoNCE** — Used in CLIP (connecting images and text)
- **SimCLR** — Self-supervised contrastive learning

### Reward Model Loss

For [[Fine-Tuning and Alignment|RLHF]]:

```
Loss = -log(σ(r(chosen) - r(rejected)))
```

The reward model learns to assign higher scores to human-preferred responses.

### DPO Loss

[[Fine-Tuning and Alignment|Direct Preference Optimization]] combines the reward model and policy optimization into a single loss:

```
Loss = -log(σ(β · (log π(chosen)/π_ref(chosen) - log π(rejected)/π_ref(rejected))))
```

Directly optimizes the policy model on preference pairs without a separate reward model.

## Optimizers

Optimizers determine *how* to update weights based on the loss gradient.

### Gradient Descent

The fundamental algorithm:

```
weight = weight - learning_rate × gradient
```

Move weights in the direction that reduces loss. The learning rate controls step size.

### SGD (Stochastic Gradient Descent)

Update weights using a random subset (mini-batch) of data instead of the full dataset:
- Much faster per step than full-batch gradient descent
- Introduces noise that can help escape local minima
- Foundation of all modern training

### Adam (Adaptive Moment Estimation)

The default optimizer for most deep learning:

```
m = β₁ · m + (1-β₁) · gradient           (momentum)
v = β₂ · v + (1-β₂) · gradient²          (velocity)
weight = weight - lr · m / (√v + ε)
```

- **Adaptive learning rates** — Each parameter gets its own learning rate
- **Momentum** — Smooths updates using exponential moving average
- **Fast convergence** — Works well with minimal tuning
- Used by most LLM training runs

### AdamW

Adam with decoupled weight decay — the standard for LLM training:
- Weight decay applied separately from the gradient update
- Produces better generalization than vanilla Adam
- Default for Hugging Face Transformers, PyTorch Lightning, etc.

## Learning Rate Schedules

The learning rate often changes during training:

### Warmup
Start with a very small learning rate and gradually increase:
```
lr = base_lr × (step / warmup_steps)
```
Prevents early instability when gradients are large and noisy.

### Cosine Decay
After warmup, decrease the learning rate following a cosine curve:
```
lr = base_lr × 0.5 × (1 + cos(π × step / total_steps))
```
Smooth transition from exploration to refinement.

### The Standard LLM Schedule
```
Learning Rate
    ↑
    │    /‾‾‾‾‾‾‾‾‾‾‾‾‾‾\
    │   /                  \
    │  /                    \______
    │ /
    └──────────────────────────────→ Training Steps
      Warmup    Constant/Peak    Cosine Decay
```

## Backpropagation

The algorithm that computes gradients for every weight in the network:

1. **Forward pass** — Compute the output from input
2. **Compute loss** — Compare output to target
3. **Backward pass** — Use the chain rule to compute ∂Loss/∂weight for every weight
4. **Update** — Apply the optimizer

For a network with L layers:
```
∂Loss/∂w_L = local gradient at layer L
∂Loss/∂w_{L-1} = ∂Loss/∂w_L × ∂w_L/∂w_{L-1}   (chain rule)
...propagate back to layer 1
```

### Gradient Problems

| Problem | Cause | Solution |
|---|---|---|
| **Vanishing gradients** | Gradients shrink through many layers | Residual connections, normalization |
| **Exploding gradients** | Gradients grow exponentially | Gradient clipping |
| **Dead neurons** | ReLU outputs zero for all inputs | Leaky ReLU, careful initialization |

Modern [[Transformers]] use residual connections and layer normalization to keep gradients well-behaved through hundreds of layers.

## Related

- [[Neural Networks]]
- [[Training and Inference]]
- [[Large Language Models]]
- [[Fine-Tuning and Alignment]]
- [[Scaling Laws]]
- [[Embeddings and Vector Search]]
