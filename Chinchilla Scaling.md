# Chinchilla Scaling

Chinchilla is a 2022 DeepMind paper ("Training Compute-Optimal Large Language Models") that fundamentally changed how AI labs allocate their training budgets. Its core finding: most [[Large Language Models]] were **dramatically undertrained** relative to their size.

## The Key Finding

For a given compute budget, there's an optimal balance between model size (parameters) and training data (tokens):

```
Compute-optimal: Parameters and tokens should scale roughly equally

If you 10x your compute budget:
  → ~3.2x more parameters
  → ~3.2x more training tokens
```

Previous practice (following Kaplan et al. 2020) over-invested in model size:
- GPT-3: 175B parameters trained on 300B tokens
- **Chinchilla says:** 175B parameters should be trained on ~3.5T tokens
- GPT-3 was ~10x undertrained

## The Chinchilla Experiment

DeepMind trained over 400 models of varying sizes (70M to 16B parameters) on varying amounts of data, all with the same total compute budget. The results showed a clear, consistent relationship.

### The Proof: Chinchilla vs Gopher

| Model | Parameters | Training Tokens | Performance |
|---|---|---|---|
| Gopher | 280B | 300B | Baseline |
| Chinchilla | 70B | 1.4T | **Better than Gopher on almost every benchmark** |

Chinchilla used **4x fewer parameters** but **4.7x more data** — and was uniformly better. Same compute, radically different allocation.

## Impact on the Industry

### Before Chinchilla (2020–2022)
The prevailing strategy was "bigger model = better":
```
GPT-3:     175B params,  300B tokens
Gopher:    280B params,  300B tokens
MT-NLG:    530B params,  270B tokens
PaLM:     540B params,  780B tokens
```

Models were enormous but saw relatively little data.

### After Chinchilla (2022+)
The industry pivoted to training smaller models on more data:
```
LLaMA 1:     65B params,  1.4T tokens
LLaMA 2:     70B params,  2.0T tokens
Mistral 7B:   7B params, ~8T tokens (estimated)
LLaMA 3:     70B params, 15T+ tokens
```

LLaMA 3 8B trained on 15T tokens is compute-equivalent to a model 10x its size on less data — and it performs remarkably well for its parameter count.

## The Practical Implications

### For AI Labs
- **Train longer, not wider** — More data, not more parameters, per compute dollar
- **Data becomes the bottleneck** — See [[Data Preprocessing for AI]]; you need trillions of high-quality tokens
- **Inference is cheaper** — A 70B model that matches a 280B model is 4x cheaper to serve
- **[[Inference Optimization]] matters more** — Smaller, well-trained models are easier to quantize and deploy

### For the Data Wall
Chinchilla created the "data wall" problem — if optimal training requires ~20 tokens per parameter:
- A 100B model needs 2T tokens
- A 1T model needs 20T tokens
- The entire internet is estimated at ~10–20T high-quality tokens

This drove investment in:
- [[Synthetic Data]] generation
- Data quality over quantity
- Multi-epoch training (reusing data with augmentation)
- Curriculum learning (ordering data strategically)

### For [[Open Source AI Models]]
Chinchilla enabled the open-source revolution:
- A well-trained 7–13B model can match a poorly-trained 70B model
- Smaller models are runnable on consumer hardware (see [[On-Device AI]])
- Meta's LLaMA deliberately followed Chinchilla-optimal ratios
- This is why models like Mistral 7B and Phi-3 "punch above their weight"

## Beyond Chinchilla

### Over-Training
Some practitioners intentionally over-train (more data than Chinchilla-optimal):

```
Chinchilla-optimal for 7B: ~140B tokens
LLaMA 3 8B training:      ~15T tokens (100x "over-trained")
```

Why? Chinchilla optimizes **training compute**. But if you're going to serve the model millions of times, a smaller, over-trained model has lower total cost (cheaper per inference):

```
Total cost = Training cost + (Inference cost × Number of queries)
```

For high-volume serving, over-training the smaller model is economically optimal even if training is "wasteful."

### Chinchilla for Fine-Tuning
The same principle applies to [[Fine-Tuning and Alignment]]:
- Don't fine-tune a huge model on a tiny dataset
- Match the fine-tuning data size to the model's capacity
- [[Transfer Learning#Parameter-Efficient Fine-Tuning|LoRA]] sidesteps this by only updating a small fraction of parameters

### Chinchilla for Inference (Test-Time Compute)
[[Scaling Laws#Scaling Compute at Inference|Test-time compute scaling]] (extended thinking, chain of thought) represents a different allocation axis:
- Instead of training a bigger model, give the existing model more time to think
- This doesn't follow Chinchilla ratios but represents a complementary scaling strategy

## The Formula

The Chinchilla-optimal relationship (approximate):

```
N_opt ∝ C^0.50    (optimal parameters scale as square root of compute)
D_opt ∝ C^0.50    (optimal data scales as square root of compute)
```

Where C is the total compute budget in FLOPs. Parameters and data scale at roughly the same rate.

More practically:
```
Optimal tokens ≈ 20 × parameters
```

A 7B model should see ~140B tokens. A 70B model should see ~1.4T tokens.

## Related

- [[Scaling Laws]]
- [[Large Language Models]]
- [[Training and Inference]]
- [[Data Preprocessing for AI]]
- [[Synthetic Data]]
- [[Open Source AI Models]]
- [[On-Device AI]]
- [[Inference Optimization]]
- [[The Bitter Lesson]]
