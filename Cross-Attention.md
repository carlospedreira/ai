# Cross-Attention

Cross-attention is a variant of the [[Attention Mechanism]] where tokens from one sequence attend to tokens from a *different* sequence. While self-attention lets a sequence attend to itself, cross-attention connects two separate information streams — making it the bridge between modalities, languages, or representations.

## How It Differs from Self-Attention

### Self-Attention
Queries, keys, and values all come from the **same** sequence:
```
Sequence: "The cat sat"
Q, K, V all derived from → ["The", "cat", "sat"]
Each token attends to every other token in the same sequence
```

### Cross-Attention
Queries come from one sequence; keys and values come from **another**:
```
Decoder: "Le chat"      → Queries (Q)
Encoder: "The cat sat"  → Keys (K) and Values (V)

"Le" attends to ["The", "cat", "sat"] to decide what to translate next
"chat" attends to ["The", "cat", "sat"] to align with "cat"
```

## The Computation

```
CrossAttention(Q_decoder, K_encoder, V_encoder) = softmax(Q·K^T / √d_k) · V
```

Same formula as self-attention, but Q comes from the decoder and K, V come from the encoder. The attention weights tell us which encoder tokens are relevant to each decoder position.

## Where Cross-Attention Appears

### [[Encoder-Decoder Architecture|Encoder-Decoder Transformers]]

The original [[Attention Is All You Need|Transformer]] uses cross-attention in every decoder layer:

```
Decoder Layer:
  1. Masked self-attention (decoder attends to itself, causally)
  2. Cross-attention (decoder attends to encoder output)  ← this
  3. Feed-forward network
```

This lets the decoder "look at" the full encoded input while generating output. Used by T5, BART, Whisper, and machine translation models.

### [[Multimodal AI|Multimodal Models]]

Cross-attention connects different modalities:

```
Vision-Language Model:
  Image encoder → image tokens → K, V
  Text decoder  → text tokens  → Q

  Text tokens attend to image tokens via cross-attention
  "What color is the car?" → attends to image regions showing the car
```

**Examples:**
- **Flamingo** — Text cross-attends to image features
- **LLaVA** — Injects image tokens into the language model's context
- **Stable Diffusion** — Image denoiser cross-attends to CLIP text embeddings

### [[Diffusion Models]]

In text-to-image generation, the denoising U-Net/DiT uses cross-attention to condition on the text prompt:

```
Text: "A cat wearing a top hat" → CLIP encoder → text embeddings (K, V)
Image: noisy latent             → denoiser     → spatial features (Q)

Cross-attention: each spatial position in the image attends to the text
→ "top hat" gets high attention in the head region of the image
```

This is how text prompts steer image generation — the cross-attention weights literally determine which parts of the image correspond to which words.

### [[Retrieval-Augmented Generation|RAG]] Architectures

Some RAG systems use cross-attention between retrieved documents and the query:

```
Retrieved documents → encoder → K, V
User query          → decoder → Q

The model attends to retrieved documents while generating the answer
```

Fusion-in-Decoder (FiD) concatenates multiple retrieved passages and uses cross-attention to select relevant information from each.

## Cross-Attention vs Concatenation

An alternative to cross-attention is simply concatenating the two sequences and using self-attention:

```
Cross-attention:    Decoder(Q) × Encoder(K,V) → focused, efficient
Concatenation:      Self-Attention(["The cat sat", "Le chat"]) → everything attends to everything
```

| Approach | Compute | Quality | Used By |
|---|---|---|---|
| **Cross-attention** | O(n_dec × n_enc) | Focused | T5, Whisper, Stable Diffusion |
| **Concatenation** | O((n_dec + n_enc)²) | Rich but expensive | GPT-style (prefix = "encoder") |

Decoder-only [[Large Language Models]] effectively use concatenation — the "system prompt + context" is the de facto encoder, and the model's response is the decoder, all processed with self-attention. This is simpler but less parameter-efficient than explicit cross-attention.

## Visualizing Cross-Attention

Cross-attention weights produce interpretable alignment maps:

```
Translation: English → French

              The   cat   sat   on   the   mat
        Le   [0.8  0.05  0.05  0.02  0.05  0.03]
      chat   [0.05  0.85  0.02  0.01  0.02  0.05]
  s'est      [0.02  0.05  0.80  0.05  0.03  0.05]
    assis    [0.02  0.03  0.80  0.05  0.03  0.07]
       sur   [0.02  0.02  0.05  0.82  0.04  0.05]
        le   [0.05  0.02  0.02  0.03  0.83  0.05]
     tapis   [0.03  0.02  0.05  0.03  0.05  0.82]
```

Each row shows which source tokens the decoder focused on when generating that target token. The diagonal-ish pattern reflects word alignment between languages.

## Cross-Attention in Coding

In [[AI Coding Harnesses]], cross-attention-like patterns appear at the architectural level:

```
CLAUDE.md + tool definitions (stable context)  →  K, V
Current task + conversation (variable context)  →  Q

The agent "cross-attends" to project knowledge while processing the current task
```

[[Prompt Caching]] exploits this by caching the K, V computations for the stable prefix.

## Related

- [[Attention Mechanism]]
- [[Encoder-Decoder Architecture]]
- [[Transformers]]
- [[Multimodal AI]]
- [[Diffusion Models]]
- [[Retrieval-Augmented Generation]]
- [[Attention Is All You Need]]
