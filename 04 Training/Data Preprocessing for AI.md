# Data Preprocessing for AI

Data preprocessing transforms raw data into a format suitable for [[Machine Learning]] and [[Large Language Models|LLM]] training. The quality of preprocessing directly determines model quality — "garbage in, garbage out" is the oldest law in ML.

## For LLM Pre-Training

### The Data Pipeline

```
Raw Web Crawl (petabytes)
    ↓ Language detection
    ↓ Deduplication
    ↓ Quality filtering
    ↓ Content filtering (toxicity, PII)
    ↓ Domain mixing
    ↓ Tokenization
Training-Ready Dataset (trillions of tokens)
```

### Deduplication

Duplicate text is surprisingly common on the internet — copy-paste, syndication, templates. Training on duplicates:
- Wastes compute on repeated information
- Causes memorization (model regurgitates exact text)
- Biases the model toward duplicated content

**Techniques:**
- **Exact dedup** — Hash-based (fast, catches copies)
- **Near-dedup** — MinHash / SimHash (catches paraphrases)
- **URL dedup** — Same URL = same content
- **Document-level vs paragraph-level** — Can dedup at different granularities

### Quality Filtering

Not all web text is useful for training:

| Filter | Removes |
|---|---|
| **Perplexity filter** | Text too random or too repetitive (scored by a small language model) |
| **Length filter** | Too short (meaningless) or too long (often auto-generated) |
| **Language filter** | Content in unwanted languages |
| **Boilerplate removal** | Navigation menus, footers, ads, cookie notices |
| **Symbol ratio** | Too many special characters (likely code dump or garbage) |
| **Repetition filter** | Text with excessive repeated phrases |

### Content Filtering

Remove harmful or problematic content:
- **Toxicity classifiers** — Hate speech, violence, explicit content
- **PII removal** — Names, emails, phone numbers, SSNs, addresses
- **Copyright-sensitive content** — Depending on [[AI and Copyright|legal strategy]]
- **CSAM detection** — Mandatory; typically uses perceptual hashing

### Domain Mixing

The ratio of different data types affects model capabilities:

| Domain | Typical Mix | Effect on Model |
|---|---|---|
| Web text | 60–70% | General knowledge, language fluency |
| Books | 5–10% | Long-form reasoning, knowledge depth |
| Code | 10–20% | Programming ability, logical reasoning |
| Academic papers | 3–5% | Scientific knowledge, formal writing |
| Conversation data | 2–5% | Dialogue ability |
| Math | 2–5% | Mathematical reasoning |

More code in the mix → better at reasoning (even on non-code tasks). This is a well-established finding.

## For Fine-Tuning

### Instruction Data Preparation

```json
{
  "instruction": "Write a function to check if a number is prime",
  "input": "",
  "output": "def is_prime(n):\n    if n < 2: return False\n    for i in range(2, int(n**0.5) + 1):\n        if n % i == 0: return False\n    return True"
}
```

Quality checklist:
- [ ] Instructions are clear and unambiguous
- [ ] Outputs are correct and well-formatted
- [ ] Diverse instruction types (not all the same pattern)
- [ ] Edge cases covered
- [ ] No contradictory examples

### Preference Data

For [[Reinforcement Learning from Human Feedback|RLHF]] / DPO:
```json
{
  "prompt": "Explain recursion",
  "chosen": "Recursion is when a function calls itself...",
  "rejected": "Recursion is a loop..."
}
```

### Data Augmentation for Fine-Tuning

- **Paraphrasing** — Rephrase instructions while keeping the same answer
- **Back-translation** — Translate to another language and back
- **Difficulty scaling** — Create easier and harder versions of the same task
- **Format variation** — Same question in different formats (Q&A, instruction, conversation)

## For Classical ML

### Numerical Data

| Step | Purpose | Example |
|---|---|---|
| **Normalization** | Scale to [0,1] | Min-max scaling |
| **Standardization** | Scale to mean=0, std=1 | Z-score |
| **Missing values** | Handle gaps | Imputation (mean, median, model-based) |
| **Outlier removal** | Remove extreme values | IQR method, Z-score threshold |
| **Feature encoding** | Convert categories to numbers | One-hot, label encoding |
| **Feature engineering** | Create informative features | Date → day_of_week, month |

### Text Data (Pre-LLM)

| Step | Purpose |
|---|---|
| **Lowercasing** | Normalize case |
| **Tokenization** | Split into words |
| **Stop word removal** | Remove "the", "is", "a" |
| **Stemming/Lemmatization** | Reduce words to root form |
| **TF-IDF** | Convert to numerical features |

These steps are largely unnecessary for LLMs, which handle raw text via [[Tokenizer Design|tokenizers]].

## Data Quality Principles

1. **More data isn't always better** — Quality > quantity ([[Scaling Laws#Chinchilla|Chinchilla]] proved this)
2. **Diversity matters** — Uniform data produces narrow models
3. **Bias auditing** — Preprocessing choices embed values (see [[AI Ethics and Safety]])
4. **Reproducibility** — Document every preprocessing step
5. **Versioning** — Track dataset versions alongside model versions

## Related

- [[Training and Inference]]
- [[Large Language Models]]
- [[Tokenizer Design]]
- [[Data Labeling and Annotation]]
- [[Synthetic Data]]
- [[Machine Learning]]
- [[Scaling Laws]]
