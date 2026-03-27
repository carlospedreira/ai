# Encoder-Decoder Architecture

The encoder-decoder architecture is a [[Neural Networks|neural network]] design where one component (encoder) processes the input into a latent representation, and another component (decoder) generates output from that representation. It is the original [[Transformers|Transformer]] design and remains important for translation, summarization, and multimodal tasks.

## The Concept

```
Input sequence → [Encoder] → Latent representation → [Decoder] → Output sequence
```

The encoder compresses the input into a fixed or variable-length representation that captures its meaning. The decoder then generates the output, conditioned on that representation.

## Variants in the Transformer Family

### Encoder-Decoder (Original Transformer)

```
Source: "The cat sat on the mat"
            ↓
       [Encoder] — processes full input bidirectionally
            ↓
   Context vectors (one per input token)
            ↓
       [Decoder] — generates output autoregressively
            ↓
Target: "Le chat s'est assis sur le tapis"
```

The decoder uses **cross-attention** to look at all encoder outputs while generating each token.

**Models:** T5, BART, mBART, Flan-T5, MarianMT, NLLB

**Best for:** Translation, summarization, question answering with extractive context

### Encoder-Only

```
Input: "The [MASK] sat on the mat"
            ↓
       [Encoder] — bidirectional attention over all tokens
            ↓
   Contextual representations for each token
            ↓
Output: Predictions for each position (e.g., [MASK] → "cat")
```

Every token attends to every other token (no causal mask). Produces rich contextual embeddings but cannot generate text autoregressively.

**Models:** BERT, RoBERTa, DeBERTa, ELECTRA

**Best for:** Classification, named entity recognition, semantic similarity, [[Embeddings and Vector Search|embeddings]]

### Decoder-Only

```
Input: "The cat sat on the"
            ↓
       [Decoder] — causal attention (each token sees only previous tokens)
            ↓
Output: "mat" (next token prediction)
```

Each token can only attend to previous tokens (causal mask). Generates text one token at a time.

**Models:** GPT family, Claude, LLaMA, Mistral, Gemini

**Best for:** Text generation, chat, coding, reasoning — the dominant [[Large Language Models|LLM]] architecture

## Comparison

| Property | Encoder-Decoder | Encoder-Only | Decoder-Only |
|---|---|---|---|
| Input processing | Bidirectional | Bidirectional | Causal (left-to-right) |
| Generation | Yes (via decoder) | No (classification only) | Yes (autoregressive) |
| Cross-attention | Yes | No | No |
| Context understanding | Excellent | Excellent | Good (but only leftward) |
| Text generation | Good | Cannot | Excellent |
| Typical size (2026) | 1B–11B | 100M–1B | 7B–1T+ |
| Current dominance | Niche | Declining (replaced by decoder-only) | Dominant |

## Why Decoder-Only Won

The field has converged on decoder-only for [[Large Language Models]]. Why?

1. **Simplicity** — One architecture, one training objective (next-token prediction)
2. **Scaling** — Decoder-only scales more efficiently with compute (no encoder overhead for generation tasks)
3. **Versatility** — Can do everything encoder-decoder can via [[Prompt Engineering]] (translation, summarization, QA, classification)
4. **In-context learning** — Decoder-only models excel at few-shot learning from examples in the prompt
5. **Training efficiency** — Every token in the training data provides a learning signal

## Where Encoder-Decoder Still Excels

| Use Case | Why Encoder-Decoder |
|---|---|
| **Machine translation** | Bidirectional encoding of source language helps accuracy |
| **Summarization** | Full bidirectional understanding of the input document |
| **Speech recognition** | Encode audio spectrogram → decode text (Whisper uses this pattern) |
| **Image captioning** | Encode image → decode caption |
| **Structured extraction** | Encode document → decode structured output |

## The Cross-Attention Mechanism

What makes encoder-decoder special is **cross-attention** in the decoder:

```
Decoder token "Le" attends to:
  Encoder outputs: ["The", "cat", "sat", "on", "the", "mat"]
  Highest attention on "The" (alignment for translation)
```

Each decoder token can look at the *entire* encoded input, regardless of generation position. This is more efficient than stuffing everything into the prompt prefix (decoder-only approach).

## In Modern Systems

Even in the decoder-only era, encoder-decoder thinking appears in:

- **[[Retrieval-Augmented Generation|RAG]]** — The retriever is like an encoder; the LLM is the decoder
- **[[Multimodal AI]]** — Vision encoder + language decoder (LLaVA, Claude's vision)
- **[[Diffusion Models]]** — Text encoder (CLIP) + image decoder (U-Net/DiT)
- **Speech** — Audio encoder (Whisper) + text decoder

The pattern of "encode context → decode output" remains fundamental, even when the components aren't a single model.

## T5: The Pinnacle of Encoder-Decoder

Google's T5 (Text-to-Text Transfer Transformer) framed every NLP task as text-to-text:

```
Translation:    "translate English to French: The cat sat" → "Le chat s'est assis"
Summarization:  "summarize: [long article]" → "Article summary..."
Classification: "sentiment: This movie was great" → "positive"
QA:             "question: What color? context: The sky is blue." → "blue"
```

This unified framing showed that a single encoder-decoder could handle any text task — a precursor to the "one model for everything" approach now dominated by decoder-only LLMs.

## Related

- [[Transformers]]
- [[Attention Mechanism]]
- [[Large Language Models]]
- [[Multimodal AI]]
- [[Natural Language Processing]]
- [[Recurrent Neural Networks]]
