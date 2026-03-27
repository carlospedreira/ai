# GPT Evolution

The GPT (Generative Pre-trained Transformer) series from OpenAI is the most influential lineage in [[Large Language Models|LLM]] history. Each generation demonstrated that [[Scaling Laws|scaling up]] [[Causal vs Masked Language Modeling|causal language modeling]] unlocks qualitatively new capabilities, ultimately leading to ChatGPT and the AI coding revolution.

## The GPT Timeline

### GPT-1 (June 2018)
"Improving Language Understanding by Generative Pre-Training"

| Property | Value |
|---|---|
| Parameters | 117M |
| Training data | BookCorpus (~5GB) |
| Architecture | 12-layer decoder-only Transformer |
| Key contribution | Proved that unsupervised pre-training + supervised fine-tuning works |

GPT-1 showed that a generative model (predict next token) could be fine-tuned for classification, entailment, and other tasks — competing with task-specific architectures. Published months before [[BERT]], but overshadowed by BERT's stronger benchmarks.

### GPT-2 (February 2019)
"Language Models are Unsupervised Multitask Learners"

| Property | Value |
|---|---|
| Parameters | 1.5B (10× GPT-1) |
| Training data | WebText (~40GB, 8M web pages) |
| Architecture | 48-layer decoder-only Transformer |
| Key contribution | [[Zero-Shot and Few-Shot Learning\|Zero-shot]] task performance without fine-tuning |

GPT-2 demonstrated that a sufficiently large language model performs tasks it was never explicitly trained for — just from the prompt. OpenAI initially withheld the full model citing "too dangerous to release" concerns, sparking the first major debate about AI safety and open release.

Also introduced **pre-norm** residual connections ([[Normalization Techniques]]) — LayerNorm before attention/FFN — which became the standard for all subsequent Transformers.

### GPT-3 (June 2020)
"Language Models are [[Zero-Shot and Few-Shot Learning|Few-Shot Learners]]"

| Property | Value |
|---|---|
| Parameters | 175B (100× GPT-2) |
| Training data | ~300B tokens (Common Crawl, WebText, books, Wikipedia) |
| Context window | 2,048 tokens |
| Training cost | ~$5M estimated |
| Key contribution | Few-shot in-context learning at scale |

The paper that changed everything. GPT-3 showed that:
- **Few-shot learning** from examples in the prompt is a real, powerful paradigm
- **Scale alone** produces qualitatively new capabilities ([[Emergent Abilities]])
- A single model can perform hundreds of tasks via prompting
- The API model (pay-per-token) becomes viable

GPT-3 spawned the "prompt engineering" discipline and the AI startup wave.

### GPT-3.5 / InstructGPT (2022)
"Training language models to follow instructions with human feedback"

| Property | Value |
|---|---|
| Parameters | ~175B (same base as GPT-3) |
| Key innovation | [[Reinforcement Learning from Human Feedback\|RLHF]] alignment |
| Key contribution | Proved that [[Instruction Tuning]] + RLHF transforms a text predictor into a useful assistant |

InstructGPT showed that a 1.3B aligned model was preferred by humans over the 175B raw GPT-3. Alignment isn't just safety — it's usability.

**ChatGPT** (November 30, 2022) — GPT-3.5 + RLHF + chat interface. Became the fastest-growing consumer product in history (~100M users in 2 months). Launched the generative AI era.

### GPT-4 (March 2023)

| Property | Value |
|---|---|
| Parameters | Rumored ~1.8T ([[Mixture of Experts]], 8 experts, ~220B active) |
| Context window | 8K (later 32K, then 128K) |
| Modalities | Text + image input |
| Key contribution | Qualitative leap in reasoning, coding, and multimodal understanding |

GPT-4 was the first model that felt genuinely "smart" across diverse tasks:
- Passed the bar exam (90th percentile)
- Strong coding ability (led to Copilot improvements)
- [[Multimodal AI]] — could analyze images
- Sparked the [[AI Coding Harnesses]] movement

### GPT-4o (May 2024)
"Omni" — natively multimodal (text, image, audio in one model):
- Real-time voice conversation
- Faster and cheaper than GPT-4
- Available on the free tier

### o1 / o3 Series (2024–2025)
The "reasoning" models — trained with extended [[Chain of Thought]]:
- Spend variable compute on "thinking" before answering
- Dramatically better at math, science, and coding
- [[Scaling Laws#Scaling Compute at Inference|Test-time compute scaling]] — more thinking = better answers
- Powers [[OpenAI Codex]]'s cloud agent (codex-1 = fine-tuned o3)

### GPT-5 Series (2025–2026)
Current generation at the time of this vault:
- GPT-5.4 — default recommended model for Codex CLI
- GPT-5.3-Codex — specialized coding variant
- GPT-5.4-mini — cost-optimized variant

## The Scaling Story

```
Parameters (log scale):
GPT-1:    117M    ■
GPT-2:    1.5B    ■■■
GPT-3:    175B    ■■■■■■■■■■■■
GPT-4:    ~1.8T   ■■■■■■■■■■■■■■■■■■■■■■

Each 10-100× scale-up unlocked qualitatively new capabilities
```

| Model | New Capability | Prior Models Couldn't |
|---|---|---|
| GPT-2 | Zero-shot task performance | Only did what it was fine-tuned for |
| GPT-3 | Few-shot in-context learning | Needed fine-tuning for each task |
| GPT-4 | Complex reasoning, multimodal | Struggled with multi-step logic |
| o1/o3 | Extended reasoning, self-correction | Rushed to answers without thinking |

This progression is the empirical evidence behind [[The Bitter Lesson]] and [[Scaling Laws]].

## GPT's Influence

### Architectural Legacy
- **Decoder-only Transformer** became the default for all LLMs
- **Pre-norm** residual connections (from GPT-2) adopted universally
- **Autoregressive training** ([[Causal vs Masked Language Modeling|CLM]]) beat [[BERT|MLM]] for generation

### Industry Impact
- Created the API-based AI business model ($X per million tokens)
- Inspired every competitor: Claude (Anthropic), Gemini (Google), LLaMA (Meta)
- Launched the [[AI Coding Harnesses]] ecosystem (Copilot, Cursor, Codex)
- ChatGPT created the mainstream AI adoption wave

### Research Impact
- [[Scaling Laws]] (Kaplan 2020) formalized GPT scaling observations
- [[Chinchilla Scaling]] corrected the parameter-data ratio
- [[Emergent Abilities]] debate sparked by GPT-3's capabilities
- [[AI Safety and Alignment Research]] urgency increased with each generation

## Related

- [[Large Language Models]]
- [[Transformers]]
- [[Scaling Laws]]
- [[Chinchilla Scaling]]
- [[Emergent Abilities]]
- [[Reinforcement Learning from Human Feedback]]
- [[Instruction Tuning]]
- [[OpenAI Codex]]
- [[Causal vs Masked Language Modeling]]
- [[AI History and Timeline]]
- [[The Bitter Lesson]]
