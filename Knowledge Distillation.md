# Knowledge Distillation

Knowledge distillation is a technique where a large, powerful "teacher" model transfers its capabilities to a smaller, faster "student" model. The student learns to mimic the teacher's behavior rather than learning from raw data alone.

## The Core Idea

```
Teacher Model (large, slow, expensive)
        ↓ generates training signal
Student Model (small, fast, cheap)
```

Instead of training the student on hard labels ("this is a cat"), you train it on the teacher's **soft outputs** — the full probability distribution across all possible answers. These soft outputs contain richer information: "this is 90% cat, 8% lynx, 2% dog" tells the student about relationships between concepts.

## How It Works

### Classic Distillation (Hinton et al., 2015)

1. Train the teacher model normally on the dataset
2. Run the teacher on training examples to produce **soft labels** (probability distributions at a high temperature)
3. Train the student to match both:
   - The soft labels from the teacher (distillation loss)
   - The hard labels from the ground truth (standard loss)
4. The combined loss transfers the teacher's "dark knowledge"

### Temperature Scaling

At temperature T=1, the output distribution is peaked:
```
cat: 0.95, lynx: 0.04, dog: 0.01
```

At temperature T=5, the distribution is softened:
```
cat: 0.45, lynx: 0.30, dog: 0.25
```

Higher temperature reveals more about the relationships between classes — this is where "dark knowledge" lives.

## Distillation in the LLM Era

Modern distillation for [[Large Language Models]] typically involves:

### Output Distillation
Student learns from the teacher's generated text:
```
Teacher generates: [high-quality response to prompt]
Student trains on: [that same response]
```

This is essentially [[Synthetic Data]] generation by the teacher.

### Reasoning Distillation
Student learns the teacher's reasoning process:
```
Teacher: "Let me think step by step... [chain of thought] ... Answer: 42"
Student trains on the full reasoning trace
```

DeepSeek R1 used this approach — distilling reasoning capabilities from a larger model into smaller variants.

### Feature Distillation
Student learns to match the teacher's internal representations:
```
Teacher hidden state at layer 20 → Student hidden state at layer 8
Minimize difference between representations
```

## Notable Examples

| Student | Teacher | Result |
|---|---|---|
| Alpaca (7B) | GPT-3.5 | Instruction-following at 1/100th the cost |
| Vicuna (13B) | ChatGPT conversations | Competitive chat capability |
| Phi-3 (3.8B) | Larger models + [[Synthetic Data]] | Outperforms 7B models on many benchmarks |
| DeepSeek R1 distilled | DeepSeek R1 (671B) | Reasoning capability in 7B–70B models |
| Whisper (tiny) | Whisper (large) | Fast speech recognition on devices |

## Why Distillation Matters

### Cost Reduction
- Run a 7B model instead of a 70B model
- 10x cheaper inference, 5x lower latency
- Enables deployment on edge devices, phones, laptops

### Democratization
- Organizations without massive GPU clusters can use distilled models
- [[Open Source AI Models]] often start as distilled versions of proprietary models
- Local inference becomes practical (see [[Inference Optimization]])

### Specialized Models
Distill a general-purpose teacher into task-specific students:
- Code-specialized model from a general model
- Medical Q&A model from a general model
- Each student is smaller and better at its specific task

## Distillation vs Fine-Tuning

| Aspect | Distillation | [[Fine-Tuning and Alignment\|Fine-Tuning]] |
|---|---|---|
| Data source | Teacher model outputs | Human-labeled or curated data |
| What's learned | Teacher's behavior and "dark knowledge" | Task-specific patterns |
| Typical use | Compress a large model | Adapt a model to a domain |
| Can combine? | Yes — distill then fine-tune | Yes — fine-tune then distill |

## Legal and Ethical Considerations

- Many API terms of service prohibit using outputs to train competing models
- The line between "distillation" and "using API outputs" is legally contested
- Open-source teachers (LLaMA, Mistral) avoid this issue
- The "Alpaca moment" (2023) sparked ongoing debate about model output licensing

## Related

- [[Large Language Models]]
- [[Synthetic Data]]
- [[Fine-Tuning and Alignment]]
- [[Open Source AI Models]]
- [[Inference Optimization]]
- [[Scaling Laws]]
