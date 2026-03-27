# Model Serving Infrastructure

Model serving infrastructure is the stack of systems that make [[Large Language Models]] available for real-time inference at scale. It bridges the gap between a trained model and a production API endpoint — handling load balancing, batching, scaling, caching, and cost optimization.

## The Serving Stack

```
Client Request (API call)
        ↓
┌──────────────────────┐
│    API Gateway        │  (auth, rate limiting, routing)
├──────────────────────┤
│    Load Balancer      │  (distribute across replicas)
├──────────────────────┤
│    Request Queue      │  (buffer during spikes)
├──────────────────────┤
│    Inference Server   │  (vLLM, TGI, TensorRT-LLM)
│    ┌────────────────┐ │
│    │ Model on GPU   │ │  (weights loaded, KV cache allocated)
│    └────────────────┘ │
├──────────────────────┤
│    Response Streaming │  ([[Batch Processing and Streaming|SSE]])
└──────────────────────┘
        ↓
Client receives tokens
```

## Inference Servers

The inference server is the core component — it loads the model, processes requests, and generates responses.

| Server | By | Language | Key Feature |
|---|---|---|---|
| **vLLM** | UC Berkeley | Python | PagedAttention, continuous batching, highest throughput |
| **TGI** (Text Generation Inference) | Hugging Face | Rust + Python | Production-ready, Hugging Face integration |
| **TensorRT-LLM** | NVIDIA | C++ | Maximum hardware utilization on NVIDIA GPUs |
| **llama.cpp** | Community | C++ | CPU + GPU, quantized models, local inference |
| **Ollama** | Community | Go | Simple UX layer on top of llama.cpp |
| **SGLang** | LMSYS | Python | Structured generation, RadixAttention |
| **MLC LLM** | Community | C++ | Cross-platform (mobile, web, edge) |

### vLLM Deep Dive

vLLM is the most widely used open-source inference server:

**PagedAttention:** Manages KV cache like virtual memory — allocates memory in pages, avoids fragmentation, enables 2–4× more concurrent requests than naive allocation.

**Continuous batching:** Instead of waiting to fill a batch:
```
Traditional: Wait for 8 requests → process batch → return all 8
Continuous:  Start request 1 → request 2 arrives → add to batch dynamically
             → request 1 finishes → slot freed → request 3 fills it
```

Result: near-optimal GPU utilization at all times.

**Key metrics:**
- Throughput: 10–24× higher than naive HuggingFace serving
- Supports: LLaMA, Mistral, Qwen, Falcon, MPT, and most open models
- Features: [[Speculative Decoding]], prefix caching, tensor parallelism, quantization

## Key Infrastructure Concerns

### Autoscaling

GPU instances are expensive (~$2–8/hour per GPU). Scale up for demand spikes, scale down during quiet periods:

```
Requests/sec:    Low          Peak         Low
GPU instances:   2            16           2
Cost:            $4/hr        $32/hr       $4/hr
```

Challenge: GPU instances take 2–5 minutes to start (loading model weights into VRAM). Solutions:
- **Warm pool** — Keep spare instances loaded but idle
- **Predictive scaling** — Forecast demand from historical patterns
- **Serverless GPU** — Providers like Modal, Replicate, RunPod handle scaling

### [[Prompt Caching]]

At the infrastructure level, prompt caching stores computed KV states for common prefixes:
- **Provider-managed** (Anthropic, OpenAI) — Automatic, transparent to the user
- **Self-hosted** — RadixAttention (SGLang), prefix caching (vLLM) — manual configuration

### Multi-Model Routing

Route requests to different models based on complexity or cost:

```
Simple question → Haiku (fast, cheap)
Code generation → Sonnet (balanced)
Complex architecture → Opus (powerful, expensive)
```

This is the pattern behind [[Claude Code]]'s `opusplan` mode and [[AI Agents in Production#The Budget Envelope Pattern|budget-aware agent design]].

### GPU Memory Management

A single GPU must hold:
- **Model weights** — 7B model at FP16 = 14GB
- **KV cache** — Grows with sequence length × batch size
- **Activation memory** — For the current forward pass

For a 70B model serving 32 concurrent requests with 4K context each:
```
Weights:   ~140 GB (FP16) → 4× A100 80GB with tensor parallelism
KV cache:  ~2 GB per request × 32 = 64 GB
Total:     ~200+ GB → needs multi-GPU setup
```

[[Inference Optimization#Quantization|Quantization]] (INT4/INT8) reduces weight memory 2–4×, enabling larger batches or fewer GPUs.

## Deployment Patterns

### Self-Hosted
Run your own inference infrastructure:
```
GPU cluster → vLLM/TGI → API endpoint
```
- Full control, lowest per-token cost at scale
- Requires ML infrastructure team
- Best for: high-volume applications, privacy requirements

### Managed API
Use a provider's hosted models:
```
Your app → Anthropic API / OpenAI API → Response
```
- Zero infrastructure management
- Pay per token
- Best for: most applications, getting started

### Hybrid
Use managed APIs for frontier models, self-host for high-volume or sensitive workloads:
```
Complex tasks → Claude Opus API ($5/$25 per MTok)
Simple tasks → Self-hosted Mistral 7B (pennies per MTok)
```

### Serverless GPU
Pay per second of GPU time, no instance management:
- **Modal** — Python-native, auto-scaling
- **Replicate** — Model hosting marketplace
- **RunPod** — GPU-as-a-service
- **Together AI** — Inference hosting for open models

## Monitoring

See [[AI Observability]] for the full framework. Key serving metrics:

| Metric | Target | Alert If |
|---|---|---|
| **TTFT** (Time to First Token) | < 500ms | > 2s |
| **TPOT** (Time Per Output Token) | < 50ms | > 100ms |
| **Throughput** (tokens/sec) | Maximize | < expected baseline |
| **GPU utilization** | > 70% | < 40% (wasteful) or > 95% (saturated) |
| **Queue depth** | Near 0 | Growing (capacity issue) |
| **KV cache utilization** | < 80% | > 90% (may OOM) |
| **Error rate** | < 0.1% | > 1% |

## Cost Optimization

| Strategy | Savings |
|---|---|
| **Quantization** (FP16 → INT4) | 2–4× fewer GPUs |
| **Continuous batching** | 2–4× more throughput per GPU |
| **Prompt caching** | 50–90% less compute for repeated prefixes |
| **Model routing** | Use cheap models for easy tasks |
| **Spot/preemptible instances** | 50–70% cheaper (with restart risk) |
| **Off-peak scheduling** | Lower demand = lower spot prices |

## Related

- [[Inference Optimization]]
- [[AI Hardware and Compute]]
- [[Prompt Caching]]
- [[Batch Processing and Streaming]]
- [[Speculative Decoding]]
- [[AI Agents in Production]]
- [[AI Observability]]
- [[Tokenomics of AI]]
- [[Open Source AI Models]]
