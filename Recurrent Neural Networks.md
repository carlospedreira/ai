# Recurrent Neural Networks

Recurrent Neural Networks (RNNs) are [[Neural Networks]] designed for sequential data — they process inputs one step at a time while maintaining a hidden state that carries information from previous steps. They dominated [[Natural Language Processing]] from 2013–2017 before being largely replaced by [[Transformers]].

## The Core Idea

Unlike feedforward networks that process each input independently, RNNs have a **loop** — the output of each step feeds back as input to the next:

```
    h₁ ──→ h₂ ──→ h₃ ──→ h₄
    ↑       ↑       ↑       ↑
    x₁      x₂      x₃      x₄
  ("The") ("cat") ("sat") ("down")
```

At each step t:
```
h_t = f(W_h · h_{t-1} + W_x · x_t + b)
```

The hidden state h_t is a compressed representation of everything the network has seen so far.

## The Vanishing Gradient Problem

Training RNNs requires backpropagation through time (BPTT) — unrolling the loop and computing gradients across all time steps. For long sequences, gradients either:

- **Vanish** — Multiply by values < 1 at each step → gradients shrink to near-zero → early tokens have no influence
- **Explode** — Multiply by values > 1 at each step → gradients grow exponentially → training diverges

This makes it difficult for vanilla RNNs to learn long-range dependencies (e.g., connecting a pronoun to a noun 50 words earlier).

## LSTM (Long Short-Term Memory)

Hochreiter & Schmidhuber (1997) solved the vanishing gradient problem with a gated architecture:

```
┌──────────────────────────────────────┐
│               LSTM Cell              │
│                                      │
│  forget gate: What to forget         │
│  input gate:  What new info to add   │
│  cell state:  Long-term memory       │
│  output gate: What to output         │
│                                      │
└──────────────────────────────────────┘
```

The **cell state** acts as a highway — information flows through with minimal modification, gated by learned sigmoid functions. This allows gradients to flow over hundreds of time steps.

## GRU (Gated Recurrent Unit)

A simplified LSTM (Cho et al., 2014):
- Merges forget and input gates into a single "update gate"
- Merges cell state and hidden state
- Fewer parameters, similar performance
- Faster to train

## RNNs vs Transformers

| Aspect | RNNs/LSTMs | [[Transformers]] |
|---|---|---|
| Processing | Sequential (one token at a time) | Parallel (all tokens simultaneously) |
| Training speed | Slow (can't parallelize across sequence) | Fast (fully parallelizable) |
| Long-range dependencies | Difficult despite LSTM gates | Easy via direct [[Attention Mechanism\|attention]] |
| Memory | Fixed-size hidden state | Grows linearly with sequence length |
| GPU utilization | Poor | Excellent |
| Current status (2026) | Largely replaced | Dominant |

### Why Transformers Won

The sequential nature of RNNs was the fatal flaw:
- Token 100 must wait for tokens 1–99 to be processed
- This can't be parallelized on GPUs
- [[Parallel Processing in AI]] is essential for modern scale

Transformers process all positions simultaneously, making them vastly more efficient on modern hardware. The [[Attention Mechanism]] also captures long-range dependencies more effectively than LSTM gates.

## Where RNNs Survive

Despite Transformers' dominance, RNNs aren't entirely gone:

| Application | Why RNNs |
|---|---|
| **State Space Models (Mamba)** | RNN-like architectures with efficient training |
| **Real-time processing** | Constant memory per step (no growing KV cache) |
| **Edge devices** | Small memory footprint |
| **Reinforcement learning** | Some RL architectures use RNNs for memory |
| **Music generation** | Sequential nature maps naturally |

### Mamba and State Space Models

A 2023 revival: Mamba (Gu & Dao) showed that structured state space models — which have RNN-like recurrence at inference but can be trained in parallel like Transformers — can match Transformer performance at certain scales with linear (not quadratic) complexity. This is an active research area as of 2026.

## Historical Significance

The RNN → LSTM → Attention → Transformer trajectory is one of the most important arcs in AI:
- RNNs proved neural networks could handle sequences (2013–2015)
- LSTMs made practical NLP possible (machine translation, speech recognition)
- Bahdanau attention (2014) showed that "looking back" at all input positions helps
- This inspired the full self-attention mechanism in "Attention Is All You Need" (2017)
- [[Transformers]] took the attention idea and removed the recurrence entirely

## Related

- [[Neural Networks]]
- [[Natural Language Processing]]
- [[Transformers]]
- [[Attention Mechanism]]
- [[Deep Learning]]
- [[Large Language Models]]
