# Semantic Search

Semantic search finds results based on **meaning** rather than exact keyword matches. It powers [[Retrieval-Augmented Generation|RAG]] pipelines, codebase indexing in [[AI Coding Harnesses]], and modern search engines.

## Keyword Search vs Semantic Search

| Query: "How do I authenticate users?" | Keyword Search | Semantic Search |
|---|---|---|
| Matches `authenticate` literally | Yes | Yes |
| Matches `login`, `sign in`, `verify credentials` | No | Yes |
| Matches `auth middleware` | Partially (`auth`) | Yes (understands relationship) |
| Matches `OAuth2 flow` | No | Yes (semantically related) |

Keyword search (BM25, TF-IDF) counts word overlap. Semantic search compares **meaning** using [[Embeddings and Vector Search|embeddings]].

## How It Works

```
Query: "How do I authenticate users?"
                ↓ embed
        Query vector: [0.23, -0.45, 0.67, ...]
                ↓ similarity search
        Find nearest vectors in the index
                ↓
        Results ranked by semantic similarity
```

### The Pipeline

1. **Index time:** Embed all documents/code chunks into vectors, store in a [[Embeddings and Vector Search|vector database]]
2. **Query time:** Embed the query into the same vector space
3. **Search:** Find the k nearest vectors (approximate nearest neighbor)
4. **Rank:** Score by cosine similarity or other metric
5. **Return:** Top results, optionally re-ranked

## Hybrid Search

The best results come from combining both approaches:

```
Query → Keyword search (BM25)  → Results A
     → Semantic search (vectors) → Results B
                                    ↓
                            Reciprocal Rank Fusion
                                    ↓
                             Merged results
```

**Reciprocal Rank Fusion (RRF):** Combine rankings by giving each result a score of 1/(rank + k), then sum across result sets. Simple but effective.

**Why hybrid?**
- Keyword search catches exact matches (variable names, error codes)
- Semantic search catches conceptual matches (related functionality)
- Together they have better recall than either alone

## Semantic Search in Code

### Codebase Indexing

[[AI Coding Landscape|Cursor]] and [[AI Coding Landscape|Augment Code]] use semantic search to find relevant code:

```
Query: "function that validates email addresses"
         ↓ semantic search
Finds: validateEmail(), isValidEmailFormat(), checkEmailSyntax()
       (even across different files, languages, naming conventions)
```

### Code-Specialized Embeddings

General-purpose embeddings (OpenAI text-embedding) work for code, but code-specialized models perform better:

| Model | Optimized For |
|---|---|
| Voyage Code 3 | Code search and retrieval |
| CodeBERT | Code understanding |
| StarEncoder | Multi-language code |
| Cursor's custom model | IDE-integrated code search |

### Chunking Code for Search

How you split code into searchable chunks matters:

| Strategy | Unit | Best For |
|---|---|---|
| Function-level | Each function | Most code searches |
| Class-level | Each class | OOP navigation |
| File-level | Entire file | Small files, config |
| Docstring + signature | Function header only | API discovery |
| AST-based | Syntax tree nodes | Maximum precision |

## Semantic Search in [[AI Coding Harnesses]]

| Harness | Search Approach |
|---|---|
| [[Claude Code]] | On-demand: Grep (keyword) + Glob (pattern). No pre-built index. |
| [[OpenCode]] | On-demand tools + optional embedding index |
| [[AI Coding Landscape\|Cursor]] | Pre-built embedding index over entire codebase |
| [[AI Coding Landscape\|Augment Code]] | Context Engine indexes 400K+ files in real-time |
| [[AI Coding Landscape\|Aider]] | Repository map via tree-sitter (structural, not embedding-based) |

The trade-off: pre-built indexes are faster at query time but require upfront computation and must be kept in sync with code changes.

## Re-Ranking

Initial retrieval is fast but imprecise. Re-ranking adds a precision layer:

```
1. Retrieve top 100 results (fast, approximate)
2. Re-rank with a cross-encoder model (slow, precise) → top 10
3. Return top 10 to the user/LLM
```

Cross-encoders process the query and document together (unlike bi-encoders which process them separately), giving much better relevance scores at higher computational cost.

## Evaluation Metrics

| Metric | What It Measures |
|---|---|
| **Recall@k** | Fraction of relevant results in top k |
| **MRR** (Mean Reciprocal Rank) | How high the first relevant result ranks |
| **NDCG** (Normalized Discounted Cumulative Gain) | Ranking quality weighted by position |
| **Precision@k** | Fraction of top k results that are relevant |

## Related

- [[Embeddings and Vector Search]]
- [[Retrieval-Augmented Generation]]
- [[Context Management]]
- [[AI Coding Harnesses]]
- [[Word Embeddings]]
- [[Natural Language Processing]]
