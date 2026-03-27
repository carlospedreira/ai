# Word Embeddings

Word embeddings are dense vector representations where each word is mapped to a point in a continuous vector space. They were the breakthrough that launched modern [[Natural Language Processing]] and remain the input layer of all [[Large Language Models]].

## Before Embeddings: One-Hot Encoding

The naive approach represents each word as a sparse vector:

```
Vocabulary: [cat, dog, fish, bird]

cat  = [1, 0, 0, 0]
dog  = [0, 1, 0, 0]
fish = [0, 0, 1, 0]
bird = [0, 0, 0, 1]
```

Problems:
- **No similarity** — "cat" and "dog" are equally distant from each other as "cat" and "fish"
- **Huge dimensionality** — Vocabulary of 50,000 words = 50,000-dimensional vectors
- **No generalization** — Learning about "cat" tells the model nothing about "kitten"

## The Embedding Idea

Map words to dense vectors (e.g., 300 dimensions) where **similar words are close together**:

```
cat   = [0.52, -0.31, 0.87, ..., 0.14]
kitten = [0.49, -0.28, 0.85, ..., 0.12]   ← close to cat!
dog   = [0.41, -0.22, 0.79, ..., 0.18]   ← close-ish (both are pets)
pizza = [-0.63, 0.74, -0.12, ..., -0.55]  ← far away
```

## Landmark Models

### Word2Vec (2013, Google)

Mikolov et al. Two training approaches:

**Skip-Gram:** Given a word, predict surrounding context words
```
Input: "sat" → Predict: "the", "cat", "on", "the", "mat"
```

**CBOW (Continuous Bag of Words):** Given context, predict the center word
```
Input: "the", "cat", ___, "on", "the" → Predict: "sat"
```

Famous property: **vector arithmetic captures semantics**
```
king - man + woman ≈ queen
Paris - France + Italy ≈ Rome
```

### GloVe (2014, Stanford)

Global Vectors for Word Representation. Instead of a neural network, GloVe directly factorizes the word co-occurrence matrix:
- Counts how often pairs of words appear near each other in a corpus
- Decomposes this matrix into word vectors
- Combines global statistics with local context

### FastText (2016, Meta/Facebook)

Extends Word2Vec with **subword information**:
- "unhappiness" → "un", "hap", "pi", "ness" (character n-grams)
- Can generate vectors for words not in the vocabulary (OOV)
- Better for morphologically rich languages

## From Static to Contextual

### Static Embeddings (Word2Vec, GloVe)
Each word gets **one** vector regardless of context:
- "bank" (financial) = same vector as "bank" (river)
- This is a significant limitation

### Contextual Embeddings (ELMo, BERT, GPT)
Each word gets a **different** vector depending on context:
- "I went to the bank to deposit money" → "bank" = financial vector
- "The river bank was muddy" → "bank" = geographic vector

**ELMo (2018):** Bidirectional LSTM produces context-dependent embeddings. First major contextual model.

**BERT (2018):** [[Transformers|Transformer]] encoder produces deep contextual embeddings. Revolutionized NLP benchmarks.

**GPT/Claude/LLaMA:** Decoder-only Transformers where every token's representation is fully contextual, conditioned on all previous tokens.

## How Modern LLMs Use Embeddings

In a [[Transformers|Transformer]]-based LLM:

1. **Token embedding layer** — Maps each token ID to an initial dense vector (learned during training)
2. **[[Positional Encoding]]** — Adds position information
3. **Transformer layers** — Progressively transform these embeddings through [[Attention Mechanism|attention]] and feed-forward layers
4. **Output layer** — Projects the final embedding back to vocabulary probabilities

The embedding dimension is a key hyperparameter:

| Model | Embedding Dimension |
|---|---|
| GPT-2 | 768 |
| GPT-3 | 12,288 |
| LLaMA 3 70B | 8,192 |
| Claude (estimated) | 8,192–16,384 |

## Embedding Applications Beyond Language

See [[Embeddings and Vector Search]] for the broader use of embeddings:
- **Sentence/document embeddings** — Represent entire texts as vectors for [[Retrieval-Augmented Generation|RAG]]
- **Code embeddings** — Represent code for semantic search (Cursor, Augment Code)
- **Image embeddings** — Represent images in the same space as text (CLIP)
- **Multimodal embeddings** — Shared space for text, images, audio

## Related

- [[Embeddings and Vector Search]]
- [[Natural Language Processing]]
- [[Tokens and Tokenization]]
- [[Transformers]]
- [[Large Language Models]]
- [[Positional Encoding]]
