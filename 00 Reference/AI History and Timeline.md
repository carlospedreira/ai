# AI History and Timeline

A chronological overview of the major milestones in [[Artificial Intelligence]], from foundational theory to the agentic coding era.

## The Foundations (1940s–1960s)

| Year | Event |
|---|---|
| 1943 | McCulloch & Pitts propose a mathematical model of the neuron |
| 1950 | Turing publishes "Computing Machinery and Intelligence" (the Turing Test) |
| 1956 | **Dartmouth Conference** — the field of "Artificial Intelligence" is named |
| 1957 | Rosenblatt invents the Perceptron (first [[Neural Networks\|neural network]] implementation) |
| 1958 | McCarthy develops Lisp, the language of early AI |

## First AI Winter (1970s)

| Year | Event |
|---|---|
| 1969 | Minsky & Papert publish *Perceptrons*, showing single-layer limitations |
| 1970s | Funding cuts as early AI fails to deliver on grand promises |

## Expert Systems Era (1980s)

| Year | Event |
|---|---|
| 1980 | Expert systems boom — rule-based AI for specific domains |
| 1986 | **Rumelhart, Hinton & Williams** popularize [[Backpropagation]] for neural networks |
| 1989 | LeCun applies backprop to [[Convolutional Neural Networks\|CNNs]] for digit recognition |
| Late 1980s | Second AI winter as expert systems prove brittle and expensive |

## The Machine Learning Era (1990s–2000s)

| Year | Event |
|---|---|
| 1997 | **Deep Blue** beats Garry Kasparov at chess |
| 1997 | Hochreiter & Schmidhuber invent [[Recurrent Neural Networks\|LSTM]] |
| 1998 | LeCun's LeNet-5 demonstrates practical [[Convolutional Neural Networks\|CNN]] for handwriting |
| 2006 | Hinton's Deep Belief Networks — "deep learning" term coined |

## The Deep Learning Revolution (2012–2017)

| Year | Event |
|---|---|
| 2012 | **AlexNet** wins ImageNet by a massive margin → [[Deep Learning]] revolution begins |
| 2013 | **Word2Vec** — [[Word Embeddings]] capture semantic meaning |
| 2014 | **GANs** introduced by Ian Goodfellow ([[GANs]]) |
| 2014 | Bahdanau attention mechanism for machine translation |
| 2015 | **ResNet** — [[Residual Connections]] enable 152-layer networks |
| 2015 | **Batch normalization** — [[Normalization Techniques]] stabilize training |
| 2016 | **AlphaGo** defeats world Go champion Lee Sedol |
| 2017 | **"Attention Is All You Need"** — the [[Attention Is All You Need\|Transformer paper]] |

## The Transformer and LLM Era (2018–2022)

| Year | Event |
|---|---|
| 2018 | **BERT** — bidirectional Transformer, revolutionizes NLP benchmarks |
| 2018 | **GPT-1** — first decoder-only Transformer language model (OpenAI) |
| 2019 | **GPT-2** — "too dangerous to release" (1.5B params) |
| 2019 | Rich Sutton publishes "[[The Bitter Lesson]]" |
| 2020 | **GPT-3** (175B params) — [[Zero-Shot and Few-Shot Learning\|few-shot learning]] breakthrough |
| 2020 | **[[Scaling Laws]]** formalized by Kaplan et al. |
| 2020 | **AlphaFold 2** solves protein structure prediction ([[AI for Science]]) |
| 2021 | **GitHub Copilot** launches — AI autocomplete for code |
| 2021 | **DALL-E** — text-to-image with Transformers |
| 2022 | **[[Chinchilla Scaling]]** — compute-optimal training ratios |
| 2022 | **Stable Diffusion** — open-source image generation ([[Diffusion Models]]) |
| 2022 | **ChatGPT** launches (Nov 30) — fastest-growing consumer product in history |

## The Generative AI Explosion (2023)

