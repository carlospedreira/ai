# Evaluation and Metrics

Evaluation is how we measure whether AI systems work well. The right metrics determine what gets optimized, and what gets optimized determines what gets built. This note covers evaluation across [[Machine Learning]], [[Large Language Models]], and [[AI Coding Harnesses]].

## Classification Metrics

For models that predict categories (spam/not spam, sentiment, medical diagnosis):

### Accuracy
```
Accuracy = Correct predictions / Total predictions
```
Simple but misleading when classes are imbalanced. A spam filter that always says "not spam" gets 95% accuracy if only 5% of emails are spam — but catches zero spam.

### Precision and Recall

| Metric | Question | Formula |
|---|---|---|
| **Precision** | Of items flagged positive, how many are truly positive? | TP / (TP + FP) |
| **Recall** | Of truly positive items, how many did we flag? | TP / (TP + FN) |
| **F1 Score** | Harmonic mean of precision and recall | 2 × P × R / (P + R) |

The precision-recall trade-off:
- High precision, low recall → conservative (misses real positives but rarely false-alarms)
- High recall, low precision → aggressive (catches everything but many false positives)

### Confusion Matrix
```
                    Predicted
                  Pos     Neg
Actual  Pos  [ TP     FN ]
        Neg  [ FP     TN ]
```

TP = True Positive, FP = False Positive, FN = False Negative, TN = True Negative.

## Language Model Metrics

### Perplexity
How "surprised" the model is by the test data:
```
Perplexity = 2^(-average log probability of tokens)
```
Lower = better. A perplexity of 1 means the model perfectly predicts every token. Typical LLM perplexities are 5–20 on standard benchmarks.

### BLEU (Bilingual Evaluation Understudy)
Measures n-gram overlap between generated and reference text:
- Originally for machine translation
- Scores 0–100 (higher = more overlap with reference)
- Limitation: penalizes valid paraphrases that differ from the reference

### ROUGE
Measures recall of n-grams from the reference in the generated text:
- **ROUGE-1** — Unigram overlap
- **ROUGE-2** — Bigram overlap
- **ROUGE-L** — Longest common subsequence
- Used for summarization evaluation

### Human Evaluation
For open-ended generation, automated metrics correlate poorly with quality. Human evaluation remains the gold standard:
- **Helpfulness** — Does the response address the question?
- **Accuracy** — Is the information correct?
- **Harmlessness** — Does it avoid harmful content?
- **Coherence** — Does it make sense?
- **ELO ratings** — Chatbot Arena uses competitive head-to-head comparison

## Coding-Specific Metrics

### Pass@k
The probability that at least one of k generated solutions passes all test cases:
```
pass@1 = probability the first attempt is correct
pass@10 = probability at least one of 10 attempts is correct
```
Used for [[AI Benchmarks|HumanEval]] and MBPP.

### SWE-bench Resolution Rate
Percentage of real GitHub issues autonomously resolved by the agent. See [[AI Benchmarks]].

### Code Quality Metrics
When evaluating AI-generated code beyond correctness:

| Metric | What It Measures |
|---|---|
| **Test pass rate** | Does the code actually work? |
| **Compilation rate** | Does it even compile? |
| **Lint compliance** | Does it follow style rules? |
| **Cyclomatic complexity** | Is the code unnecessarily complex? |
| **Test coverage** | Does the generated code have tests? |

## Retrieval Metrics

For [[Retrieval-Augmented Generation|RAG]] and [[Semantic Search]]:

| Metric | What It Measures |
|---|---|
| **Recall@k** | Fraction of relevant documents in top k |
| **MRR** | Position of the first relevant result |
| **NDCG** | Ranking quality, weighted by position |
| **Faithfulness** | Does the LLM answer match the retrieved context? |
| **Answer relevance** | Does the answer address the question? |

## Embedding Metrics

For [[Embeddings and Vector Search]]:
- **Cosine similarity** — Alignment between query and result vectors
- **Retrieval accuracy** — Correct matches in top k

## The Evaluation Problem for LLMs

LLMs are hard to evaluate because:

1. **Open-ended outputs** — No single "correct" answer
2. **Multi-dimensional quality** — Helpful, accurate, safe, coherent, creative, concise...
3. **Benchmark gaming** — Models optimized for benchmarks may not improve on real tasks (see [[AI Benchmarks#Limitations of Benchmarks]])
4. **Evaluator disagreement** — Humans disagree on what's "better"
5. **Contamination** — Test data may appear in training data

### LLM-as-Judge
Use a strong LLM to evaluate another LLM's output:
```
Judge prompt: "Rate this response on helpfulness (1-5) and accuracy (1-5).
              Explain your reasoning."
```

Advantages: scalable, consistent, cheap
Disadvantages: biases (prefers longer responses, self-preference), not a substitute for human evaluation on novel tasks

## A/B Testing for AI Features

In production, the definitive evaluation is A/B testing:
```
50% of users → AI feature enabled
50% of users → Control (no AI or baseline AI)

Measure: task completion rate, time, user satisfaction, error rate
```

## Related

- [[AI Benchmarks]]
- [[Machine Learning]]
- [[Large Language Models]]
- [[Retrieval-Augmented Generation]]
- [[Semantic Search]]
- [[Hallucination and Grounding]]
- [[AI Code Review]]
