# Inference Optimization

Inference optimization encompasses techniques that make [[Large Language Models]] faster, cheaper, and more efficient to run without (significantly) sacrificing quality. This is critical for both API providers serving millions of requests and developers running [[Open Source AI Models|open models]] locally.

## Why It Matters

Training happens once; inference happens on every request. For a model like GPT-4 or Claude:
- **Billions of requests per day** multiply any per-request inefficiency
- **Latency directly impacts UX** — users expect fast responses
- **Cost determines viability** — per-token pricing comes from inference cost
- **Hardware is scarce** — GPU capacity is the bottleneck

For [[AI Coding Harnesses]], inference optimization directly affects how fast agents can iterate through the [[Agentic Coding Paradigm#The Agent Loop|agent loop]].

## Key Techniques

### KV Caching

During autoregressive generation, the model recomputes attention for all previous tokens at each step. KV caching stores the Key and Value matrices from previous tokens, avoiding redundant computation.

- **Impact**: ~2–10x speedup for generation
- **Trade-off**: Requires GPU memory proportional to sequence length
- **Used by**: Every production LLM deployment

### Prompt Caching

When many requests share a common prefix (system prompt, instructions), cache the computed KV values for that prefix:

```
Request 1: [System prompt] + [User query A] → Cache system prompt KVs
Request 2: [System prompt] + [User query B] → Reuse cached KVs, only compute for B
```

- **Impact**: 50–90% reduction in input token processing for repeated prefixes
- **Critical for**: [[AI Coding Harnesses]] where CLAUDE.md, AGENTS.md, and tool definitions are the same every turn
- **[[OpenAI Codex]]** specifically splits prompts into stable prefix + variable suffix to maximize cache hits
- **Anthropic** offers prompt caching at ~$0.30/MTok (vs $3/MTok for full input)

### Quantization

Reduce the numerical precision of model weights:

| Precision | Bits per Weight | Memory | Quality Impact |
|---|---|---|---|
| FP32 (full) | 32 | Baseline | None |
| FP16 / BF16 | 16 | 2x smaller | Negligible |
| INT8 | 8 | 4x smaller | Minimal |
| INT4 (GPTQ/AWQ) | 4 | 8x smaller | Small, noticeable on benchmarks |
| GGUF Q4 | ~4.5 | ~7x smaller | Small, good for local inference |

A 70B parameter model at FP16 needs ~140GB of GPU memory. At INT4, it fits in ~35GB — runnable on a single high-end GPU or even a MacBook with 64GB RAM.

- **GPTQ** — Post-training quantization, widely used
- **AWQ** — Activation-aware quantization, slightly better quality
- **GGUF** — Format used by llama.cpp for CPU + GPU inference

### Speculative Decoding

Use a small, fast "draft" model to generate candidate tokens, then verify them in parallel with the large model:

```
Draft model (fast): generates 5 candidate tokens
Large model (slow): verifies all 5 in one forward pass
Accept: 4 out of 5 match → saved 3 sequential generation steps
```

- **Impact**: 2–3x speedup for generation
- **Key insight**: Verification is parallelizable, generation is sequential

### Mixture of Experts (MoE)

Instead of activating all parameters for every token, route each token to a subset of "expert" sub-networks:

```
Token → Router → Expert 2, Expert 7 (out of 16 experts)
```

- **Model**: 100B total parameters, but only 12B active per token
- **Impact**: Much faster inference than a dense model of the same quality
- **Used by**: Mixtral (8x7B), GPT-4 (rumored), LLaMA 4, DeepSeek V3

### Batching

Process multiple requests simultaneously to maximize GPU utilization:

- **Static batching** — Wait for N requests, process together
- **Continuous batching** — Add new requests to an ongoing batch dynamically
- **Impact**: 5–20x throughput improvement
- **Used by**: vLLM, TGI, and all production inference servers

### PagedAttention (vLLM)

Manage KV cache memory like virtual memory pages in an OS — allocate on demand, avoid fragmentation:

- **Impact**: 2–4x more requests served simultaneously
- **Key insight**: KV cache memory is often wasted due to pre-allocation for maximum sequence length

### Tensor Parallelism / Pipeline Parallelism

Split a single model across multiple GPUs:
- **Tensor parallelism** — Split each layer across GPUs (lower latency)
- **Pipeline parallelism** — Assign different layers to different GPUs (easier to implement)

## Local Inference Stack

For running [[Open Source AI Models]] on your own hardware:

```
Model (GGUF/GPTQ format)
        ↓
Inference Engine (llama.cpp, vLLM, TGI)
        ↓
Server (Ollama, LM Studio)
        ↓
Client ([[OpenCode]], Aider, Cline, etc.)
```

### Hardware Requirements (Approximate)

| Model Size | Quantization | Min RAM/VRAM | Suitable Hardware |
|---|---|---|---|
| 7–8B | Q4 | 6 GB | Any modern GPU, MacBook |
| 13B | Q4 | 10 GB | RTX 3060 12GB, MacBook |
| 70B | Q4 | 40 GB | 2x RTX 3090, Mac Studio 64GB |
| 405B | Q4 | 200 GB | Multi-GPU server |

Apple Silicon Macs are surprisingly good for local inference due to unified memory architecture.

## Related

- [[Training and Inference]]
- [[Large Language Models]]
- [[Open Source AI Models]]
- [[Scaling Laws]]
- [[AI Hardware and Compute]]
- [[Tokens and Tokenization]]
