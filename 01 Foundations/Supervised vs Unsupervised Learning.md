# Supervised vs Unsupervised Learning

These are the two foundational learning paradigms in [[Machine Learning]]. They differ in whether the training data includes labels (correct answers).

## Supervised Learning

The model learns from **labeled** examples — each input is paired with the correct output.

### How It Works

```
Input: photo of a cat → Label: "cat"
Input: photo of a dog → Label: "dog"
```

The model learns to map inputs to outputs, then generalizes to new, unseen inputs.

### Tasks

- **Classification** — Assign a category (spam vs. not spam, disease vs. healthy)
- **Regression** — Predict a continuous value (house price, temperature)

### Common Algorithms

- Linear / logistic regression
- Decision trees and random forests
- Support vector machines (SVMs)
- [[Neural Networks]]

### Strengths and Weaknesses

- Produces highly accurate models when good labels exist
- Labeling data is expensive and time-consuming
- Limited by the quality and biases in the labeled data

## Unsupervised Learning

The model finds patterns in **unlabeled** data — no correct answers are provided.

### How It Works

The model discovers structure on its own: groups, patterns, or compressed representations.

### Tasks

- **Clustering** — Group similar items (customer segments, document topics)
- **Dimensionality reduction** — Compress data while preserving structure (PCA, t-SNE)
- **Anomaly detection** — Identify unusual data points
- **Association** — Find relationships between items (market basket analysis)

### Common Algorithms

- K-Means clustering
- DBSCAN
- Principal Component Analysis (PCA)
- Autoencoders

## Self-Supervised Learning

A hybrid approach where the model **generates its own labels** from the data. This is the dominant paradigm for [[Large Language Models]]:

- Mask a word in a sentence → predict the masked word (BERT)
- Given preceding text → predict the next token (GPT, Claude)

Self-supervised learning enables training on massive unlabeled datasets, which is what makes modern LLMs possible.

## Comparison

| Aspect | Supervised | Unsupervised | Self-Supervised |
|---|---|---|---|
| Labels | Required | None | Auto-generated |
| Goal | Predict known output | Discover structure | Learn representations |
| Data cost | High (labeling) | Low | Low |
| Use in LLMs | Fine-tuning | Rarely | Pre-training |

## Related

- [[Machine Learning]]
- [[Reinforcement Learning]]
- [[Large Language Models]]
