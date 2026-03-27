# AI Job Market and Skills

The rise of [[Artificial Intelligence]] — and [[AI Coding Harnesses|AI coding tools]] in particular — is reshaping what skills matter for software developers, data scientists, and AI practitioners. This note maps the evolving landscape of AI-relevant careers and the competencies that drive value in the AI era.

## Emerging Roles

### AI Engineer
Builds applications that use LLMs — [[Retrieval-Augmented Generation|RAG]] pipelines, [[AI Agents and Sub-Agents|agents]], chatbots, AI-powered features. Doesn't train models; uses them via APIs.

**Key skills:** [[Prompt Engineering]], [[Context Engineering]], [[AI Agent Frameworks]], [[Model Context Protocol|MCP]], API integration, evaluation

### ML Engineer
Trains, fine-tunes, and deploys models. Bridges research and production.

**Key skills:** [[AI Development Frameworks|PyTorch/JAX]], [[Training and Inference|training pipelines]], [[Inference Optimization|model serving]], [[Hyperparameter Tuning]], distributed computing

### AI Research Scientist
Advances the field — new architectures, training methods, capabilities.

**Key skills:** Deep math (linear algebra, probability, optimization), paper reading/writing, experiment design, [[Scaling Laws]] intuition

### Prompt Engineer / AI Whisperer
Specializes in getting the best output from LLMs through prompt design and [[Context Engineering]].

**Key skills:** [[Prompt Engineering]], [[Zero-Shot and Few-Shot Learning|few-shot design]], [[Chain of Thought]], [[CLAUDE.md Best Practices|project instruction files]], [[Conversation Design for AI]]

### Agent Operator
Manages fleets of [[Autonomous Coding Agents]] and [[Multi-Agent Systems]]. A new role emerging in organizations with heavy AI adoption.

**Key skills:** [[AI Agents in Production]], [[AI Observability]], [[Tokenomics of AI|cost management]], [[Approval Flows and Sandboxing|safety configuration]]

### AI Safety Researcher
Works on ensuring AI systems behave as intended. See [[AI Safety and Alignment Research]].

**Key skills:** [[Reinforcement Learning from Human Feedback|RLHF]], [[Reward Modeling]], interpretability, red teaming, formal methods

### Data Engineer (AI-Specialized)
Builds the data pipelines that feed AI training. See [[Data Preprocessing for AI]].

**Key skills:** [[Data Preprocessing for AI|Data cleaning]], [[Data Labeling and Annotation|annotation pipelines]], [[Synthetic Data|synthetic data generation]], data quality, distributed storage

## The Developer Skill Shift

### Skills Becoming More Valuable

| Skill | Why |
|---|---|
| **System design / architecture** | AI handles code; humans handle design decisions |
| **[[Context Engineering]]** | Directly determines AI tool effectiveness |
| **Code review** | More AI-generated code to review ([[AI Code Review]]) |
| **Testing strategy** | Defining what to test is human judgment; AI executes ([[AI-Powered Testing]]) |
| **Domain expertise** | AI doesn't know your business rules |
| **Communication** | Describing intent clearly to AI (and humans) |
| **Security awareness** | [[AI and Security\|AI introduces new attack surfaces]] |
| **Evaluation skills** | Knowing whether AI output is good ([[Evaluation and Metrics]]) |

### Skills Becoming Less Critical

| Skill | Why |
|---|---|
| **Syntax memorization** | AI handles syntax perfectly |
| **Boilerplate writing** | AI generates repetitive code faster |
| **Stack Overflow searching** | AI provides contextualized answers |
| **Manual refactoring** | [[AI-Assisted Refactoring]] handles mechanical changes |
| **Typing speed** | The bottleneck is thinking, not typing |

### Skills That Remain Critical

| Skill | Why |
|---|---|
| **Debugging complex systems** | AI helps but can't replace deep system understanding |
| **Performance optimization** | Requires profiling, measurement, domain knowledge |
| **Understanding trade-offs** | Every design decision involves trade-offs AI can't evaluate |
| **Team collaboration** | AI doesn't replace human communication and coordination |
| **Learning ability** | The field changes constantly; adaptability is paramount |

## Hiring Trends (2025–2026)

### High Demand
- AI Engineers (application builders)
- ML Engineers (model training and deployment)
- AI Safety researchers
- "AI-augmented" developers (senior devs who leverage AI effectively)

### Growing Demand
- Agent operators / AI infrastructure engineers
- Data engineers specialized in AI pipelines
- [[AI Regulation|AI governance]] and compliance specialists
- AI UX designers ([[Conversation Design for AI]])

### Changing Demand
- Traditional software developers → expected to use AI tools ([[AI and Developer Productivity]])
- Data scientists → increasingly need engineering skills alongside analysis
- Junior developers → higher bar for entry but AI tools accelerate learning

## Learning Path

### For Software Developers Entering AI

```
1. Use AI coding tools daily (Claude Code, Copilot, Cursor)
2. Understand LLM basics (this vault: Transformers → LLMs → Prompt Engineering)
3. Build a RAG application (embeddings, vector search, retrieval)
4. Build an agent (tool use, function calling, agent frameworks)
5. Learn evaluation (metrics, benchmarks, testing AI systems)
6. Specialize (safety, infrastructure, applications, or research)
```

### For AI Researchers

```
1. Strong math foundation (linear algebra, calculus, probability, optimization)
2. Implement core architectures from scratch (attention, transformer, training loop)
3. Read landmark papers (Attention Is All You Need, Scaling Laws, Chinchilla)
4. Reproduce published results
5. Run experiments, analyze scaling behavior
6. Contribute to open source (Hugging Face, PyTorch, model implementations)
```

### For Non-Technical Professionals

```
1. Understand AI capabilities and limitations (this vault's core concepts)
2. Learn prompt engineering (Prompt Engineering, Context Engineering)
3. Understand the economics (Tokenomics of AI, Scaling Laws)
4. Learn AI governance (AI Regulation, AI Ethics and Safety, AI and Copyright)
5. Practice with AI tools in your domain
```

## The Productivity Multiplier

[[AI and Developer Productivity]] data suggests:
- Developers using AI tools are 20–55% more productive on eligible tasks
- The best AI-augmented developers are not the best at using AI — they're the best developers who also use AI
- AI amplifies existing skill: a senior developer + AI > a junior developer + AI

This means the most valuable investment is still deep technical expertise — AI multiplies it.

## Related

- [[AI and Developer Productivity]]
- [[AI Coding Harnesses]]
- [[AI Pair Programming]]
- [[Prompt Engineering]]
- [[Context Engineering]]
- [[AI Safety and Alignment Research]]
- [[AI Regulation]]
- [[AI Development Frameworks]]
- [[Artificial Intelligence]]
