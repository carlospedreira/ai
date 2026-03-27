# BERT

BERT (Bidirectional Encoder Representations from Transformers) is a 2018 model from Google that revolutionized [[Natural Language Processing]] by applying the [[Transformers|Transformer]] encoder with [[Causal vs Masked Language Modeling|Masked Language Modeling]]. It dominated NLP benchmarks for two years and established the paradigm of pre-training + fine-tuning that all modern [[Large Language Models]] follow.

## Why BERT Mattered

Before BERT, NLP models were either:
- **Unidirectional** (GPT-1) — only saw leftward context
- **Shallow bidirectional** (ELMo) — used separate left-to-right and right-to-left LSTMs

BERT was the first model to use **deep bidirectional** context — every token attends to every other token in the sequence simultaneously. This produced dramatically richer representations.

```
"I went to the bank to deposit money"

Unidirectional: "bank" sees only ["I", "went", "to", "the"]
BERT:           "bank" sees ["I", "went", "to", "the", "to", "deposit", "money"]
                → Clearly "bank" = financial institution (from "deposit", "money")
```

## Architecture

BERT is an [[Encoder-Decoder Architecture|encoder-only]] [[Transformers|Transformer]]:

```
Input tokens → Token Embeddings + Position Embeddings + Segment Embeddings
     ↓
[Transformer Encoder Layer] × 12 (BERT-base) or 24 (BERT-large)
     ↓
Contextual representations for each token
```

| Variant | Layers | Hidden Size | Heads | Parameters |
|---|---|---|---|---|
| BERT-base | 12 | 768 | 12 | 110M |
| BERT-large | 24 | 1024 | 16 | 340M |

Tiny by modern standards (GPT-4 is ~1000× larger), but transformative in 2018.

## Pre-Training Objectives

### 1. Masked Language Modeling (MLM)

Randomly mask 15% of tokens; predict the masked ones:
```
Input:  "The [MASK] sat on the [MASK]"
Target: cat, mat
```

This forces the model to build deep bidirectional understanding — it must use context from both sides to predict masked words.

### 2. Next Sentence Prediction (NSP)

Given two sentences, predict whether sentence B follows sentence A:
```
Input:  [CLS] The cat sat on the mat. [SEP] It was a sunny day. [SEP]
Target: IsNext (50%) or NotNext (50%)
```

Later research (RoBERTa) showed NSP doesn't actually help much — MLM alone is sufficient.

## Fine-Tuning

BERT's breakthrough was the **pre-train → fine-tune** paradigm:

```
Pre-training (expensive, done once):
  Train on massive text corpus (Wikipedia + BookCorpus)
  → General language understanding

Fine-tuning (cheap, done per task):
  Add a task-specific head on top of BERT
  Train on labeled task data (1K–100K examples)
  → State-of-the-art on that specific task
```

### Fine-Tuning for Different Tasks

```
Classification: [CLS] token → Linear → Softmax → Label
                (sentiment analysis, spam detection)

Token Classification: Each token → Linear → Softmax → Per-token label
                      (named entity recognition, POS tagging)

Question Answering: Token representations → Start/End span prediction
                    (SQuAD-style extractive QA)

Sentence Similarity: [CLS] of sentence pair → Similarity score
                     (semantic textual similarity)
```

## Impact and Results

BERT set new state-of-the-art on 11 NLP benchmarks simultaneously at release:

| Benchmark | Previous SOTA | BERT | Task |
|---|---|---|---|
| GLUE | 70.0 | 80.5 | Multi-task NLU |
| SQuAD 1.1 | 84.6 F1 | 93.2 F1 | Reading comprehension |
| SQuAD 2.0 | 66.3 F1 | 83.1 F1 | Reading comprehension + unanswerable |
| MNLI | 80.6 | 86.7 | Natural language inference |

The margins were unprecedented — BERT didn't just beat the competition, it demolished it.

## BERT's Descendants

| Model | Year | Innovation |
|---|---|---|
| **RoBERTa** (Meta) | 2019 | More data, longer training, no NSP → better than BERT |
| **ALBERT** | 2019 | Parameter sharing → smaller model, similar performance |
| **DistilBERT** | 2019 | [[Knowledge Distillation]] → 60% size, 97% performance |
| **DeBERTa** (Microsoft) | 2020 | Disentangled attention → improved understanding |
| **ELECTRA** | 2020 | Replaced token detection (not masking) → more efficient pre-training |

## Where BERT Lives On (2026)

BERT itself is no longer state-of-the-art, but its architecture and training paradigm live on:

### [[Embeddings and Vector Search|Embedding Models]]
Modern sentence embedding models (used in [[Retrieval-Augmented Generation|RAG]] and [[Semantic Search]]) are BERT-descendants:
- Sentence-BERT / sentence-transformers
- E5, GTE embedding models
- Bidirectional encoders remain superior for embeddings

### Classification and NER
For specific classification tasks, fine-tuned BERT-class models remain fast, cheap, and effective — far more efficient than calling an LLM API for simple label prediction.

### Rerankers
Cross-encoder rerankers (used in [[Semantic Search#Re-Ranking|search pipelines]]) use BERT-style bidirectional encoding to score query-document relevance.

## BERT vs GPT: The Fork in the Road

BERT and GPT-1 were released within months of each other in 2018. They made opposite architectural choices:

| | BERT | GPT |
|---|---|---|
| Architecture | Encoder-only | Decoder-only |
| Attention | Bidirectional | Causal (left-to-right) |
| Pre-training | [[Causal vs Masked Language Modeling\|MLM]] | [[Causal vs Masked Language Modeling\|CLM]] |
| Strength | Understanding | Generation |
| Scaling trajectory | Plateaued at ~1B | Scaled to 1T+ |
| 2026 status | Niche (embeddings, classification) | Dominant (LLMs) |

GPT won the generation war (and with it, the chat/agent era). BERT won the embedding war. Both were right about pre-training + Transformers.

## Related

- [[Transformers]]
- [[Causal vs Masked Language Modeling]]
- [[Encoder-Decoder Architecture]]
- [[Natural Language Processing]]
- [[Embeddings and Vector Search]]
- [[Transfer Learning]]
- [[Word Embeddings]]
- [[Attention Is All You Need]]
- [[Semantic Search]]
