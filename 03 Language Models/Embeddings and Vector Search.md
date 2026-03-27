# Embeddings and Vector Search

Embeddings are dense vector representations of data (text, images, code) that capture semantic meaning in a numerical format. Vector search finds similar items by comparing these vectors. Together, they power [[Retrieval-Augmented Generation]], [[Context Management|codebase indexing]], and recommendation systems.

## What Are Embeddings?

An embedding model converts input into a fixed-size vector of floating-point numbers:

```
"king"  → [0.5, 0.2, -0.3, ..., 0.1]   (e.g., 1536 dimensions)
"queen" → [0.5, 0.2, -0.1, ..., 0.1]   (very similar!)
"car"   → [-0.3, 0.8, 0.4, ..., -0.6]  (very different)
```

Key property: **semantic similarity** maps to **vector proximity**. Items that mean similar things end up close in vector space.

### Famous Example

```
king - man + woman ≈ queen
```

This vector arithmetic works because embeddings capture relationships, not just identity.

## Embedding Models

| Model | Dimensions | By | Use Case |
|---|---|---|---|
| `text-embedding-3-large` | 3072 | OpenAI | General text |
| `text-embedding-3-small` | 1536 | OpenAI | Cost-efficient |
| `voyage-code-3` | 1024 | Voyage AI | Code-specialized |
| Cohere Embed v3 | 1024 | Cohere | Multilingual |
| `nomic-embed-text` | 768 | Nomic | Open source |
| `bge-large-en-v1.5` | 1024 | BAAI | Open source |
| Cursor Embedding | Custom | Cursor | Codebase indexing |

Code-specialized embeddings (like Voyage Code) understand programming constructs and perform better for code search than general-purpose models.

## Vector Search

### How It Works

1. **Index phase**: Embed all documents/chunks, store vectors in a database
2. **Query phase**: Embed the query, find nearest vectors in the index

### Similarity Metrics

| Metric | Formula | Notes |
|---|---|---|
| Cosine similarity | `cos(θ) = A·B / (‖A‖·‖B‖)` | Most common; direction matters, magnitude doesn't |
| Dot product | `A·B` | Fast; assumes normalized vectors |
| Euclidean (L2) | `‖A - B‖` | Absolute distance |

### Approximate Nearest Neighbor (ANN)

Exact search is O(n) — too slow for millions of vectors. ANN algorithms trade accuracy for speed:

| Algorithm | Used By | Approach |
|---|---|---|
| HNSW | Most databases | Navigable small-world graph |
| IVF | Faiss, Milvus | Inverted file index with clustering |
| ScaNN | Google | Anisotropic quantization |
| Product Quantization | Various | Compress vectors, search in compressed space |

## Vector Databases

See [[Retrieval-Augmented Generation#Vector Databases]] for the full list.

Key capabilities:
- Store millions/billions of vectors
- Sub-millisecond similarity search
- Metadata filtering (filter by date, type, etc.)
- Hybrid search (combine vector + keyword)

## In AI Coding

### Codebase Indexing (Cursor)
Cursor uses a custom embedding model to index every file in your project. When you ask a question, it retrieves the most relevant code chunks — not just keyword matches, but semantically similar code.

### Code Search
```
Query: "function that handles user authentication"
→ Embedding finds: authenticateUser(), verifyJWT(), loginHandler()
  (even if none contain the word "authentication")
```

### Commit Message Search
Embed all commit messages → search by intent rather than keywords.

### Documentation Retrieval
Embed API docs → agent retrieves relevant pages when coding against an API.

## Chunking for Code

How you split code into chunks for embedding matters:

| Strategy | Chunk | Best For |
|---|---|---|
| Function-level | Each function is one chunk | Most code |
| Class-level | Each class is one chunk | OOP codebases |
| File-level | Entire file | Small files |
| Sliding window | Overlapping token windows | Large files |
| AST-based | Split at syntax tree boundaries | Maximum precision |

## Related

- [[Retrieval-Augmented Generation]]
- [[Context Management]]
- [[Tokens and Tokenization]]
- [[Neural Networks]]
- [[Large Language Models]]
