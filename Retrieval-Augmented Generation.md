# Retrieval-Augmented Generation

Retrieval-Augmented Generation (RAG) is a technique that augments [[Large Language Models]] with external knowledge by retrieving relevant documents and injecting them into the prompt before generation. It's one of the most impactful patterns in applied AI.

## The Problem RAG Solves

LLMs have fixed training data with a knowledge cutoff. They also hallucinate — generating plausible but incorrect information. RAG addresses both by grounding the model in real, up-to-date data.

## How It Works

```
┌──────────┐     ┌──────────────┐     ┌──────────┐
│  Query   │────►│  Retriever   │────►│   LLM    │
│          │     │ (find relevant│     │(generate │
│          │     │  documents)  │     │ answer)  │
└──────────┘     └──────┬───────┘     └────┬─────┘
                        │                   │
                 ┌──────┴───────┐          │
                 │  Knowledge   │          │
                 │    Base      │     ┌────┴─────┐
                 │(docs, code,  │     │ Response │
                 │ database)    │     │(grounded)│
                 └──────────────┘     └──────────┘
```

### Steps

1. **Index** — Split documents into chunks, compute embeddings, store in a vector database
2. **Query** — Convert the user's question into an embedding
3. **Retrieve** — Find the most similar chunks using vector similarity search
4. **Augment** — Insert retrieved chunks into the LLM's prompt as context
5. **Generate** — LLM answers using both its training and the retrieved context

## Embeddings and Vector Search

### What Are Embeddings?

Embeddings are dense vector representations of text. Similar texts produce similar vectors. They power the retrieval step of RAG.

```
"How do I authenticate?" → [0.23, -0.45, 0.67, ..., 0.12]  (1536 dimensions)
"Login flow for users"   → [0.21, -0.43, 0.65, ..., 0.14]  (similar vector!)
"Database schema design"  → [0.89, 0.12, -0.34, ..., -0.56] (different vector)
```

### Vector Databases

| Database | Type | Notes |
|---|---|---|
| Pinecone | Cloud | Managed, serverless |
| Weaviate | Open source | Hybrid search |
| Qdrant | Open source | Rust-based, fast |
| ChromaDB | Open source | Simple, Python-native |
| pgvector | Postgres extension | Use existing Postgres |
| Milvus | Open source | Scalable, production-grade |

### Similarity Metrics

- **Cosine similarity** — Most common; measures angle between vectors
- **Dot product** — Fast, works when vectors are normalized
- **Euclidean distance** — Measures straight-line distance

## RAG in AI Coding Harnesses

Every [[AI Coding Harnesses|coding harness]] uses RAG-like patterns, even if they don't call it RAG:

| Pattern | RAG Equivalent |
|---|---|
| Agent reads files matching a grep query | Keyword retrieval |
| Cursor indexes codebase with embeddings | Vector RAG |
| Agent reads CLAUDE.md/AGENTS.md | Pre-retrieved context |
| MCP server returns relevant docs | Dynamic retrieval |

The codebase **is** the knowledge base. The tools (Glob, Grep, LSP) **are** the retriever.

## Advanced RAG Patterns

### Hybrid Search
Combine vector search (semantic) with keyword search (exact match) for better recall:
```
Vector search: "authentication flow" → conceptually similar docs
Keyword search: "authenticate" → exact matches
→ Merge and re-rank results
```

### Reranking
After initial retrieval, use a cross-encoder model to re-score relevance:
```
Initial retrieval (fast, 100 results) → Reranker (accurate, top 10)
```

### Chunking Strategies
How you split documents affects retrieval quality:

| Strategy | Description | Best For |
|---|---|---|
| Fixed-size | Split every N tokens | Simple, general purpose |
| Sentence | Split at sentence boundaries | Prose, documentation |
| Semantic | Split at topic changes | Long documents |
| Code-aware | Split at function/class boundaries | Source code |
| Parent-child | Retrieve child, include parent for context | Hierarchical docs |

### Agentic RAG
The LLM decides what to retrieve and iterates:
```
1. LLM: "I need info about the payment module"
2. Retriever returns payment docs
3. LLM: "These mention a 'Stripe adapter' — I need more on that"
4. Retriever returns Stripe integration docs
5. LLM: Now has enough context to answer
```

This is essentially how [[AI Agents and Sub-Agents|agents]] explore codebases — iterative, tool-driven retrieval.

## When to Use RAG

| Scenario | Use RAG? |
|---|---|
| Knowledge changes frequently | Yes |
| Domain-specific data (company docs, codebase) | Yes |
| Need citations / source attribution | Yes |
| General knowledge questions | Often not needed |
| Small, static dataset | Fine-tuning may be simpler |

## Related

- [[Large Language Models]]
- [[Prompt Engineering]]
- [[Context Management]]
- [[AI Agents and Sub-Agents]]
- [[Tool Use in AI]]
