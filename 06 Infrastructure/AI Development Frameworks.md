# AI Development Frameworks

AI development frameworks are the software libraries that researchers and engineers use to build, train, and deploy [[Neural Networks]] and [[Large Language Models]]. They provide the building blocks — tensor operations, automatic differentiation, GPU acceleration — so developers can focus on model architecture rather than low-level math.

## Major Frameworks

### PyTorch (Meta)

The dominant framework for AI research and increasingly for production.

- **Creator:** Meta AI (originally Facebook AI Research)
- **Language:** Python (C++ backend)
- **Philosophy:** Define-by-run (eager execution) — code runs like normal Python
- **Key strength:** Intuitive debugging, Pythonic API, massive ecosystem
- **Used by:** Most academic research, Anthropic, OpenAI, Meta, Hugging Face

```python
import torch
import torch.nn as nn

model = nn.Transformer(d_model=512, nhead=8, num_encoder_layers=6)
output = model(src, tgt)
loss = criterion(output, target)
loss.backward()  # automatic differentiation
optimizer.step()
```

### TensorFlow / Keras (Google)

Google's framework, once dominant, now second to PyTorch.

- **Creator:** Google Brain
- **Language:** Python (C++ backend)
- **Philosophy:** Originally define-then-run (graph execution); TF2 added eager mode
- **Key strength:** Production deployment (TensorFlow Serving, TFLite, TF.js)
- **Used by:** Google production, some enterprise deployments

### JAX (Google)

Google's newer framework focused on functional programming and hardware acceleration.

- **Creator:** Google DeepMind
- **Philosophy:** Composable function transformations (grad, jit, vmap, pmap)
- **Key strength:** Automatic vectorization, XLA compilation, TPU-native
- **Used by:** DeepMind (Gemini, AlphaFold), some research groups

```python
import jax
import jax.numpy as jnp

@jax.jit
def forward(params, x):
    return jnp.dot(x, params['W']) + params['b']

grads = jax.grad(loss_fn)(params, x, y)
```

### MLX (Apple)

Apple's framework optimized for Apple Silicon.

- **Creator:** Apple
- **Philosophy:** NumPy-like API, lazy evaluation, unified memory
- **Key strength:** Efficient on M-series chips, great for local inference
- **Used by:** On-device AI on Apple products, local LLM inference

## High-Level Libraries

Built on top of the core frameworks:

| Library | Purpose | Built On |
|---|---|---|
| **Hugging Face Transformers** | Pre-trained model hub and training pipeline | PyTorch / TF / JAX |
| **Hugging Face PEFT** | Parameter-efficient fine-tuning (LoRA, etc.) | PyTorch |
| **Lightning** | Training boilerplate reduction | PyTorch |
| **DeepSpeed** | Distributed training optimization | PyTorch |
| **FSDP** | Fully Sharded Data Parallel training | PyTorch |
| **Megatron-LM** | Large-scale model training | PyTorch |
| **vLLM** | High-throughput inference server | PyTorch |
| **llama.cpp** | CPU/GPU inference for quantized models | C++ |
| **ONNX Runtime** | Cross-framework model inference | Framework-agnostic |

## The Hugging Face Ecosystem

Hugging Face has become the de facto hub for the AI community:

- **Model Hub** — 500K+ pre-trained models (download with one line of code)
- **Datasets** — 100K+ datasets ready to use
- **Transformers** — Unified API for BERT, GPT, LLaMA, T5, etc.
- **Tokenizers** — Fast tokenization in Rust
- **Accelerate** — Simplified distributed training
- **PEFT** — LoRA, QLoRA, adapters
- **TRL** — Transformer Reinforcement Learning (RLHF/DPO training)
- **Spaces** — Free model hosting and demo deployment

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-3-8B")
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-3-8B")
```

## Framework Selection Guide

| Scenario | Recommendation |
|---|---|
| Research / prototyping | PyTorch |
| Production deployment (Google Cloud) | JAX or TensorFlow |
| On-device / mobile | TFLite, CoreML, or MLX |
| Local LLM inference | llama.cpp, MLX, or vLLM |
| Fine-tuning open models | PyTorch + Hugging Face |
| Distributed training at scale | PyTorch + DeepSpeed or FSDP |
| Apple Silicon optimization | MLX |

## The Training Stack

A complete LLM training setup involves many layers:

```
Model Code (PyTorch)
    ↓
Distributed Training (DeepSpeed / FSDP / Megatron)
    ↓
Hardware Abstraction (CUDA / ROCm / XLA / Metal)
    ↓
GPU/TPU Hardware (see [[AI Hardware and Compute]])
    ↓
Data Pipeline (Hugging Face Datasets / custom)
    ↓
Experiment Tracking (Weights & Biases / MLflow)
    ↓
Model Registry (Hugging Face Hub / internal)
```

## Related

- [[Neural Networks]]
- [[Training and Inference]]
- [[Large Language Models]]
- [[Deep Learning]]
- [[AI Hardware and Compute]]
- [[Inference Optimization]]
- [[Open Source AI Models]]