| Month | Event |
|---|---|
| Feb | Meta releases **LLaMA** — catalyzes [[Open Source AI Models]] movement |
| Mar | **GPT-4** launches — multimodal, dramatic quality jump |
| Mar | **Claude** (Anthropic) launches to the public |
| Mar | **Devin** demonstrated (first autonomous coding agent concept) |
| Apr | **Auto-GPT** goes viral — autonomous agent hype wave |
| Jul | **Claude 2** with 100K token context |
| Jul | **LLaMA 2** — open-source with commercial license |
| Nov | **Anthropic introduces [[Model Context Protocol]]** (MCP) |
| Dec | **Gemini** (Google) launches — multimodal from the ground up |
| Dec | **Mixtral 8x7B** — [[Mixture of Experts]] goes mainstream |

## The Agentic Era (2024–2025)

| Month | Event |
|---|---|
| 2024 Feb | Claude 3 family (Opus, Sonnet, Haiku) |
| 2024 Mar | **Devin** launches — first commercial autonomous coding agent |
| 2024 Jun | Claude 3.5 Sonnet — coding benchmark leader |
| 2024 Sep | **OpenAI o1** — reasoning model with chain-of-thought |
| 2024 Oct | Nobel Prize for Chemistry to AlphaFold (Hassabis, Jumper) |
| 2024 Nov | **Mamba** / [[State Space Models]] gain traction |
| 2024 Dec | Claude 3.5 Sonnet updated, coding capabilities surge |
| 2025 Jan | **ACP** announced by JetBrains and Zed ([[Agent Client Protocol]]) |
| 2025 Feb | **[[Claude Code]]** launches (research preview) |
| 2025 Apr | **[[OpenAI Codex]]** CLI launches (open source, Apache 2.0) |
| 2025 May | Claude Code GA; Claude 4 family |
| 2025 May | **Codex Cloud** launches in ChatGPT |
| 2025 Jun | **[[OpenCode]]** launches by SST team |
| 2025 Jun | Codex CLI rewritten in Rust |
| 2025 Jun | **Gemini CLI** launches (open source) |

## The Present (2025–2026)

| Month | Event |
|---|---|
| 2025 Dec | OpenCode v1.0 rewrite; 50K+ GitHub stars |
| 2026 Jan | Anthropic blocks third-party OAuth → OpenCode gains 18K stars in 2 weeks |
| 2026 Jan | [[Agent Client Protocol\|ACP]] adoption grows across editors |
| 2026 Feb | **Claude Opus 4.6** — agent teams, 1M context |
| 2026 Mar | **Codex Security** — specialized security scanning agent |
| 2026 Mar | OpenCode at 131K stars, 650K+ monthly active developers |
| 2026 Mar | Anthropic reports AI integrated into 60% of developer work |
| 2026 Mar | **This vault:** 112 interconnected notes covering the full AI landscape |

## Patterns Across History

### The AI Winter Cycle
```
Hype → Overpromise → Underdeliver → Funding cuts → Winter → New breakthrough → Repeat
```

Each cycle raises the floor — the "winter" of 2025 would still have tools that seemed like science fiction in 2020.

### [[The Bitter Lesson]] Repeating
At every era, the winning approach was the one that scaled compute:
- Expert systems (knowledge-heavy) lost to ML (compute-heavy)
- Hand-crafted NLP lost to neural NLP
- RNNs lost to Transformers (more parallelizable = more compute-efficient)
- Specialized models lost to large general models

### The Democratization Arc
```
1950s: A few labs with custom hardware
1980s: Universities with workstations
2012:  Researchers with GPUs
2020:  Anyone with a cloud account
2023:  Anyone with a laptop (open source models)
2025:  Anyone with a phone (on-device AI)
```

## Related

- [[Artificial Intelligence]]
- [[Deep Learning]]
- [[Transformers]]
- [[Large Language Models]]
- [[Attention Is All You Need]]
- [[The Bitter Lesson]]
- [[Scaling Laws]]
- [[AI Coding Harnesses]]
- [[Agentic Coding Paradigm]]
