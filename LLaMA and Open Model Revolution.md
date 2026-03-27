# LLaMA and Open Model Revolution

LLaMA (Large Language Model Meta AI) is Meta's family of [[Open Source AI Models]] that catalyzed an open-source revolution in AI. By releasing competitive model weights to the public, Meta fundamentally changed who can build with frontier AI — shifting power from a handful of API providers to the global developer community.

## The LLaMA Timeline

### LLaMA 1 (February 2023)

| Property | Value |
|---|---|
| Sizes | 7B, 13B, 33B, 65B parameters |
| Training data | 1.4T tokens (public datasets only) |
| License | Research-only (non-commercial) |
| Key finding | 13B matched GPT-3 (175B) on most benchmarks |

The bombshell finding: a **13B parameter model** trained on enough data could match a **175B model** trained on less data. This was [[Chinchilla Scaling]] in practice — LLaMA 1 was deliberately trained with more data per parameter than GPT-3.

**The leak:** LLaMA 1 weights were leaked on 4chan within a week of the research-only release. This accidentally created the open-source LLM movement — once the weights were public, the community ran with them.

### The Cambrian Explosion (March–June 2023)

The LLaMA leak triggered a wave of community innovation:

| Model | Innovation | Significance |
|---|---|---|
| **Alpaca** (Stanford) | [[Instruction Tuning]] with 52K GPT-3.5-generated examples | Proved you could make LLaMA follow instructions cheaply |
| **Vicuna** | Fine-tuned on ShareGPT conversations | Chat capability rivaling ChatGPT (per GPT-4 evaluation) |
| **Koala** (Berkeley) | Fine-tuned on dialogue data | Academic chat model |
| **WizardLM** | Evol-Instruct (progressively harder instructions) | Better instruction following |
| **Code Llama** (Meta) | Code-specialized LLaMA | Competitive code generation |
| **Guanaco** | [[Transfer Learning#Parameter-Efficient Fine-Tuning\|QLoRA]] fine-tuning on a single GPU | Democratized fine-tuning |

This period (March–June 2023) saw more open-source LLM innovation than the prior decade combined.

### LLaMA 2 (July 2023)

| Property | Value |
|---|---|
| Sizes | 7B, 13B, 70B parameters |
| Training data | 2T tokens |
| Context window | 4K tokens |
| License | **Commercial use allowed** (up to 700M monthly users) |
| Key innovation | Grouped Query Attention (GQA) for efficient inference |

The commercial license was transformative — startups could now build products on LLaMA 2 without asking Meta's permission. Also released **LLaMA 2 Chat** — an RLHF-aligned chat variant.

### Code Llama (August 2023)

Code-specialized variant:
- Fine-tuned on 500B+ tokens of code
- Fill-in-the-middle capability (complete code from context)
- 7B, 13B, 34B sizes
- Free for research and commercial use

### LLaMA 3 (April 2024)

| Property | Value |
|---|---|
| Sizes | 8B, 70B (initially); 405B (July 2024) |
| Training data | 15T+ tokens |
| Context window | 8K tokens (extended to 128K) |
| License | Llama 3 Community License (commercial, with conditions) |
| Key innovation | Massive data scaling, better tokenizer (128K vocab) |

LLaMA 3 represented a paradigm shift:
- The 8B model trained on **15T tokens** — over 100× the [[Chinchilla Scaling|Chinchilla-optimal]] ratio
- This "over-training" produced an 8B model that matched many 30B+ models
- The 405B model matched GPT-4 class performance on many benchmarks
- Added [[Multimodal AI|multimodal]] capabilities (vision, audio)

### LLaMA 3.2 (September 2024)

Added smaller and multimodal variants:
- 1B, 3B (small, for [[On-Device AI|mobile/edge]])
- 11B, 90B with vision capabilities
- Optimized for Qualcomm and MediaTek mobile chips

### LLaMA 4 (2025)

Introduced [[Mixture of Experts]] architecture:
- Multiple MoE variants with different expert configurations
- Better quality per active parameter
- Continued the aggressive data scaling strategy

## Why LLaMA Changed Everything

### Before LLaMA (Early 2023)
- Frontier models were proprietary (GPT-4, PaLM)
- Using AI required paying API fees to OpenAI or Google
- Fine-tuning was only available through provider APIs (limited)
- Running models locally required training from scratch (impractical for most)

### After LLaMA
- Anyone can download and run frontier-quality models
- [[On-Device AI]] became practical (quantized 7B on a laptop)
- [[Fine-Tuning and Alignment|Fine-tuning]] is accessible on consumer hardware via [[Transfer Learning#Parameter-Efficient Fine-Tuning|QLoRA]]
- Thousands of specialized models built on top of LLaMA
- [[OpenCode]] and other [[AI Coding Harnesses]] can use local models with zero API cost

### The Ecosystem LLaMA Created

```
LLaMA base model
    ↓ community fine-tunes
├── Chat models (Vicuna, WizardLM)
├── Code models (Code Llama, CodeUp)
├── Medical models (Med-LLaMA, PMC-LLaMA)
├── Legal models (LawGPT)
├── Multilingual models (Chinese-LLaMA, Japanese-LLaMA)
├── Quantized variants (GGUF for llama.cpp)
└── Thousands more on Hugging Face
```

## LLaMA's Influence on Competitors

LLaMA forced other labs to open-source or improve their offerings:

| Response | By | Action |
|---|---|---|
| Falcon | TII (UAE) | Released competitive open models |
| Mistral | Mistral AI | Founded specifically to build open models |
| Gemma | Google | Released small open models |
| Phi | Microsoft | Proved small models can be excellent with good data |
| Qwen | Alibaba | Open multilingual models |
| DeepSeek | DeepSeek | Open reasoning models rivaling o1 |
| Yi | 01.AI | Open Chinese+English models |

The competitive pressure from LLaMA raised the floor for the entire open-source AI ecosystem.

## Meta's Motivation

Why does Meta give away models that cost $100M+ to train?

1. **Ecosystem lock-in** — Developers building on LLaMA create a Meta-centric ecosystem
2. **Commoditize the complement** — If models are free, Meta's services (social media, advertising) benefit from a world where AI is ubiquitous
3. **Recruitment** — Open research attracts top talent
4. **Safety through transparency** — Open models can be audited by the community
5. **Competitive disruption** — Undermines OpenAI's and Google's paid API model

## The Licensing Debate

LLaMA's license is "open" but not fully "open source" by OSI standards:
- Commercial use allowed (with 700M MAU cap)
- Cannot use outputs to train competing models (until LLaMA 3)
- Must include Meta's attribution
- Some restrictions on use cases

See [[AI and Copyright]] and [[Open Source AI Models#Licensing Nuances]] for the broader debate.

## Related

- [[Open Source AI Models]]
- [[Large Language Models]]
- [[Chinchilla Scaling]]
- [[Scaling Laws]]
- [[Fine-Tuning and Alignment]]
- [[Transfer Learning]]
- [[On-Device AI]]
- [[Instruction Tuning]]
- [[AI History and Timeline]]
- [[GPT Evolution]]
- [[Claude History]]
