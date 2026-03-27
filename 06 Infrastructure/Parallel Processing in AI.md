# Parallel Processing in AI

Parallel processing is the backbone of modern AI — [[Training and Inference|training]] and running [[Large Language Models]] is only possible because the work is distributed across thousands of processors working simultaneously. Understanding parallelism explains why GPUs dominate AI, why [[AI Hardware and Compute|hardware]] is so critical, and why [[Transformers]] won.

## Why AI Needs Parallelism

A single forward pass through a large model involves:
- Trillions of floating-point operations
- Billions of parameters to read from memory
- Millions of attention computations

Sequential processing would take hours per query. Parallelism makes it milliseconds.

## GPU vs CPU

| Property | CPU | GPU |
|---|---|---|
| Core count | 8–128 | 10,000–16,000+ |
| Core type | Complex, general-purpose | Simple, specialized |
| Best at | Sequential, branching logic | Massively parallel, uniform operations |
| Memory bandwidth | ~100 GB/s | ~3,000 GB/s (HBM) |
| AI use case | Data loading, orchestration | Matrix multiplication, attention |

Neural network operations (matrix multiplies, activations) are **embarrassingly parallel** — the same operation applied to thousands of data points independently. GPUs excel at exactly this.

## Levels of Parallelism

### Data Parallelism

Replicate the model on multiple GPUs; each processes a different batch of data:

```
GPU 0: Model copy → Batch 0 → Gradients 0 ─┐
GPU 1: Model copy → Batch 1 → Gradients 1 ──┤→ Average → Update all copies
GPU 2: Model copy → Batch 2 → Gradients 2 ──┤
GPU 3: Model copy → Batch 3 → Gradients 3 ──┘
```

- **Scales training throughput** linearly with GPU count
- **Limitation:** Each GPU must hold the full model in memory
- Used for models that fit on a single GPU (up to ~30B parameters)

### Tensor Parallelism

Split individual layers across GPUs — each GPU computes part of every matrix multiplication:

```
Layer's weight matrix:
[A₁ | A₂ | A₃ | A₄]
GPU0  GPU1  GPU2  GPU3

Each GPU computes its slice → results combined
```

- **Enables models too large for one GPU**
- **Low latency** — GPUs communicate within each layer
- **Requires fast interconnect** (NVLink, InfiniBand)
- Typically 2–8 GPUs per tensor-parallel group

### Pipeline Parallelism

Assign different layers to different GPUs:

```
GPU 0: Layers 1-20    (stage 1)
GPU 1: Layers 21-40   (stage 2)
GPU 2: Layers 41-60   (stage 3)
GPU 3: Layers 61-80   (stage 4)
```

Data flows through GPUs sequentially, but multiple batches can be in-flight simultaneously (like a factory pipeline).

- **Simple to implement**
- **Pipeline bubbles** — Some GPUs idle while waiting for others
- **Microbatching** reduces bubble overhead

### Expert Parallelism (for MoE)

In [[Mixture of Experts]] models, different experts live on different GPUs:

```
GPU 0: Expert 0, 1, 2, 3
GPU 1: Expert 4, 5, 6, 7
GPU 2: Expert 8, 9, 10, 11
GPU 3: Expert 12, 13, 14, 15

Router sends each token to the GPU hosting its selected expert
```

### Sequence Parallelism

Split the sequence dimension across GPUs — each GPU processes a portion of the tokens:

```
Sequence: [token₁...token₅₁₂ | token₅₁₃...token₁₀₂₄]
              GPU 0                    GPU 1
```

Critical for long-context models where the [[Attention Mechanism]] produces very large intermediate tensors.

## Combined Parallelism (3D Parallelism)

Production training combines all three:

```
                 Pipeline Stages
              ┌──────┬──────┬──────┐
              │ S1   │ S2   │ S3   │
Data          ├──────┼──────┼──────┤
Replicas    → │ S1   │ S2   │ S3   │
              ├──────┼──────┼──────┤
              │ S1   │ S2   │ S3   │
              └──────┴──────┴──────┘
                Within each box:
                Tensor parallelism
                across 4-8 GPUs
```

A 10,000 GPU cluster might use:
- 8-way tensor parallelism
- 16-way pipeline parallelism
- 80-way data parallelism

## Parallelism in Inference

### Batching
Process multiple user requests simultaneously:
- **Static batching** — Fixed batch size
- **Continuous batching** — Dynamically add/remove requests (see [[Inference Optimization]])

### Speculative Decoding
See [[Inference Optimization]]. Small model generates candidates in parallel; large model verifies in one pass.

### Parallel Tool Calls
[[AI Coding Harnesses]] execute multiple tool calls simultaneously:
```
Agent requests: read_file("a.ts"), read_file("b.ts"), grep("pattern")
→ All three execute in parallel → results returned together
```

This is why modern models request multiple [[Tool Use in AI|tool calls]] in a single response.

## Why Transformers Won (Parallelism Angle)

RNNs process tokens **sequentially** — token 2 depends on token 1's output:
```
RNN: t₁ → t₂ → t₃ → t₄ (sequential, can't parallelize)
```

[[Transformers]] process all tokens **simultaneously** via attention:
```
Transformer: [t₁, t₂, t₃, t₄] → all at once (fully parallel)
```

This architectural advantage made Transformers vastly more efficient on GPU hardware, enabling the scale that produced modern [[Large Language Models]].

## Related

- [[AI Hardware and Compute]]
- [[Training and Inference]]
- [[Inference Optimization]]
- [[Transformers]]
- [[Attention Mechanism]]
- [[Mixture of Experts]]
- [[Neural Networks]]
