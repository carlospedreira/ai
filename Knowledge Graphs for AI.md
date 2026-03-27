# Knowledge Graphs for AI

Knowledge graphs are structured representations of real-world knowledge as entities and relationships. They complement [[Large Language Models]] by providing grounded, queryable, and explainable facts — addressing key LLM weaknesses like [[Hallucination and Grounding|hallucination]] and knowledge staleness.

## What Is a Knowledge Graph?

A knowledge graph stores facts as **triples**: (subject, predicate, object)

```
(Paris, is_capital_of, France)
(France, is_in, Europe)
(Eiffel Tower, is_located_in, Paris)
(Eiffel Tower, was_built_in, 1889)
```

These triples form a directed graph:
```
Paris ──is_capital_of──→ France ──is_in──→ Europe
  ↑
  └──is_located_in── Eiffel Tower ──was_built_in──→ 1889
```

## Knowledge Graphs vs Vector Databases

| Aspect | Knowledge Graph | [[Embeddings and Vector Search\|Vector Database]] |
|---|---|---|
| Storage | Structured triples (entities + relations) | Dense vectors |
| Query | Explicit graph traversal (SPARQL, Cypher) | Similarity search |
| Reasoning | Multi-hop (follow relationships) | Single-hop (nearest neighbors) |
| Explainability | High (path from query to answer) | Low (cosine similarity score) |
| Updates | Add/remove triples | Re-embed documents |
| Best for | Factual Q&A, relationship reasoning | Semantic similarity, RAG |

## Notable Knowledge Graphs

| Graph | Scale | By | Use |
|---|---|---|---|
| **Wikidata** | 100M+ items | Wikimedia | Open, general knowledge |
| **Google Knowledge Graph** | Billions of facts | Google | Search, Google Assistant |
| **DBpedia** | Millions of entities | Community | Structured Wikipedia extract |
| **UMLS** | Medical concepts | NLM | Medical/clinical AI |
| **ConceptNet** | Commonsense knowledge | Community | NLP, commonsense reasoning |

## Knowledge Graphs + LLMs

### Graph-Enhanced RAG

Instead of vector-only [[Retrieval-Augmented Generation|RAG]], use the knowledge graph to retrieve structured context:

```
Query: "Who is the CEO of the company that acquired Twitter?"

Vector RAG: Retrieves paragraphs about Twitter acquisition (might miss the connection)
Graph RAG: Traverses: Twitter → acquired_by → X Corp → CEO → Elon Musk (precise)
```

### GraphRAG (Microsoft)

Microsoft's GraphRAG builds a knowledge graph from documents, then uses it for retrieval:

1. **Indexing:** LLM extracts entities and relationships from documents → builds a graph
2. **Community detection:** Groups related entities into clusters
3. **Summarization:** LLM generates summaries for each community
4. **Query time:** Route query to relevant communities, retrieve summaries + source documents

Advantages over vanilla RAG:
- Better at synthesizing information across many documents
- Handles "what are the main themes?" type queries
- Provides more coherent, comprehensive answers

### LLM-Powered Knowledge Graph Construction

Use LLMs to build knowledge graphs automatically:

```
Input text: "Apple, founded by Steve Jobs in 1976, acquired Beats
            Electronics in 2014 for $3 billion."

Extracted triples:
(Apple, founded_by, Steve Jobs)
(Apple, founded_in, 1976)
(Apple, acquired, Beats Electronics)
(Beats acquisition, year, 2014)
(Beats acquisition, price, $3 billion)
```

### Knowledge Graph-Grounded Generation

Force the LLM to cite knowledge graph facts in its response:

```
System: "Answer based on the following facts from our knowledge graph:
         - Product X: launched 2024-03, price $99, category: electronics
         - Product X: rating 4.5/5, reviews: 1,247"

User: "Tell me about Product X"

LLM: "Product X is an electronics product launched in March 2024,
      priced at $99. It has a 4.5/5 star rating based on 1,247 reviews."
      [Every claim traceable to a specific triple]
```

## Knowledge Graphs in [[AI Coding Harnesses]]

### Code Knowledge Graphs

Represent codebase structure as a graph:
```
(UserService, has_method, authenticate)
(authenticate, calls, validateToken)
(validateToken, uses, JWTLibrary)
(authenticate, throws, AuthenticationError)
(UserController, depends_on, UserService)
```

This enables queries like:
- "What depends on UserService?" → graph traversal
- "What happens if validateToken fails?" → follow error paths
- "What's the call chain from the API endpoint to the database?" → path query

### Sourcegraph / Amp

[[AI Coding Landscape|Amp]] (by Sourcegraph) uses a **code graph** — a knowledge graph built from code intelligence (definitions, references, dependencies). This is why it can answer complex cross-repository questions that file-based search misses.

### MCP Memory Server

The [[Model Context Protocol|MCP]] `server-memory` provides a persistent knowledge graph:
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-memory"]
    }
  }
}
```

The agent can store and retrieve structured knowledge across sessions.

## Graph Databases

| Database | Query Language | Notes |
|---|---|---|
| **Neo4j** | Cypher | Most popular graph database |
| **Amazon Neptune** | SPARQL, Gremlin | AWS managed |
| **ArangoDB** | AQL | Multi-model (graph + document) |
| **TigerGraph** | GSQL | High-performance analytics |

## Related

- [[Retrieval-Augmented Generation]]
- [[Embeddings and Vector Search]]
- [[Hallucination and Grounding]]
- [[Semantic Search]]
- [[Large Language Models]]
- [[Model Context Protocol]]
