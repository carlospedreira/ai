# Data Labeling and Annotation

Data labeling is the process of adding meaningful tags, classifications, or annotations to raw data so that [[Machine Learning]] models can learn from it. It is the often-invisible human labor that powers [[Supervised vs Unsupervised Learning|supervised learning]] and [[Fine-Tuning and Alignment|alignment]].

## Why Labeling Matters

Models learn patterns from labeled examples:
```
Image of a cat → Label: "cat"
"I love this product!" → Label: "positive sentiment"
Code snippet → Label: "contains SQL injection vulnerability"
Response A vs Response B → Label: "A is better" (preference data)
```

Without labels, supervised learning is impossible. Without preference labels, [[Fine-Tuning and Alignment|RLHF]] is impossible.

## Types of Labels

### Classification Labels
Assign categories to data:
- Image classification: "cat", "dog", "car"
- Text classification: "spam", "not spam"
- Sentiment: "positive", "negative", "neutral"

### Bounding Boxes and Segmentation
For [[Computer Vision]]:
- Draw rectangles around objects in images
- Label every pixel with a category (semantic segmentation)
- Outline individual object instances (instance segmentation)

### Sequence Labels
For [[Natural Language Processing]]:
- Named Entity Recognition: "Paris" → Location, "Google" → Organization
- Part-of-speech tagging: "runs" → Verb

### Preference Pairs
For [[Fine-Tuning and Alignment|RLHF/DPO]]:
```
Prompt: "Explain quantum computing"
Response A: [detailed, accurate explanation]
Response B: [vague, slightly wrong explanation]
Label: A > B
```

Human raters compare two model outputs and indicate which is better.

### Instruction-Response Pairs
For instruction fine-tuning:
```
Instruction: "Write a Python function to sort a list"
Response: "def sort_list(lst): return sorted(lst)"
```

## The Labeling Pipeline

```
Raw Data → Task Design → Annotation → Quality Check → Training Data
              ↓              ↓             ↓
         Define labels   Human raters   Agreement
         Write guidelines  (or AI)     metrics,
         Create examples               review
```

### Quality Control
- **Inter-annotator agreement** — Multiple people label the same item; measure consistency
- **Gold standard items** — Known-correct examples mixed in to detect careless labelers
- **Review queues** — Expert review of a sample of annotations
- **Iterative refinement** — Update guidelines based on disagreements

## Who Does the Labeling?

### Human Annotators
- **Crowd workers** — Platforms like Scale AI, Surge AI, Labelbox
- **Domain experts** — Medical professionals for medical data, lawyers for legal data
- **Internal teams** — Dedicated annotation teams at AI labs
- **RLHF raters** — Specially trained evaluators who rank model outputs

### AI-Assisted Labeling
- **Pre-labeling** — Model generates initial labels, humans correct
- **Active learning** — Model identifies which examples would benefit most from labeling
- **[[Synthetic Data]]** — Use a teacher model to generate labeled examples
- **RLAIF** — AI generates preference judgments instead of humans

### The Human Cost
- RLHF data for frontier models involves thousands of human raters
- Often outsourced to countries with lower labor costs
- Raises ethical questions about working conditions and fair compensation
- Anthropic, OpenAI, and others have faced scrutiny over contractor treatment

## Scale of Labeling

| Model Stage | Data Needed | Labeling Effort |
|---|---|---|
| Pre-training | Trillions of tokens | Minimal (self-supervised) |
| Instruction fine-tuning | 10K–100K examples | Moderate |
| RLHF preference data | 100K–1M comparisons | Very high |
| Domain specialization | 1K–50K examples | Moderate |
| Evaluation benchmarks | 1K–10K examples | High (expert-quality) |

## Labeling for AI Coding

### Code Quality Labels
```
Code snippet → "Has bug" / "Bug-free"
Code snippet → "Security vulnerability: SQL injection"
Code change → "Improves performance" / "Degrades performance"
```

### Code Review Preferences
```
PR Review A: [thorough, catches real issues]
PR Review B: [surface-level, misses bugs]
Label: A > B
```

This preference data trains models to produce better [[AI Code Review|code reviews]].

### SWE-bench
The [[AI Benchmarks|SWE-bench]] benchmark itself is labeled data: real GitHub issues paired with verified fixes (the test suite serves as the label).

## The Trend: Less Human, More AI

The trajectory is toward reducing human labeling:
1. **Self-supervised pre-training** — No labels needed (learn from raw text)
2. **Synthetic instruction data** — AI generates training examples
3. **Constitutional AI** — Model critiques itself (less human preference data)
4. **Active learning** — Only label the most valuable examples
5. **Zero-shot / few-shot** — Skip fine-tuning, use prompting

But human labeling remains essential for:
- Aligning model values (RLHF)
- Creating evaluation benchmarks
- Handling edge cases and ambiguity
- Quality assurance

## Related

- [[Supervised vs Unsupervised Learning]]
- [[Fine-Tuning and Alignment]]
- [[Synthetic Data]]
- [[Machine Learning]]
- [[AI Benchmarks]]
- [[AI Ethics and Safety]]
