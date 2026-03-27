# Prompt Engineering

Prompt engineering is the practice of crafting inputs to [[Large Language Models]] to get better, more reliable outputs. It is the primary interface between humans and modern AI systems.

## Why It Matters

LLMs are sensitive to how questions are phrased. The same underlying question can produce vastly different results depending on the prompt. Good prompting can:
- Improve accuracy and reduce [[Large Language Models#Limitations|hallucination]]
- Get structured, formatted output
- Unlock reasoning capabilities
- Control tone, style, and detail level

## Core Techniques

### Zero-Shot Prompting
Ask the model directly without examples.
> "Classify this review as positive or negative: ..."

### Few-Shot Prompting
Provide examples before the actual task.
> "Review: Great product! → Positive
> Review: Broke after a day → Negative
> Review: Decent for the price → ?"

### Chain-of-Thought (CoT)
Ask the model to reason step by step.
> "Solve this problem. Think step by step."

This dramatically improves performance on math, logic, and multi-step reasoning tasks.

### System Prompts
Set the model's role, constraints, and behavior upfront.
> "You are a helpful coding assistant. Always provide working code with explanations."

### Structured Output
Request specific formats.
> "Return your answer as JSON with keys: name, category, confidence."

## Advanced Techniques

- **Self-consistency** — Generate multiple reasoning paths, take the majority answer
- **Tree of thought** — Explore branching reasoning paths
- **Retrieval-Augmented Generation (RAG)** — Inject relevant documents into the prompt to ground responses in facts
- **Tool use** — Instruct the model to call external tools (calculators, APIs, databases)
- **Prompt chaining** — Break complex tasks into a pipeline of simpler prompts

## Best Practices

1. **Be specific** — Vague prompts get vague answers
2. **Provide context** — Include relevant background information
3. **Define the format** — Specify how you want the output structured
4. **Iterate** — Refine prompts based on results
5. **Use constraints** — Set boundaries ("in 3 sentences", "for a 5-year-old")

## Related

- [[Large Language Models]]
- [[Natural Language Processing]]
- [[Generative AI]]
