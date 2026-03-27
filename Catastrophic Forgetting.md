# Catastrophic Forgetting

Catastrophic forgetting is a phenomenon where a [[Neural Networks|neural network]] loses previously learned knowledge when trained on new data. It is a fundamental challenge in [[Fine-Tuning and Alignment|fine-tuning]], continual learning, and [[Transfer Learning]].

## The Problem

When you fine-tune a model on new data, the weight updates that improve performance on the new task can overwrite the weights responsible for previous capabilities:

```
Pre-trained model: Good at English, French, German, code, math

Fine-tune on medical Q&A:
After fine-tuning: Excellent at medical Q&A
                   Worse at French (forgot some)
                   Worse at code (forgot some)
                   Worse at math (forgot some)
```

The model didn't "choose" to forget — gradient descent moved weights toward the new objective, and those movements happened to damage old capabilities.

## Why It Happens

Neural networks store knowledge in a **distributed** fashion — the same weights participate in many tasks. Updating weights for task B disrupts the representations used by task A.

```
Weights W₁...W₁₀:
  Task A uses: W₁, W₃, W₅, W₇, W₉
  Task B uses: W₂, W₃, W₅, W₈, W₁₀
  Shared:      W₃, W₅ ← updating these for B can hurt A
```

## Severity Spectrum

| Scenario | Forgetting Risk |
|---|---|
| Fine-tune on similar domain | Low — shared representations help both tasks |
| Fine-tune on different domain | Medium — some overlap, some conflict |
| Fine-tune on small dataset | High — few examples, aggressive weight changes |
| Fine-tune on contradictory data | Very high — directly overwrites old knowledge |

## Mitigation Strategies

### Learning Rate Control
Use a very small learning rate for fine-tuning:
- Small updates are less likely to disrupt existing knowledge
- Trade-off: slower convergence on new task
- Typical: fine-tuning LR is 10–100x smaller than pre-training LR

### LoRA and PEFT

[[Transfer Learning#Parameter-Efficient Fine-Tuning|Parameter-efficient methods]] (LoRA, QLoRA, adapters) are the most practical solution:

```
Pre-trained weights (frozen, unchanged)
         ↓
    + Small adapter weights (learned for new task)
         ↓
    Combined output
```

Since the base weights aren't modified, the model retains all pre-trained knowledge. The adapter adds new capabilities on top.

### Regularization

Add a penalty for changing weights too much from their pre-trained values:

| Method | How It Works |
|---|---|
| **L2 regularization** | Penalize large deviations from pre-trained weights |
| **EWC (Elastic Weight Consolidation)** | Penalize changes to weights that are important for previous tasks |
| **Knowledge distillation** | Use the original model as a teacher to preserve its behavior |

### Replay / Rehearsal

Mix old task data into new task training:

```
Fine-tuning batch = 70% new task data + 30% original pre-training data
```

The model continues to see examples of its old capabilities, preventing forgetting. This is commonly used in LLM fine-tuning.

### Progressive Training

Train on tasks sequentially, but with overlap:

```
Phase 1: Task A (100%)
Phase 2: Task A (50%) + Task B (50%)
Phase 3: Task A (25%) + Task B (25%) + Task C (50%)
```

Gradual introduction prevents sudden disruption.

## Forgetting in LLM Context

### Fine-Tuning
When organizations fine-tune [[Open Source AI Models]] on domain-specific data:
- Medical fine-tuning might degrade code generation
- Code fine-tuning might degrade conversational ability
- Mitigation: mix domain data with general data during fine-tuning

### Alignment
[[Reinforcement Learning from Human Feedback|RLHF]] can cause forgetting of raw capabilities:
- The model becomes more helpful but may lose some factual knowledge
- Known as the "alignment tax"
- Mitigation: carefully balanced reward signal, KL divergence penalty

### Continual Learning
The dream: a model that continuously learns from new data without forgetting old knowledge. This remains an open research problem. Current LLMs are trained once and frozen — they don't learn from user interactions.

## Measuring Forgetting

```
1. Evaluate model on benchmark tasks BEFORE fine-tuning → Score_before
2. Fine-tune on new task
3. Evaluate on SAME benchmark tasks AFTER fine-tuning → Score_after
4. Forgetting = Score_before - Score_after
```

If forgetting is significant, adjust the fine-tuning strategy.

## Related

- [[Fine-Tuning and Alignment]]
- [[Transfer Learning]]
- [[Training and Inference]]
- [[Neural Networks]]
- [[Knowledge Distillation]]
- [[Reinforcement Learning from Human Feedback]]
