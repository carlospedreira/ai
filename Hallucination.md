# Hallucination

Hallucination is when a [[Large Language Models|Large Language Model]] generates output that is fluent and confident but factually incorrect, fabricated, or inconsistent with the provided context. It is one of the most significant limitations of current AI systems.

## Why It Happens

LLMs are trained to predict the **most likely next token**, not the **most truthful** one. This creates several failure modes:

### 1. Training Data Gaps
The model encounters a question outside its training data and generates a plausible-sounding but fabricated answer rather than saying "I don't know."

### 2. Statistical Patterns Over Facts
The model learns correlations, not facts. If "the capital of Australia" frequently co-occurs with "Sydney" in web text (due to discussions about the misconception), the model may confidently state Sydney is the capital.

### 3. Sycophancy
Models fine-tuned with [[Reinforcement Learning|RLHF]] may learn that agreeing with the user gets higher reward scores, leading to validation of incorrect premises.

### 4. Confabulation in Long Contexts
In long conversations or complex tool-use chains, the model may "forget" earlier context and fill in gaps with plausible but incorrect information.

### 5. Source Conflation
When multiple pieces of information are in context, the model may mix attributes from different sources — attributing the wrong author to a quote, or the wrong version number to a library.

## Types of Hallucination

| Type | Description | Example |
|---|---|---|
| **Factual** | States incorrect facts | "Python was created by Guido van Rossum in 1995" (it was 1991) |
| **Fabrication** | Invents nonexistent things | Citing a paper that doesn't exist |
| **Attribution** | Assigns correct facts to wrong sources | Wrong function name for an API |
| **Logical** | Draws invalid conclusions from valid premises | Flawed mathematical reasoning |
| **Code** | Generates code using APIs/functions that don't exist | `response.data.items.forEach()` on a library that has no such API |

## Hallucination in AI Coding

For [[AI Coding Harnesses]], hallucination manifests as:

- **Invented APIs** — Generating code that calls functions that don't exist in the library being used
- **Wrong syntax** — Correct-looking code for the wrong version of a language/framework
- **Phantom files** — Referencing file paths that don't exist in the codebase
- **Incorrect fixes** — Confidently patching a bug with code that introduces a new one
- **Made-up flags** — Using CLI flags or config options that don't exist

### Why Agents Hallucinate Less

[[Agentic Coding Paradigm|Agentic systems]] have a natural advantage: they can **verify** their output by reading files, running code, and checking results. The agent loop self-corrects:

```
Agent generates code → Runs tests → Tests fail → Agent reads error →
Agent fixes the issue → Runs tests again → Tests pass
```

This is why [[AI Coding Workflows|test-driven workflows]] with AI agents are so effective — tests catch hallucinations before they ship.

## Mitigation Strategies

### For Model Developers
- **RLHF / Constitutional AI** — Train models to express uncertainty ([[Fine-Tuning and Alignment]])
- **Grounding** — Connect models to real data sources via [[Retrieval-Augmented Generation|RAG]]
- **Calibration training** — Teach models to say "I'm not sure" when uncertain

### For Coding Agent Users
- **Run tests** — Always have the agent run tests after changes
- **Read the diff** — Review what the agent actually wrote
- **Use LSP integration** — [[OpenCode]]'s LSP feeds real compiler errors back to the model
- **Verify with tools** — The agent should read the actual file/API docs, not guess
- **Use RAG** — Connect documentation via [[Model Context Protocol|MCP]] so the model has real references

### For Application Builders
- **[[Retrieval-Augmented Generation]]** — Ground answers in real documents
- **Citations** — Require the model to cite its sources
- **Constrained generation** — Use [[Structured Output]] to limit possible responses
- **Multi-step verification** — Have a second model check the first model's output

## The Fundamental Tension

Hallucination is not a bug that can be fully patched — it's a structural consequence of how LLMs work. The same mechanism that enables creative writing, analogical reasoning, and novel code generation also enables fabrication. Making models more conservative reduces hallucination but also reduces capability.

The practical solution is not "eliminate hallucination" but **"build systems that catch and correct it"** — which is exactly what [[AI Coding Harnesses]] do through the agent loop.

## Related

- [[Large Language Models]]
- [[Fine-Tuning and Alignment]]
- [[AI Ethics and Safety]]
- [[Retrieval-Augmented Generation]]
- [[AI Coding Harnesses]]
- [[AI Coding Workflows]]
