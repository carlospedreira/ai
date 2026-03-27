# Backpropagation

Backpropagation (backprop) is the algorithm that makes [[Neural Networks]] learn. It efficiently computes the gradient of the [[Loss Functions and Optimization|loss function]] with respect to every weight in the network, enabling gradient descent to update weights in the direction that reduces error.

## The Core Problem

A neural network might have billions of weights. After a forward pass produces a prediction, the loss tells us *how wrong* the prediction is — but not *which weights* are responsible or *how to fix them*.

Backpropagation answers: "For each weight, how much would a tiny change to that weight affect the loss?"

This is the **gradient**: ∂Loss/∂w for every weight w.

## The Chain Rule

Backprop is fundamentally an application of the calculus chain rule. If a network computes:

```
x → Layer₁ → z₁ → Layer₂ → z₂ → Layer₃ → output → Loss
```

Then by the chain rule:

```
∂Loss/∂w₁ = ∂Loss/∂output × ∂output/∂z₂ × ∂z₂/∂z₁ × ∂z₁/∂w₁
```

Each term is a **local gradient** — how much one layer's output affects the next. The chain rule multiplies them to get the total effect of any weight on the loss.

## The Two Passes

### Forward Pass
Data flows from input to output, computing activations at each layer:

```
Input x
  → z₁ = W₁x + b₁ → a₁ = ReLU(z₁)
  → z₂ = W₂a₁ + b₂ → a₂ = ReLU(z₂)
  → z₃ = W₃a₂ + b₃ → output = softmax(z₃)
  → Loss = CrossEntropy(output, target)
```

All intermediate values (z₁, a₁, z₂, a₂, ...) are **stored** — they're needed for the backward pass.

### Backward Pass
Gradients flow from loss back to input, computing ∂Loss/∂w at each layer:

```
∂Loss/∂output  (how loss changes with output — from loss function derivative)
  → ∂Loss/∂z₃  (how loss changes with pre-activation — backprop through softmax)
  → ∂Loss/∂W₃  (gradient for weights in layer 3)
  → ∂Loss/∂a₂  (how loss changes with layer 2 output)
  → ∂Loss/∂z₂  (backprop through ReLU)
  → ∂Loss/∂W₂  (gradient for weights in layer 2)
  → ...continue to layer 1
```

At each layer, we compute:
1. **∂Loss/∂W** — the gradient for this layer's weights (used for update)
2. **∂Loss/∂input** — the gradient to pass to the previous layer

## Computational Efficiency

A naive approach would compute each weight's gradient independently — with billions of weights, this is impossibly expensive.

Backprop's insight: **reuse intermediate gradients**. The gradient ∂Loss/∂z₂ is computed once and used for both:
- Computing ∂Loss/∂W₂ (this layer's weight gradient)
- Computing ∂Loss/∂a₁ (the gradient passed backward)

This makes backprop **O(n)** in the number of operations — the same cost as a forward pass. Computing all gradients is approximately 2–3× the cost of a single forward pass.

## Backprop Through Specific Operations

### Linear Layer (y = Wx + b)
```
∂Loss/∂W = ∂Loss/∂y × xᵀ
∂Loss/∂b = ∂Loss/∂y
∂Loss/∂x = Wᵀ × ∂Loss/∂y
```

### ReLU (y = max(0, x))
```
∂Loss/∂x = ∂Loss/∂y × (1 if x > 0 else 0)
```
Zero gradient for negative inputs — this is the "dying ReLU" problem (see [[Activation Functions]]).

### Softmax + Cross-Entropy
The combined gradient simplifies beautifully:
```
∂Loss/∂z = predicted - target
```
The gradient is just the difference between what the model predicted and what was correct.

### [[Attention Mechanism|Attention]]
Backprop through self-attention requires computing gradients through the softmax, the QKᵀ matrix multiply, and the value projection — this is where the O(n²) memory cost of attention comes from (need to store the full attention matrix for the backward pass). [[Attention Mechanism#Flash Attention|Flash Attention]] solves this by recomputing attention during the backward pass instead of storing it.

## Automatic Differentiation

Modern [[AI Development Frameworks]] handle backprop automatically:

```python
# PyTorch
loss = model(input, target)
loss.backward()        # Backprop happens here — all gradients computed
optimizer.step()       # Update weights using computed gradients
```

The framework builds a **computation graph** during the forward pass, then traverses it backward to compute all gradients. Developers never implement backprop manually.

## Historical Significance

- **1960s–70s:** Chain rule applied to networks (Linnainmaa, Werbos)
- **1986:** Rumelhart, Hinton & Williams popularized backprop for neural networks in the landmark paper "Learning Representations by Back-Propagating Errors"
- **Without backprop**, deep learning as we know it would be impossible — there's no other known efficient way to compute gradients for deep networks

## Challenges at Scale

### Memory
The forward pass stores all intermediate activations for the backward pass. For a large [[Transformers|Transformer]] with a long sequence:
- Activations can consume hundreds of GB of GPU memory
- **Gradient checkpointing** trades compute for memory: recompute activations during backward pass instead of storing them

### Gradient Problems
See [[Loss Functions and Optimization#Gradient Problems]]:
- **Vanishing gradients** — Mitigated by [[Residual Connections]], [[Normalization Techniques]]
- **Exploding gradients** — Mitigated by gradient clipping
- Both problems are why deep networks were impractical before modern architectural innovations

### Distributed Backprop
In [[Parallel Processing in AI|multi-GPU training]], gradients computed on different GPUs must be synchronized:
```
GPU 0: forward + backward → gradients₀ ─┐
GPU 1: forward + backward → gradients₁ ──┤→ All-reduce average → update weights
GPU 2: forward + backward → gradients₂ ──┤
GPU 3: forward + backward → gradients₃ ──┘
```

This communication is a major bottleneck in large-scale training.

## Related

- [[Neural Networks]]
- [[Loss Functions and Optimization]]
- [[Training and Inference]]
- [[Deep Learning]]
- [[Activation Functions]]
- [[Residual Connections]]
- [[AI Development Frameworks]]
- [[Parallel Processing in AI]]
