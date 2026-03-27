# Synthetic Data

Synthetic data is artificially generated data used to train, fine-tune, or evaluate [[Large Language Models]] and other AI systems. As natural data sources become exhausted, synthetic data has become essential for continued [[Scaling Laws|scaling]].

## Why Synthetic Data?

### The Data Wall
- The internet has a finite amount of high-quality text
- Estimates suggest we're approaching the limit of naturally available training data
- Models need more data than exists to continue improving on current trajectories

### Quality Over Quantity
- Most internet text is low-quality (spam, duplicates, errors)
- Curated synthetic data can be higher quality than scraped web data
- Targeted generation can fill gaps in specific domains

### Cost and Access
- Human-labeled data is expensive ($5–$50+ per example)
- Some domains lack public data (medical, legal, proprietary code)
- Synthetic generation can produce millions of examples cheaply

## Types of Synthetic Data

### Model-Generated Text

Use a large model to generate training data for a smaller model:

```
Teacher model (GPT-4, Claude) → generates examples → Student model (LLaMA 7B) trained on them
```

This is called **distillation** when the goal is to transfer capability to a smaller model.

### Instruction-Response Pairs

Generate training data for [[Fine-Tuning and Alignment|instruction fine-tuning]]:

```
"Generate a diverse set of coding questions and high-quality answers"
→ Thousands of (instruction, response) pairs
→ Fine-tune a model on these pairs
```

Notable examples:
- **Alpaca** — Stanford generated 52K instruction-response pairs using GPT-3.5
- **WizardLM** — Used "Evol-Instruct" to progressively make instructions harder
- **Orca** — Used detailed explanations from GPT-4 to train smaller models

### Code Synthesis

Generate code + test case pairs:

```
1. Generate a function specification
2. Generate an implementation
3. Generate test cases
4. Execute tests to verify correctness
5. Keep only examples that pass
```

This creates verified training data — the execution step filters out bad examples.

### Self-Play and Self-Improvement

The model generates data from itself, then trains on the best examples:

```
Model generates multiple solutions → Evaluate quality → Train on best ones
```

Used in:
- **STaR** (Self-Taught Reasoner) — Model generates reasoning chains, trains on correct ones
- **Constitutional AI** — Model critiques and revises its own outputs
- **ReST** (Reinforced Self-Training) — Generate, filter, retrain loop

### Data Augmentation

Transform existing data to create variations:
- Paraphrase questions while keeping the answer
- Translate to other languages and back
- Add noise and perturbations
- Change coding style while preserving semantics

## Synthetic Data in Practice

### DeepSeek R1
Trained its reasoning capability using synthetic chain-of-thought data, achieving performance rivaling OpenAI's o1 at a fraction of the training cost.

### Phi Models (Microsoft)
Microsoft's Phi series was trained primarily on synthetic "textbook-quality" data, demonstrating that small models trained on excellent synthetic data can outperform larger models trained on raw web data.

### Code Models
Most [[Open Source AI Models|open-source code models]] use synthetic code generation pipelines:
1. Generate code from specifications
2. Execute to verify correctness
3. Generate test cases
4. Only keep verified examples

## Risks and Limitations

### Model Collapse
When models train on other models' outputs, errors can compound:
```
Model A → generates data → Model B trained on it → generates worse data → Model C ...
```

Each generation amplifies biases and reduces diversity. Known as "model collapse" or "Habsburg AI."

### Monoculture
Synthetic data from one model type produces homogeneous training data:
- All examples share the same style and biases
- Reduces diversity of perspectives and approaches
- Can narrow the model's capability distribution

### Quality Ceiling
A student model trained on synthetic data from a teacher model generally can't exceed the teacher's quality. You need a better teacher (or real data) to push the frontier.

### Legal and Ethical Questions
- Is it legal to use GPT-4's output to train a competing model?
- Terms of service typically prohibit this
- The legal landscape is evolving

## The Emerging Pipeline

Modern model training increasingly looks like:

```
1. Pre-train on natural data (web, books, code)
2. Generate synthetic instruction data with a frontier model
3. Fine-tune on synthetic + curated human data
4. Self-play / self-improvement loops
5. RLHF/DPO with human preferences
```

Synthetic data is no longer optional — it's a core ingredient.

## Related

- [[Large Language Models]]
- [[Training and Inference]]
- [[Scaling Laws]]
- [[Fine-Tuning and Alignment]]
- [[Open Source AI Models]]
