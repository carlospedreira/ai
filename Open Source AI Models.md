# Open Source AI Models

Open-source AI models are [[Large Language Models]] whose weights (and often training code and data) are publicly released, allowing anyone to download, run, fine-tune, and deploy them without depending on a proprietary API. They have become a critical counterbalance to closed models from OpenAI, Anthropic, and Google.

## Why Open Source Matters

- **Privacy** — Run models locally; data never leaves your machine
- **Cost** — No per-token API fees; only hardware costs
- **Customization** — [[Fine-Tuning and Alignment|Fine-tune]] on your own data
- **No vendor lock-in** — Switch between models freely
- **Research** — Inspect weights, study behavior, reproduce results
- **Offline use** — Works without internet access

## Major Open-Source Model Families

### Meta — LLaMA

| Model | Parameters | Release | Notes |
|---|---|---|---|
| LLaMA 2 | 7B–70B | Jul 2023 | First widely-adopted open model |
| LLaMA 3 | 8B–70B | Apr 2024 | Competitive with GPT-3.5 |
| LLaMA 3.1 | 8B–405B | Jul 2024 | 405B matched GPT-4 class |
| LLaMA 3.2 | 1B–90B | Sep 2024 | Added vision (multimodal) |
| LLaMA 4 | Various | 2025 | Mixture of Experts architecture |

Meta's LLaMA family catalyzed the open-source AI movement. Licensed for commercial use (with restrictions above 700M monthly users).

### Mistral AI

| Model | Parameters | Notes |
|---|---|---|
| Mistral 7B | 7B | Punched above its weight at launch |
| Mixtral 8x7B | ~12B active (47B total) | Mixture of Experts; competitive with LLaMA 70B |
| Mistral Large | Undisclosed | Commercial tier, competitive with GPT-4 |
| Codestral | 22B | Code-specialized |

French AI lab. Pioneered efficient architectures (MoE, sliding window attention).

### Alibaba — Qwen

| Model | Notes |
|---|---|
| Qwen 2.5 | Strong multilingual, especially Chinese + English |
| Qwen 2.5 Coder | Code-specialized, competitive with larger models |
| QwQ | Reasoning-focused model |

### DeepSeek

| Model | Notes |
|---|---|
| DeepSeek V2/V3 | Efficient MoE architecture |
| DeepSeek R1 | Reasoning model rivaling o1 |
| DeepSeek Coder | Code-specialized |

Chinese lab that gained attention for matching frontier performance at lower costs.

### Google — Gemma

| Model | Parameters | Notes |
|---|---|
| Gemma 2 | 2B–27B | Lightweight, derived from Gemini research |

### Others

- **Phi** (Microsoft) — Small but capable models (Phi-3, Phi-4)
- **Command R** (Cohere) — RAG-optimized
- **StarCoder** (BigCode/HuggingFace) — Code-specialized
- **Falcon** (TII) — Early open model, 40B–180B

## Running Open Models Locally

### Inference Servers

| Tool | What It Does |
|---|---|
| **Ollama** | Simplest way to run models locally. `ollama pull llama3` and go. |
| **LM Studio** | GUI for downloading and running models, OpenAI-compatible API |
| **llama.cpp** | C++ inference engine for running models on CPU and GPU |
| **vLLM** | High-throughput inference server for production |
| **TGI** (HuggingFace) | Text Generation Inference server |

### With Coding Agents

[[OpenCode]] is the premier choice for using open models with an [[AI Coding Harnesses|AI coding agent]] — it supports Ollama, LM Studio, and any OpenAI-compatible endpoint natively. [[AI Coding Landscape|Aider]], Cline, Roo Code, and Continue.dev also support local models.

```bash
# Run a model locally with Ollama
ollama pull llama3
# Point OpenCode at it
# (auto-detects at http://localhost:11434/v1)
```

## Open vs Closed: Trade-offs

| Aspect | Open Source | Closed (API) |
|---|---|---|
| Quality (frontier) | Competitive but trailing | Slightly ahead |
| Privacy | Full control | Trust the provider |
| Cost (high volume) | Much cheaper | Expensive at scale |
| Cost (low volume) | Hardware investment | Pay-per-use |
| Customization | Full fine-tuning | Limited or no fine-tuning |
| Speed to new capabilities | Lags 3–6 months | First to market |
| Support | Community | Commercial SLA |

## The Convergence

The gap between open and closed models has narrowed dramatically:
- LLaMA 3.1 405B matched GPT-4 class performance
- DeepSeek R1 rivaled o1 in reasoning benchmarks
- Qwen 2.5 Coder competes with proprietary coding models
- Quantized models (see [[Inference Optimization]]) run on consumer hardware

## Licensing Nuances

Not all "open" models are truly open source:
- **LLaMA** — Custom license, commercial use allowed with restrictions
- **Mistral** — Apache 2.0 (truly open)
- **Qwen** — Apache 2.0 or custom depending on variant
- **Gemma** — Google's custom terms of use

The **Open Source Initiative (OSI)** has debated what "open source" means for AI, as many models release weights but not training data or code.

## Related

- [[Large Language Models]]
- [[Fine-Tuning and Alignment]]
- [[Inference Optimization]]
- [[OpenCode]]
- [[AI Coding Landscape]]
- [[AI Hardware and Compute]]
