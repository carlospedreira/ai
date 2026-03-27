# Natural Language Processing

Natural Language Processing (NLP) is the branch of [[Artificial Intelligence]] focused on enabling computers to understand, interpret, and generate human language.

## Evolution

### Classical NLP (pre-2013)
- Rule-based systems and hand-crafted grammars
- Bag-of-words models, TF-IDF
- Statistical methods (n-grams, Hidden Markov Models)

### Neural NLP (2013–2017)
- Word embeddings (Word2Vec, GloVe) — words as dense vectors
- RNNs and LSTMs for sequential processing

### Transformer Era (2017–present)
- [[Transformers]] revolutionized the field
- Pre-trained models ([[Large Language Models]]) set new benchmarks on virtually every NLP task
- Single models now handle many tasks instead of specialized systems per task

## Core Tasks

| Task | Description | Example |
|---|---|---|
| Text classification | Assign labels to text | Spam detection, sentiment analysis |
| Named entity recognition | Identify entities (people, places) | "Paris" → Location |
| Machine translation | Convert between languages | English → Spanish |
| Summarization | Condense text | Article → 3-sentence summary |
| Question answering | Answer questions from context | Reading comprehension |
| Text generation | Produce coherent text | Chatbots, creative writing |
| Semantic similarity | Measure meaning overlap | Duplicate question detection |

## Key Concepts

- **Tokenization** — Splitting text into tokens (words, subwords, or characters). Modern LLMs use subword tokenization (BPE, SentencePiece).
- **Embeddings** — Dense vector representations of tokens or sentences that capture semantic meaning
- **Attention** — The mechanism that lets models focus on relevant parts of the input (see [[Transformers]])
- **Context window** — The maximum amount of text a model can process at once

## Modern NLP = LLMs

Most NLP tasks are now addressed by [[Large Language Models]] through [[Prompt Engineering]] rather than task-specific architectures. A single LLM can perform classification, translation, summarization, and more — based on how you prompt it.

## Related

- [[Transformers]]
- [[Large Language Models]]
- [[Prompt Engineering]]
