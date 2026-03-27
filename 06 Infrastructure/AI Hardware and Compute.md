# AI Hardware and Compute

The hardware that trains and runs [[Large Language Models]] is a critical bottleneck in AI development. GPU access, power consumption, and chip architecture directly shape what models are possible and who can build them.

## Why Hardware Matters

- **Training frontier models** requires thousands of GPUs running for months
- **Inference** at scale requires massive GPU clusters
- **GPU scarcity** is one of the biggest constraints on AI progress
- **Energy costs** are becoming a sustainability concern
- **Hardware architecture** determines what algorithms are efficient

## Key Hardware

### NVIDIA GPUs

NVIDIA dominates AI compute with ~80% market share:

| GPU | Memory | Use Case | Notable |
|---|---|---|---|
| H100 | 80 GB HBM3 | Training + inference | Workhorse of 2023–2025 AI training |
| H200 | 141 GB HBM3e | Training + inference | 2x memory bandwidth over H100 |
| B200 (Blackwell) | 192 GB HBM3e | Training + inference | 2.5x H100 for LLM inference |
| GB200 (Grace Blackwell) | 384 GB (2x B200) | Large model training | NVLink-connected dual-GPU |
| RTX 4090 | 24 GB | Local inference, fine-tuning | Consumer GPU, strong for small models |
| RTX 5090 | 32 GB | Local inference | Latest consumer tier |

A typical frontier model training cluster: **10,000–100,000 H100s** connected via InfiniBand.

### Google TPUs

Tensor Processing Units — Google's custom AI accelerators:

| TPU | Memory | Notes |
|---|---|---|
| TPU v4 | 32 GB HBM | Used to train Gemini |
| TPU v5e | 16 GB HBM | Cost-optimized for inference |
| TPU v6 (Trillium) | 32 GB HBM | Latest generation |

TPUs are not sold — only available via Google Cloud.

### Apple Silicon

Surprisingly relevant for local AI inference:
- **Unified memory** — CPU and GPU share the same RAM pool
- **M4 Max** — 128 GB unified memory can run 70B models
- **M4 Ultra** — 192 GB, can run very large quantized models
- **Metal Performance Shaders** — GPU acceleration for inference

### Other Chips

- **AMD MI300X** — 192 GB HBM3, competing with H100
- **Intel Gaudi** — AI accelerator, lower cost tier
- **Cerebras WSE** — Wafer-scale chip, entire model fits on one chip
- **Groq LPU** — Ultra-fast inference (500+ tokens/sec), purpose-built for serving

## The Cost of AI

### Training Costs

| Model (estimated) | Training Cost | GPUs | Duration |
|---|---|---|---|
| GPT-3 (2020) | ~$5M | Unknown | Weeks |
| GPT-4 (2023) | ~$100M | ~25,000 A100s | ~3 months |
| Frontier models (2025) | $200M–$1B+ | 50,000–100,000 H100s | Months |

### Inference Costs

For API providers, inference is the ongoing cost:
- Per-token costs are driven by GPU utilization and throughput
- [[Inference Optimization]] techniques (quantization, batching, caching) reduce costs dramatically
- The gap between training cost (one-time) and inference cost (ongoing) determines pricing

### Energy

- A single H100 draws ~700W
- A 10,000-GPU cluster consumes ~7 MW
- Training a frontier model uses energy comparable to powering thousands of homes for a year
- Major AI labs are signing deals with nuclear power providers

## The GPU Supply Chain

NVIDIA GPUs are manufactured by TSMC (Taiwan):
- **Geopolitical risk** — Taiwan's semiconductor industry is strategically vital
- **Lead times** — New GPU orders take 6–12+ months to fulfill
- **Export controls** — US restricts advanced chip exports to China
- **GPU hoarding** — Cloud providers (AWS, Azure, GCP) secure multi-year allocations

This scarcity is why:
- Cloud GPU instances are expensive and often unavailable
- Model providers charge significant API fees
- [[Open Source AI Models]] running on consumer hardware (via [[Inference Optimization|quantization]]) are so valuable

## Cloud AI Compute

Major providers offer GPU instances:

| Provider | Key Offerings |
|---|---|
| **AWS** | P5 (H100), Inf2 (Inferentia), Trainium |
| **Google Cloud** | A3 (H100), TPU v5, Cloud TPU |
| **Azure** | ND H100, ND H200 |
| **CoreWeave** | GPU-specialized cloud |
| **Lambda** | GPU cloud for AI |
| **Together AI** | Inference hosting for open models |

## The Trend: More Compute, More Intelligence

[[Scaling Laws]] show that more compute → better models. This drives:
1. Bigger training clusters (approaching $10B+ investment)
2. More efficient architectures (MoE, better attention)
3. Better [[Inference Optimization]] (serve the same model for less)
4. Specialized chips (inference-optimized, training-optimized)
5. On-device AI (running smaller models on phones and laptops)

## Related

- [[Scaling Laws]]
- [[Training and Inference]]
- [[Inference Optimization]]
- [[Open Source AI Models]]
- [[Large Language Models]]
