# Dropout and Regularization

Regularization techniques prevent [[Neural Networks]] from **overfitting** — memorizing training data rather than learning generalizable patterns. Dropout is the most famous regularization method in deep learning, but modern [[Transformers]] rely more on other approaches.

## Overfitting

```
Training Loss          Test Loss
    ↓ keeps decreasing     ↓ starts increasing ← overfitting begins
    ──────────────        ──╲──────────────
                              ╲
                               ╲ gap = overfitting
```

An overfit model performs well on training data but poorly on new data — it memorized the answers rather than learning the underlying patterns.

## Dropout (Srivastava et al., 2014)

During each training step, randomly set a fraction of neurons to zero:

```
Training (dropout rate = 0.5):
  [0.8, 0.3, 0.6, 0.1, 0.9] → [0.8, 0, 0.6, 0, 0.9]
                                        ↑         ↑ (randomly zeroed)

Inference:
  All neurons active, outputs scaled by (1 - dropout_rate)
  Or equivalently: scale up during training ("inverted dropout")
```

### Why It Works

- **Prevents co-adaptation** — Neurons can't rely on specific other neurons always being present
- **Ensemble effect** — Each training step uses a different random sub-network; the final network is like an ensemble of 2^n sub-networks
- **Noise injection** — The randomness acts as a regularizer, smoothing the loss landscape

### Dropout in Modern Networks

| Architecture | Dropout Usage |
|---|---|
| Classic CNNs (AlexNet, VGG) | Heavy (0.5 in fully-connected layers) |
| ResNets | Light or none (residual connections + BatchNorm suffice) |
| [[Transformers]] (BERT era) | Moderate (0.1 in attention and FFN) |
| Modern LLMs (GPT, LLaMA, Claude) | **Minimal or none** — replaced by other techniques |

Modern frontier [[Large Language Models]] largely don't use dropout because:
- Massive datasets make overfitting less likely (the model never sees the same example enough to memorize it)
- Other regularization techniques are more effective at scale
- Dropout introduces noise that can slow convergence

## Other Regularization Techniques

### Weight Decay (L2 Regularization)

Add a penalty proportional to weight magnitude:

```
Loss_total = Loss_data + λ × Σ(w²)
```

Encourages smaller weights, which tend to produce simpler, more generalizable functions. **AdamW** applies weight decay correctly (decoupled from the gradient) and is the standard optimizer for LLM training.

### Label Smoothing

Instead of training on hard labels (0 or 1), use soft labels:

```
Hard: target = [0, 0, 1, 0, 0]  (100% confidence in class 3)
Soft: target = [0.02, 0.02, 0.92, 0.02, 0.02]  (smoothed)
```

Prevents the model from becoming overconfident. Used in the original Transformer paper.

### Data Augmentation

Create training variety by transforming existing data:
- **Images:** Rotation, flipping, cropping, color jitter
- **Text:** Paraphrasing, back-translation, word replacement
- **Code:** Variable renaming, style transformation

See [[Data Preprocessing for AI]] and [[Synthetic Data]].

### Early Stopping

Monitor validation loss during training; stop when it starts increasing:

```
Epoch 10: val_loss = 2.1 ← improving
Epoch 11: val_loss = 1.9 ← improving
Epoch 12: val_loss = 1.85 ← improving
Epoch 13: val_loss = 1.87 ← degrading → stop here, use epoch 12 weights
```

Simple and effective, but requires a separate validation set.

### Stochastic Depth

Randomly skip entire layers during training (keep via residual connection):

```
Training: Layer 5 → [skip] → Layer 6 → Layer 7 → [skip] → Layer 8
          (skipped layers just pass input through via residual)

Inference: All layers active
```

Similar to dropout but at the layer level. Used in some vision and Transformer architectures.

### Mixup

Blend training examples and their labels:

```
x_new = 0.7 × image_cat + 0.3 × image_dog
y_new = 0.7 × "cat" + 0.3 × "dog"
```

Forces the model to learn smooth decision boundaries. Effective for classification.

## What Modern LLMs Actually Use

| Technique | Used? | Notes |
|---|---|---|
| **Weight decay** | Yes | Standard (via AdamW) |
| **Dropout** | Minimal | Small (0.0–0.1) or none |
| **Label smoothing** | Sometimes | Used in some training recipes |
| **Data augmentation** | Yes | Via [[Synthetic Data]], not traditional augmentation |
| **Early stopping** | Rarely | Training runs to a planned duration based on [[Scaling Laws]] |
| **[[Normalization Techniques]]** | Yes | RMSNorm/LayerNorm provides implicit regularization |
| **Large datasets** | Yes | The best regularizer is more data |

The implicit regularization from using massive datasets, proper [[Weight Initialization]], and [[Normalization Techniques]] has largely replaced explicit regularization methods at frontier scale.

## Related

- [[Neural Networks]]
- [[Training and Inference]]
- [[Loss Functions and Optimization]]
- [[Deep Learning]]
- [[Normalization Techniques]]
- [[Scaling Laws]]
- [[Data Preprocessing for AI]]
