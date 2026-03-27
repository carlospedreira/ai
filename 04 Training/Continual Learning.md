# Continual Learning

Continual learning (also called lifelong learning or incremental learning) is the ability of an AI system to learn new tasks or incorporate new data over time without forgetting what it previously learned. It is one of the most significant unsolved problems in AI — and its solution would transform how [[Large Language Models]] and [[AI Agents and Sub-Agents|AI agents]] work.

## The Problem

Current LLMs have a **frozen** training paradigm:
```
Pre-train (months) → Fine-tune (days) → Deploy (frozen weights forever)
```

Once deployed, the model cannot:
- Learn from user interactions
- Update its knowledge when the world changes
- Improve on tasks it struggled with
- Incorporate new information without full retraining

This is fundamentally unlike human learning, where we continuously accumulate knowledge and skills.

## Why It's Hard

### [[Catastrophic Forgetting]]
The central obstacle. When a neural network learns new information, gradient updates overwrite knowledge stored in shared weights:

```
Model knows:    English + French + Code + Math
Learn:          Medical terminology
Result:         Medical ✓, English ✓, French ↓, Code ↓, Math ↓
                (new knowledge partially overwrites old)
```

See [[Catastrophic Forgetting]] for detailed analysis and mitigations.

### Stability-Plasticity Dilemma
- **Stability** — Resist changes to preserve old knowledge
- **Plasticity** — Accept changes to learn new knowledge
- These are fundamentally in tension — you can't maximize both

```
High stability: Remembers everything, learns nothing new
High plasticity: Learns everything new, forgets everything old
Balance:         Learns new while retaining most old (the goal)
```

## Approaches

### Rehearsal / Replay

Mix old data into new training:
```
New training batch = 70% new data + 30% sampled old data
```

The model continues to see examples of old tasks, preventing forgetting.

**Variants:**
- **Experience replay** — Store a buffer of past examples
- **Generative replay** — Generate synthetic examples of old tasks ([[Synthetic Data]])
- **[[Knowledge Distillation|Distillation]] replay** — Use the old model as a teacher for old-task behavior

### Regularization-Based

Penalize changes to weights that are important for previous tasks:

| Method | How It Works |
|---|---|
| **EWC (Elastic Weight Consolidation)** | Estimate which weights matter for old tasks (via Fisher information), penalize changes to them |
| **SI (Synaptic Intelligence)** | Track each weight's contribution to past learning, protect important weights |
| **L2 penalty** | Simple: penalize deviation from previous weights |

### Architecture-Based

Dedicate different parts of the network to different tasks:

| Method | How It Works |
|---|---|
| **Progressive Networks** | Add new columns for new tasks; freeze old columns |
| **PackNet** | Prune and freeze weights for each task; use freed capacity for new tasks |
| **Expert modules** | [[Mixture of Experts]]-like routing: different experts for different tasks |
| **Adapter stacking** | Add new [[Transfer Learning#Parameter-Efficient Fine-Tuning\|LoRA]] adapters for each task; switch or combine at inference |

### Memory-Augmented

External memory that grows with experience:

```
Neural Network + External Memory (vector store, knowledge graph)
                                 ↓
Query memory for relevant past experience → use in current task
Store new experiences → available for future tasks
```

This is conceptually similar to [[Retrieval-Augmented Generation|RAG]] — the model's knowledge is externalized so it can grow without weight changes.

## Continual Learning for LLMs

### Current State (2026)

LLMs don't truly learn continually. Instead, proxy solutions:

| Approach | How | Limitations |
|---|---|---|
| **Periodic retraining** | Train a new model every few months | Expensive ($100M+), not continuous |
| **[[Retrieval-Augmented Generation\|RAG]]** | Retrieve up-to-date information at query time | External, not internalized knowledge |
| **[[Fine-Tuning and Alignment\|Fine-tuning]]** | Adapt to new domains or tasks | Risks [[Catastrophic Forgetting]] |
| **Context-based adaptation** | [[Zero-Shot and Few-Shot Learning\|In-context learning]] from examples | Per-session only, not persistent |
| **Memory files** | [[Claude Code]]'s MEMORY.md, auto-memory | Text-based, not in model weights |

### What True Continual Learning Would Enable

```
Current:  "What happened in the news today?" → "My knowledge cutoff is..."
Future:   "What happened in the news today?" → accurate, up-to-date answer

Current:  Agent fails at a task → same failure next session
Future:   Agent fails at a task → learns, succeeds next time

Current:  New API released → model doesn't know it
Future:   New API released → model learns from docs, starts using it
```

### Research Directions

| Direction | Approach |
|---|---|
| **Online learning** | Update model weights in real-time from user interactions |
| **Modular architectures** | Hot-swap knowledge modules without full retraining |
| **Memory-augmented Transformers** | Persistent external memory integrated into attention |
| **Retrieval + learning** | Combine RAG (immediate) with weight updates (gradual) |
| **Sparse updates** | Only update a small subset of weights for new knowledge |

## Continual Learning for [[AI Coding Harnesses]]

Coding agents face continual learning challenges daily:

```
Session 1: Agent learns your project conventions → stores in MEMORY.md
Session 2: Agent reads MEMORY.md → has context, but it's text, not internalized
Session 3: Project evolves → MEMORY.md may be stale

True continual learning would mean:
Session 1: Agent learns conventions → weights update
Session 2: Agent inherently knows conventions → no file needed
Session 3: Agent notices changes → adapts automatically
```

Current workarounds:
- **CLAUDE.md / AGENTS.md** — Human-maintained persistent context
- **Auto-memory** — Agent-maintained notes (Claude Code's MEMORY.md)
- **Session continuation** — Resume with previous context
- **[[Knowledge Graphs for AI|Knowledge graphs]]** — Structured memory via MCP

These are approximations of continual learning, not the real thing.

## Related

- [[Catastrophic Forgetting]]
- [[Transfer Learning]]
- [[Fine-Tuning and Alignment]]
- [[Retrieval-Augmented Generation]]
- [[Large Language Models]]
- [[Training and Inference]]
- [[Neural Networks]]
- [[Context Management]]
- [[Claude Code]]
