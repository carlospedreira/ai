# Attention Mechanism

The attention mechanism is the core innovation behind [[Transformers]] and modern [[Large Language Models]]. It allows every element in a sequence to dynamically focus on the most relevant other elements, enabling models to capture long-range dependencies and complex relationships.

## The Core Idea

In natural language, the meaning of a word depends on context:

> "The **bank** of the **river** was muddy."
> "I went to the **bank** to deposit money."

Attention lets the model learn that "bank" should attend to "river" in the first sentence and "deposit" / "money" in the second — dynamically, based on content.

## How Self-Attention Works

For each token in a sequence, three vectors are computed:

- **Query (Q)** — "What am I looking for?"
- **Key (K)** — "What do I contain?"
- **Value (V)** — "What information do I provide?"

### The Computation

```
Attention(Q, K, V) = softmax(QK^T / √d_k) · V
```

Step by step:
1. **QK^T** — Dot product of each query with all keys → raw attention scores
2. **/ √d_k** — Scale by square root of key dimension (prevents gradients from vanishing)
3. **softmax** — Convert scores to probabilities (they sum to 1)
4. **· V** — Weight the values by these probabilities

The result: each token gets a new representation that's a weighted combination of all other tokens' values, where the weights reflect relevance.

### Example

For the sentence "The cat sat on the mat":

The word "sat" might attend strongly to:
- "cat" (who is doing the sitting?) — high attention weight
- "mat" (where is the sitting happening?) — moderate weight
- "the" (less relevant) — low weight

## Multi-Head Attention

Instead of computing attention once, the model runs **multiple attention heads in parallel**, each learning to focus on different types of relationships:

```
Head 1: syntactic relationships (subject-verb)
Head 2: semantic similarity (synonyms)
Head 3: positional patterns (adjacent words)
Head 4: long-range dependencies (pronouns to antecedents)
...
```

Outputs from all heads are concatenated and projected back to the model dimension.

Typical head counts:
- Small models: 8–12 heads
- Large models: 32–128+ heads

## Types of Attention

### Self-Attention
Every token attends to every other token in the same sequence. Used in both encoders and decoders.

### Cross-Attention
Tokens in one sequence attend to tokens in another sequence. Used in encoder-decoder models (translation: target language attends to source language).

### Causal (Masked) Attention
Used in decoder-only models ([[Large Language Models|LLMs]]). Each token can only attend to previous tokens — future tokens are masked. This enforces the autoregressive property: the model can only use information it has already generated.

```
Token 1: can see [1]
Token 2: can see [1, 2]
Token 3: can see [1, 2, 3]
Token 4: can see [1, 2, 3, 4]
```

## Computational Cost

Self-attention is **O(n²)** in sequence length — every token attends to every other token. For a 100K token context window, that's 10 billion attention computations per layer.

This is why:
- [[Context Management]] is critical — filling context with irrelevant tokens wastes quadratic compute
- Long-context models require architectural innovations
- [[Inference Optimization|KV caching]] is essential

## Efficiency Improvements

| Technique | Approach | Used By |
|---|---|---|
| **Flash Attention** | Fused GPU kernels, tiling for memory efficiency | Most modern models |
| **Grouped Query Attention (GQA)** | Share K/V heads across multiple Q heads | LLaMA 2/3, Mistral |
| **Multi-Query Attention (MQA)** | Single K/V head for all Q heads | PaLM, Falcon |
| **Sliding Window** | Attend only to a local window + global tokens | Mistral, Longformer |
| **Ring Attention** | Distribute attention across devices for very long sequences | Research |
| **Linear Attention** | Approximate attention with O(n) complexity | Research |

### Flash Attention

The most impactful optimization. Standard attention materializes the full n×n attention matrix in GPU memory. Flash Attention:
- Tiles the computation into blocks that fit in GPU SRAM
- Never materializes the full matrix
- 2–4x faster, uses 5–20x less memory
- Enables practical training with long contexts

## Attention Beyond Language

The attention mechanism has been adopted across all of AI:

| Domain | Application |
|---|---|
| [[Computer Vision]] | Vision Transformers (ViT) — attend over image patches |
| Audio | Whisper — attend over spectrogram frames |
| Protein folding | AlphaFold — attend over amino acid residues |
| Robotics | Attend over sensor observations |
| Multi-modal | Cross-attend between text and image tokens |

## Related

- [[Transformers]]
- [[Neural Networks]]
- [[Large Language Models]]
- [[Context Management]]
- [[Inference Optimization]]
- [[Tokens and Tokenization]]
