# Deep Learning

Deep Learning is a subset of [[Machine Learning]] that uses [[Neural Networks]] with many layers (hence "deep") to learn hierarchical representations of data.

## Why "Deep"?

Traditional ML models often require manual feature engineering. Deep learning models automatically learn increasingly abstract features through their layers:

- **Early layers** detect simple patterns (edges, phonemes, word fragments)
- **Middle layers** combine these into more complex features (shapes, syllables, phrases)
- **Later layers** represent high-level concepts (objects, meaning, intent)

## Key Architectures

| Architecture | Primary Use | Notes |
|---|---|---|
| Feedforward Networks | Tabular data | Simplest architecture |
| Convolutional Neural Networks (CNNs) | [[Computer Vision]] | Exploit spatial structure |
| Recurrent Neural Networks (RNNs) | Sequential data | Process sequences step by step |
| [[Transformers]] | Language, vision, multi-modal | Dominant modern architecture |
| GANs | [[Generative AI]] | Two networks competing |
| Autoencoders | Compression, generation | Encode then decode |
| Diffusion Models | [[Generative AI]] | Denoise to generate images |

## What Enabled the Deep Learning Revolution

- **Data** — The internet produced massive datasets
- **Compute** — GPUs and later TPUs made training feasible
- **Algorithms** — Backpropagation, batch normalization, attention mechanisms
- **Frameworks** — PyTorch, TensorFlow made experimentation accessible

## Breakthroughs

- **2012** — AlexNet wins ImageNet, sparking the deep learning era
- **2014** — GANs introduced by Ian Goodfellow
- **2017** — [[Transformers]] introduced in "Attention Is All You Need"
- **2018–2020** — BERT, GPT-2, GPT-3 demonstrate [[Large Language Models|LLM]] capabilities
- **2022–present** — ChatGPT, diffusion models, and multimodal AI reach mainstream

## Related

- [[Neural Networks]]
- [[Transformers]]
- [[Generative AI]]
