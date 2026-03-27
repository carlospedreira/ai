# Weight Initialization

Weight initialization determines the starting values of a [[Neural Networks|neural network's]] parameters before [[Training and Inference|training]] begins. Bad initialization can prevent learning entirely — gradients vanish, explode, or the network converges to a poor solution. Good initialization gives [[Backpropagation]] a fighting chance.

## Why It Matters

At initialization, the network makes random predictions. The gradient signal from the [[Loss Functions and Optimization|loss function]] must propagate back through every layer to update every weight. If activations or gradients have the wrong scale at the start:

- **Too large** → activations saturate, gradients explode, NaN losses
- **Too small** → activations collapse to zero, gradients vanish, no learning
- **Just right** → stable signal flow, training proceeds smoothly

The goal: keep activations and gradients at a consistent scale across all layers.

## Common Initialization Methods

### Zero Initialization (Don't Do This)

```
All weights = 0
```

**Catastrophic failure:** Every neuron computes the same output, receives the same gradient, and remains identical forever. The symmetry is never broken — the network effectively has one neuron per layer.

### Random Normal / Uniform

```
W ~ N(0, σ²)  or  W ~ Uniform(-a, a)
```

Better than zeros (breaks symmetry), but the scale σ or a matters enormously. Too large or too small causes the same problems.

### Xavier / Glorot Initialization (2010)

Designed for layers with sigmoid or tanh [[Activation Functions]]:

```
W ~ N(0, 2 / (fan_in + fan_out))
or
W ~ Uniform(-√(6 / (fan_in + fan_out)), √(6 / (fan_in + fan_out)))
```

Where fan_in = input size, fan_out = output size.

**Insight:** Scale weights so that the variance of activations is preserved across layers. This prevents both vanishing and exploding signals.

### Kaiming / He Initialization (2015)

Designed for ReLU-family [[Activation Functions]]:

```
W ~ N(0, 2 / fan_in)
```

**Why different from Xavier?** ReLU zeros out half the activations (negative values), so the surviving activations need 2× the variance to maintain signal scale. The factor of 2 compensates for this.

**Default in PyTorch** for `nn.Linear` and `nn.Conv2d`.

### Scaled Initialization for Transformers

Modern [[Transformers]] use additional scaling:

```
W ~ N(0, 1 / √d_model)  for most layers
W ~ N(0, 1 / √(2 · n_layers · d_model))  for residual projections
```

The extra 1/√(2·n_layers) factor accounts for the [[Residual Connections]] — each layer adds to the residual stream, so without scaling, the magnitude would grow with depth.

GPT-2, LLaMA, and most modern LLMs use this pattern.

## Initialization and Modern Architecture

### Why Pre-Norm Helps

[[Normalization Techniques#Pre-Norm|Pre-norm]] (LayerNorm before attention/FFN) normalizes activations at every layer, making training less sensitive to initialization. This is one reason pre-norm became the default — it's more forgiving of imperfect initialization.

### Why Residual Connections Help

[[Residual Connections]] create an identity shortcut. Even with poor initialization, the gradient can flow through the skip connection (gradient of identity = 1). The network starts close to an identity function and gradually learns useful transformations.

### The μP Paradigm (Maximal Update Parameterization)

A 2022 technique from Microsoft that makes hyperparameters (including initialization) **transfer across model scales**:

- Find optimal initialization on a small model
- μP guarantees the same hyperparameters work on the large model
- Eliminates expensive hyperparameter search at scale
- Used by some frontier model training runs

## Initialization in Practice

Most practitioners never think about initialization because frameworks handle it:

```python
# PyTorch: Kaiming initialization is the default
model = nn.Linear(512, 512)  # Already initialized with Kaiming

# Custom initialization
nn.init.xavier_uniform_(model.weight)
nn.init.zeros_(model.bias)

# For Transformers, Hugging Face handles everything
model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-3-8B")
# Weights loaded from pre-trained checkpoint — no initialization needed
```

In the era of [[Transfer Learning]], most models are initialized from pre-trained checkpoints, not from scratch. Random initialization only matters when pre-training from scratch — a task performed by a handful of labs worldwide.

## Diagnosing Initialization Problems

| Symptom | Likely Cause | Fix |
|---|---|---|
| Loss is NaN from step 1 | Exploding activations | Reduce initialization scale |
| Loss doesn't decrease | Vanishing gradients or dead neurons | Use Kaiming init, check for dead ReLUs |
| All neurons output the same value | Symmetry not broken | Use random init (not zeros) |
| Training is very slow | Suboptimal scale | Try different init scheme, add [[Normalization Techniques\|normalization]] |

## Related

- [[Neural Networks]]
- [[Backpropagation]]
- [[Loss Functions and Optimization]]
- [[Activation Functions]]
- [[Residual Connections]]
- [[Normalization Techniques]]
- [[Training and Inference]]
- [[Deep Learning]]
