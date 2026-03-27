# Zero-Shot and Few-Shot Learning

Zero-shot and few-shot learning are paradigms where a [[Large Language Models|Large Language Model]] performs tasks it was never explicitly trained for — either with no examples (zero-shot) or just a handful (few-shot). They are the practical manifestation of [[Emergent Abilities]] and the foundation of [[Prompt Engineering]].

## Zero-Shot Learning

The model performs a task based solely on instructions, with no examples:

```
Prompt: "Classify the following review as positive or negative:
         'The battery life is terrible and the screen cracks easily.'"

Model:  "Negative"
```

The model was never trained on a "review classification" dataset. It infers the task from the instruction alone — an emergent capability of large-scale pre-training.

### Why It Works

During pre-training on trillions of tokens, the model encounters text in every conceivable format — reviews followed by summaries, questions followed by answers, code followed by explanations. It learns the *meta-pattern* of following instructions, not just specific tasks.

### Zero-Shot Chain of Thought

Adding "Let's think step by step" to any zero-shot prompt dramatically improves reasoning (see [[Chain of Thought]]):

```
Prompt: "If a train travels 120km in 2 hours, and then 180km in 3 hours,
         what was its average speed? Let's think step by step."

Model:  "Total distance: 120 + 180 = 300km
         Total time: 2 + 3 = 5 hours
         Average speed: 300 / 5 = 60 km/h"
```

## Few-Shot Learning

Provide a few examples in the prompt before the actual task:

```
Prompt:
  Review: "Amazing product, works perfectly!" → Positive
  Review: "Broke after one day, waste of money" → Negative
  Review: "It's okay, nothing special" → Neutral
  Review: "The camera quality blew me away" →

Model:  "Positive"
```

### Why Few-Shot Is Powerful

- **No training required** — Examples are in the prompt, not the weights
- **Instant adaptation** — Switch tasks by changing examples
- **Format specification** — Examples implicitly define the output format
- **Disambiguation** — Examples clarify what you mean by ambiguous instructions

### How Many Examples?

| Examples | Effect |
|---|---|
| 0 (zero-shot) | Relies entirely on instructions and model knowledge |
| 1–3 | Usually sufficient for format and intent |
| 5–10 | Stronger pattern lock, better for nuanced tasks |
| 10–30 | Diminishing returns; approaches [[Fine-Tuning and Alignment\|fine-tuning]] territory |
| 30+ | Consider fine-tuning instead (cheaper per inference) |

## In-Context Learning

Zero-shot and few-shot are both forms of **in-context learning** — the model adapts to a task using only information in its context window, without any weight updates.

```
Traditional ML: Learn task by updating model weights (training)
In-context:     Learn task by reading examples in the prompt (no weight update)
```

This is fundamentally different from [[Fine-Tuning and Alignment|fine-tuning]]:

| Aspect | In-Context Learning | Fine-Tuning |
|---|---|---|
| Weight updates | None | Yes |
| Speed to adapt | Instant | Hours to days |
| Cost | Per-token (context space) | Training compute |
| Persistence | Per-session only | Permanent |
| Flexibility | Change examples = change task | Retrain = change task |
| Quality ceiling | Limited by context size | Can exceed in-context |

## Few-Shot for Code

Few-shot is heavily used in [[AI Coding Harnesses]]:

### Code Style Examples
```
// Example 1:
// Input: fetch user by email
// Output:
async function getUserByEmail(email: string): Promise<User | null> {
  return await db.users.findUnique({ where: { email } });
}

// Example 2:
// Input: fetch orders by customer ID
// Output:
async function getOrdersByCustomerId(customerId: string): Promise<Order[]> {
  return await db.orders.findMany({ where: { customerId } });
}

// Now generate:
// Input: fetch products by category
```

The examples teach the model the project's code style, naming conventions, and ORM usage.

### CLAUDE.md as Few-Shot Context

Project instruction files (CLAUDE.md, AGENTS.md) serve as persistent few-shot context:

```markdown
## Code Examples
When writing API handlers, follow this pattern:
```typescript
export const handler = async (req: Request, res: Response) => {
  try {
    const result = await service.method(req.params.id);
    res.json({ data: result });
  } catch (error) {
    next(error);
  }
};
```

This is few-shot learning embedded in [[Context Engineering]].

## The GPT-3 Breakthrough

GPT-3 (2020) was the first model to demonstrate strong few-shot learning at scale. The paper "Language Models are Few-Shot Learners" showed that a sufficiently large model could perform tasks competitively with fine-tuned models just from examples in the prompt — a paradigm shift that launched the modern LLM era.

| Method | Training Examples | LAMBADA Accuracy |
|---|---|---|
| Fine-tuned SOTA | Thousands | 68.0% |
| GPT-3 zero-shot | 0 | 76.2% |
| GPT-3 few-shot | 15 | 86.4% |

A model with *no task-specific training* outperformed all fine-tuned models.

## Limitations

- **Context window cost** — Examples consume precious tokens (see [[Tokenomics of AI]])
- **Example quality matters** — Bad examples teach bad patterns
- **Not all tasks respond** — Complex reasoning still needs [[Chain of Thought]] or [[Fine-Tuning and Alignment|fine-tuning]]
- **Sensitivity to formatting** — Small changes in example format can change results significantly

## Related

- [[Prompt Engineering]]
- [[Chain of Thought]]
- [[Large Language Models]]
- [[Emergent Abilities]]
- [[Fine-Tuning and Alignment]]
- [[Context Engineering]]
- [[Transfer Learning]]
- [[Scaling Laws]]
