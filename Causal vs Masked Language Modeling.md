# Causal vs Masked Language Modeling

Causal Language Modeling (CLM) and Masked Language Modeling (MLM) are the two primary pre-training objectives for [[Large Language Models|language models]]. They determine how the model learns from text during [[Training and Inference|pre-training]] and fundamentally shape what the model can do.

## Causal Language Modeling (CLM)

Predict the **next token** given all previous tokens:

```
Input:  "The cat sat on the"
Target: "mat"

The model sees: [The] → predict [cat]
                [The, cat] → predict [sat]
                [The, cat, sat] → predict [on]
                [The, cat, sat, on] → predict [the]
                [The, cat, sat, on, the] → predict [mat]
```

Each position can only attend to previous positions (**causal mask** — the future is hidden):

```
Token:    The  cat  sat  on   the  mat
The:      [✓    ✗    ✗    ✗    ✗    ✗]
cat:      [✓    ✓    ✗    ✗    ✗    ✗]
sat:      [✓    ✓    ✓    ✗    ✗    ✗]
on:       [✓    ✓    ✓    ✓    ✗    ✗]
the:      [✓    ✓    ✓    ✓    ✓    ✗]
mat:      [✓    ✓    ✓    ✓    ✓    ✓]
```

### Properties
- **Autoregressive** — Generates text left-to-right, one token at a time
- **Every token is a training signal** — Each position provides a prediction target
- **Natural for generation** — The training objective IS the generation task
- **Unidirectional** — Each token only sees leftward context

### Used By
All decoder-only [[Large Language Models]]: GPT family, Claude, LLaMA, Mistral, Gemini, Qwen

### Why CLM Won

CLM became the dominant paradigm because:
1. **Simplicity** — One objective, one architecture, scales cleanly
2. **Efficiency** — Every token in the training data provides a learning signal (no masking waste)
3. **Generality** — A model trained on next-token prediction can do anything via [[Prompt Engineering]]
4. **[[Scaling Laws]]** — CLM loss scales predictably with compute, data, and parameters
5. **Generation** — The model naturally generates text, which is the core capability needed for assistants and [[AI Coding Harnesses|coding agents]]

## Masked Language Modeling (MLM)

Randomly mask some tokens and predict them from the surrounding context:

```
Input:  "The [MASK] sat on the [MASK]"
Target: [MASK₁] = "cat", [MASK₂] = "mat"
```

The model sees the *entire* sequence (both before and after the masked position):

```
Token:    The  [M]  sat  on   the  [M]
The:      [✓    ✓    ✓    ✓    ✓    ✓]   ← bidirectional!
[MASK]:   [✓    ✓    ✓    ✓    ✓    ✓]
sat:      [✓    ✓    ✓    ✓    ✓    ✓]
on:       [✓    ✓    ✓    ✓    ✓    ✓]
the:      [✓    ✓    ✓    ✓    ✓    ✓]
[MASK]:   [✓    ✓    ✓    ✓    ✓    ✓]
```

### Properties
- **Bidirectional** — Each token attends to the full context (past and future)
- **Not autoregressive** — Can't naturally generate text left-to-right
- **~15% masking rate** — Only masked tokens provide a training signal (less efficient)
- **Rich representations** — Bidirectional context produces excellent [[Embeddings and Vector Search|embeddings]]

### Used By
Encoder-only models: **BERT**, RoBERTa, DeBERTa, ELECTRA

### Why MLM Lost (for Generation)

MLM produces excellent contextual representations but can't generate text naturally:
- No autoregressive generation capability
- The [MASK] token doesn't exist at inference time
- Generating text requires iterative masking and prediction (slow, awkward)
- CLM models can do everything MLM models do via prompting, but not vice versa

## Comparison

| Aspect | CLM (GPT, Claude) | MLM (BERT) |
|---|---|---|
| Direction | Left-to-right only | Bidirectional |
| Training signal | Every token (~100%) | Masked tokens (~15%) |
| Generation | Natural | Unnatural |
| Understanding | Good (but only leftward) | Excellent (full context) |
| Embeddings | Good | Excellent |
| Scalability | Proven to 1T+ parameters | Plateaued at ~1B |
| Current status | Dominant | Niche (embeddings, classification) |

## Prefix Language Modeling

A hybrid: the prefix is processed bidirectionally (like MLM), then the suffix is generated causally (like CLM):

```
Prefix (bidirectional): "Translate to French: The cat sat on the mat."
Suffix (causal):        "Le chat s'est assis sur le tapis."
```

Used by some [[Encoder-Decoder Architecture|encoder-decoder]] models and the UL2 (Unified Language Learner) approach.

## Span Corruption (T5)

Google's T5 uses a variant of masking — replace random **spans** (not individual tokens) with sentinel tokens:

```
Input:  "The <X> sat <Y> mat"
Target: "<X> cat <Y> on the"
```

More efficient than single-token masking because spans provide more context per prediction.

## Practical Implications

### For [[AI Coding Harnesses]]
All coding agents use **CLM models** (GPT, Claude, LLaMA) because they need to generate code. MLM models can't generate — they can only fill in blanks.

### For [[Embeddings and Vector Search]]
**MLM models** (or their derivatives) often produce better embeddings because bidirectional context captures meaning more fully. This is why embedding models (text-embedding-3, voyage-code-3) use encoder architectures.

### For [[Semantic Search]]
Search systems often use an MLM-based bi-encoder for retrieval and a CLM model for generation — combining the strengths of both.

## Related

- [[Large Language Models]]
- [[Transformers]]
- [[Encoder-Decoder Architecture]]
- [[Training and Inference]]
- [[Attention Mechanism]]
- [[Embeddings and Vector Search]]
- [[Scaling Laws]]
- [[Tokens and Tokenization]]
