# Transfer Learning

Transfer learning is the technique of taking a model trained on one task and adapting it for a different but related task. It is the foundation of modern AI — virtually every practical AI system uses transfer learning rather than training from scratch.

## The Core Insight

Training a model from scratch requires massive data and compute. But knowledge learned on one task is often useful for another:

```
Task A (large dataset)     Task B (small dataset)
   ↓ train from scratch       ↓ transfer from A
   Model A                    Model B (much better than training from scratch)
```

A model pre-trained on millions of images can recognize edges, textures, shapes, and objects. These features are useful for almost any visual task — medical imaging, satellite analysis, quality control — even with only hundreds of task-specific examples.

## Transfer Learning in LLMs

Modern [[Large Language Models]] are the ultimate transfer learning machines:

### Stage 1: Pre-Training (Source Task)
Train on trillions of tokens of general text:
- Learns language structure, world knowledge, reasoning patterns
- This is the most expensive stage ($10M–$1B+)
- Done once by the model provider

### Stage 2: Fine-Tuning (Target Task)
Adapt the pre-trained model to a specific application:
- [[Fine-Tuning and Alignment|Instruction fine-tuning]] — Learn to follow instructions
- Domain fine-tuning — Specialize in medicine, law, code, etc.
- Alignment — Learn to be helpful and safe (RLHF, DPO)

### Stage 3: In-Context Learning (Zero/Few-Shot)
Adapt at inference time via the prompt alone:
- No weight updates — pure [[Prompt Engineering]]
- Provide examples in the prompt ([[Chain of Thought|few-shot]])
- This is transfer learning at its most extreme — the model adapts to a new task in seconds

## Types of Transfer Learning

### Feature Extraction
Freeze the pre-trained model, add a new output layer:
```
Pre-trained backbone (frozen) → New classification head (trainable)
```
- Cheapest option — only trains the new head
- Works well when the source and target domains are similar

### Full Fine-Tuning
Update all parameters on the target task:
```
Pre-trained model (all parameters trainable) → Target task data
```
- Best quality when you have enough target data
- Expensive — requires significant compute
- Risk of catastrophic forgetting (losing source task knowledge)

### Parameter-Efficient Fine-Tuning (PEFT)
Update only a small subset of parameters:

| Method | What It Does | Parameters Updated |
|---|---|---|
| **LoRA** | Adds low-rank matrices to attention layers | ~0.1% of total |
| **QLoRA** | LoRA on quantized model | ~0.1% + quantized base |
| **Adapters** | Inserts small trainable modules between layers | ~1–5% |
| **Prompt Tuning** | Trains soft prompt tokens | ~0.01% |
| **Prefix Tuning** | Trains prefix activations for each layer | ~0.1% |

LoRA is the dominant approach — it adds tiny matrices to the model that learn task-specific adjustments while the base weights stay frozen. This makes fine-tuning practical on consumer hardware.

## Why Transfer Learning Dominates

### Before Transfer Learning (pre-2018)
- Train a separate model for each task
- Need large labeled datasets per task
- Each model starts from scratch
- Only works if you have enough domain data

### After Transfer Learning
- One pre-trained model serves as the foundation for hundreds of tasks
- Fine-tune with as few as 50–100 examples
- Or skip fine-tuning entirely (zero-shot via prompting)
- Dramatically lower cost and time to deployment

### The Foundation Model Paradigm

Transfer learning created the "foundation model" concept:
```
Foundation Model (pre-trained on broad data)
    ├── Fine-tune for coding → Code model
    ├── Fine-tune for medicine → Medical model
    ├── Fine-tune for chat → Chat assistant
    ├── Prompt for translation → Translator
    └── Prompt for analysis → Analyst
```

One expensive pre-training run → many cheap downstream applications.

## Transfer Learning in [[AI Coding Harnesses]]

Coding agents benefit from transfer learning at every level:
- **Pre-trained models** understand code syntax, patterns, and logic from training data
- **Instruction tuning** teaches them to follow developer instructions
- **CLAUDE.md / AGENTS.md** provides in-context transfer to a specific codebase
- **[[Retrieval-Augmented Generation|RAG]]** transfers knowledge from documentation to the model's context

## Related

- [[Large Language Models]]
- [[Fine-Tuning and Alignment]]
- [[Training and Inference]]
- [[Knowledge Distillation]]
- [[Prompt Engineering]]
- [[Scaling Laws]]
