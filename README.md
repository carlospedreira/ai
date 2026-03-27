# AI Knowledge Vault

An interconnected Obsidian vault covering artificial intelligence from first principles to production agent systems. **141 notes, 2,100+ wikilinks, 19,000+ lines** — designed to be explored as a graph, not read linearly.

## Structure

```
00 Reference/            Hub note, glossary, timeline, community, careers
01 Foundations/          Neural networks, backprop, activations, loss functions, regularization
02 Architectures/       Transformers, attention, CNNs, RNNs, SSMs, MoE, GANs, diffusion
03 Language Models/     LLMs, tokens, scaling laws, prompting, chain-of-thought, embeddings
04 Training/            Data pipelines, fine-tuning, RLHF, distillation, continual learning
05 Model Families/      GPT, BERT, Claude, LLaMA, open source ecosystem
06 Infrastructure/      Hardware, inference optimization, serving, caching, on-device, energy
07 Agents/              Agent systems, design patterns, frameworks, production, observability
08 Coding Tools/        Harnesses (Claude Code, Codex, OpenCode), MCP, ACP, approval flows
09 Coding Workflows/    Pair programming, debugging, testing, refactoring, CI/CD, migration
10 Retrieval and Search/ RAG, semantic search, knowledge graphs, evaluation metrics
11 Applications/        Healthcare, finance, education, science, creative work, NLP, CV
12 Safety and Governance/ Ethics, alignment research, regulation, copyright, security
```

## Starting Points

- **[[Artificial Intelligence]]** — The hub note linking to everything
- **[[AI Glossary]]** — A-Z quick reference with links to full notes
- **[[AI History and Timeline]]** — 1943 to 2026 in chronological order
- **[[AI Coding Harnesses]]** — Entry point for the coding tools cluster
- **[[Large Language Models]]** — Entry point for the LLM cluster
- **[[Transformers]]** — Entry point for the architecture cluster

## How to Use

**In Obsidian:** Open the vault, then use Graph View (`Cmd+G`) to explore connections visually. The densest hubs are Artificial Intelligence, Large Language Models, Transformers, AI Coding Harnesses, and Neural Networks.

**As reference:** Search for any AI term — there's likely a note with context, comparisons, and links to related concepts.

**For learning:** Follow a path through the numbered folders (00→12) for a structured curriculum, or follow wikilinks for a choose-your-own-adventure exploration.

## Paths Through the Vault

### "How do LLMs work?"
Neural Networks → Attention Mechanism → Transformers → Attention Is All You Need → Large Language Models → Tokens and Tokenization → Scaling Laws → Causal vs Masked Language Modeling → Beam Search and Sampling

### "How do I use AI coding tools?"
AI Coding Harnesses → Claude Code / OpenAI Codex / OpenCode → Agentic Coding Paradigm → AI Coding Workflows → CLAUDE.md Best Practices → Context Engineering → AI Pair Programming

### "How are models trained?"
Data Preprocessing for AI → Tokenizer Design → Training and Inference → Loss Functions and Optimization → Backpropagation → Scaling Laws → Chinchilla Scaling → Instruction Tuning → Reinforcement Learning from Human Feedback → Reward Modeling

### "How do AI agents work?"
AI Agents and Sub-Agents → Tool Use in AI → Function Calling → Agentic Design Patterns → Multi-Agent Systems → AI Agents in Production → AI Observability

### "What are the safety concerns?"
AI Ethics and Safety → AI Safety and Alignment Research → Prompt Injection → Hallucination and Grounding → AI and Security → Responsible AI Deployment → AI Regulation

### "How did we get here?"
AI History and Timeline → The Bitter Lesson → Attention Is All You Need → GPT Evolution → BERT → Claude History → LLaMA and Open Model Revolution → Scaling Laws → Emergent Abilities

## Coverage

| Domain | Notes | Highlights |
|---|---|---|
| Architecture | 14 | Full Transformer anatomy, attention variants, CNNs through SSMs |
| Foundations | 13 | Backprop to regularization, every building block |
| Language Models | 18 | Tokenization through chain-of-thought, scaling laws to sampling |
| Training | 15 | Raw data through RLHF, distillation through continual learning |
| Model Families | 5 | Deep dives on GPT, BERT, Claude, LLaMA lineages |
| Infrastructure | 12 | GPUs through on-device, serving through energy sustainability |
| Agents | 11 | Design patterns, frameworks, production, multi-agent |
| Coding Tools | 13 | Three harness deep-dives, MCP, ACP, approval flows |
| Coding Workflows | 13 | Every phase: debug, test, refactor, review, migrate, document |
| Retrieval | 5 | RAG, semantic search, knowledge graphs, evaluation |
| Applications | 9 | Healthcare, finance, education, science, creative |
| Safety | 7 | Alignment, regulation, copyright, prompt injection |
| Reference | 5 | Hub, glossary, timeline, community, careers |

## Built With

Every note in this vault was written by Claude Opus 4.6 via [Claude Code](https://claude.ai/claude-code), Anthropic's agentic coding tool. The vault itself is a demonstration of AI-assisted knowledge work — 141 interconnected notes produced across 28 commits.
