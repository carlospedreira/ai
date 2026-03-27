# Transformers

The Transformer is a [[Neural Networks|neural network]] architecture introduced in the 2017 paper *"Attention Is All You Need"* by Vaswani et al. It is the foundation of virtually all modern [[Large Language Models]] and has been adopted across [[Computer Vision]], audio, and other domains.

## Core Idea: Attention

The key innovation is the **self-attention mechanism**, which allows every element in a sequence to attend to every other element simultaneously, rather than processing tokens one at a time (as RNNs did).

### Self-Attention in Brief

For each token in a sequence, the model computes:

- **Query (Q)** — "What am I looking for?"
- **Key (K)** — "What do I contain?"
- **Value (V)** — "What information do I provide?"

Attention scores are computed as:

```
Attention(Q, K, V) = softmax(QK^T / √d_k) V
```

This lets the model weigh which other tokens are most relevant to each position.

### Multi-Head Attention

Instead of one attention computation, the model runs several in parallel ("heads"), each learning to attend to different types of relationships.

## Architecture

The original Transformer has two halves:

- **Encoder** — Reads the full input and builds a contextual representation
- **Decoder** — Generates output one token at a time, attending to both the encoder output and previously generated tokens

### Variants

| Variant | Structure | Examples | Use Case |
|---|---|---|---|
| Encoder-only | Encoder | BERT | Understanding, classification |
| Decoder-only | Decoder | GPT, Claude, LLaMA | Text generation ([[Large Language Models]]) |
| Encoder-decoder | Both | T5, BART | Translation, summarization |

## Why Transformers Won

- **Parallelizable** — Unlike RNNs, all positions are processed simultaneously during training
- **Long-range dependencies** — Attention can connect distant tokens directly
- **Scalable** — Performance improves predictably with more data and compute (see scaling laws)

## Key Components

- **Positional encoding** — Injects sequence order since attention has no inherent notion of position
- **Layer normalization** — Stabilizes training
- **Residual connections** — Help gradients flow through deep networks
- **Feed-forward layers** — Applied after attention at each layer

## Related

- [[Neural Networks]]
- [[Large Language Models]]
- [[Deep Learning]]
- [[Generative AI]]
