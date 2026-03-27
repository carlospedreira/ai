# Training and Inference

Every AI system has two main phases: **training** (learning from data) and **inference** (using what was learned). These are fundamentally different in cost, compute requirements, and how they work.

## Training

Training is the process of adjusting a model's parameters to minimize error on a dataset.

### Steps

1. **Initialize** — Set random initial weights
2. **Forward pass** — Feed input through the [[Neural Networks|network]] to get a prediction
3. **Compute loss** — Measure how wrong the prediction is
4. **Backward pass (backpropagation)** — Calculate how each weight contributed to the error
5. **Update weights** — Adjust weights using an optimizer (e.g., Adam, SGD)
6. **Repeat** — Iterate over the dataset for multiple epochs

### Key Concepts

- **Epoch** — One full pass through the training data
- **Batch size** — Number of examples processed before a weight update
- **Learning rate** — How much weights are adjusted per step (too high = instability, too low = slow)
- **Loss function** — The metric being minimized (e.g., cross-entropy for classification)
- **Optimizer** — Algorithm that determines how to update weights (Adam is most common)

### Training Compute

Training [[Large Language Models]] requires:
- Thousands of GPUs running for weeks or months
- Petabytes of text data
- Millions of dollars in compute cost

## Inference

Inference is using a trained model to make predictions on new inputs.

### Characteristics

- **Much cheaper** per operation than training
- **Latency matters** — Users expect fast responses
- **Can run on smaller hardware** — Including phones and edge devices for smaller models

### For LLMs

During inference, [[Large Language Models]] generate text **autoregressively** — one token at a time, each conditioned on all previous tokens. Techniques to improve inference:
- **KV caching** — Reuse computations from previous tokens
- **Quantization** — Reduce precision of weights (e.g., from 16-bit to 4-bit)
- **Speculative decoding** — Use a smaller model to draft, larger model to verify
- **Batching** — Process multiple requests simultaneously

## Training vs Inference

| Aspect | Training | Inference |
|---|---|---|
| Goal | Learn parameters | Use parameters |
| Compute | Massive (weeks on GPU clusters) | Modest per request |
| Data | Full training dataset | Single input |
| Frequency | Once (or periodically) | Every user request |
| Cost | Millions of dollars for LLMs | Fractions of a cent per query |

## Related

- [[Neural Networks]]
- [[Machine Learning]]
- [[Large Language Models]]
