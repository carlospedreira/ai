# On-Device AI

On-device AI runs [[Large Language Models|language models]] and other AI models directly on local hardware — phones, laptops, edge devices — without sending data to cloud APIs. It combines [[Inference Optimization]] techniques with increasingly capable consumer hardware to deliver AI that is private, offline-capable, and latency-free.

## Why On-Device?

| Benefit | Detail |
|---|---|
| **Privacy** | Data never leaves the device — critical for medical, legal, personal data |
| **Latency** | No network round-trip; response starts in milliseconds |
| **Offline** | Works without internet (planes, remote areas, air-gapped networks) |
| **Cost** | No per-token API fees; only hardware cost |
| **Sovereignty** | Comply with data residency regulations without cloud dependency |

## What's Possible (2026)

### On Phones

| Device | RAM | Capable Of |
|---|---|---|
| iPhone 16 Pro | 8 GB | 1–3B parameter models (summarization, classification, simple chat) |
| Samsung Galaxy S25 | 12 GB | 3–7B models with aggressive quantization |
| Pixel 9 Pro | 16 GB | 7B models via Gemini Nano |

Apple Intelligence runs on-device models for writing tools, notification summaries, and Siri. Google's Gemini Nano runs locally on Pixel phones.

### On Laptops

| Hardware | RAM | What It Runs |
|---|---|---|
| MacBook Air M4 | 16–32 GB | 7–13B models comfortably; 30B with quantization |
| MacBook Pro M4 Max | 64–128 GB | 70B models; even 100B+ with heavy quantization |
| Mac Studio M4 Ultra | 192 GB | 405B models (quantized) |
| Gaming PC (RTX 4090) | 24 GB VRAM | 13B models at full speed; 70B split CPU/GPU |

Apple Silicon's **unified memory** is a game-changer — CPU and GPU share the same RAM pool, so a 64 GB MacBook can load models that would require a $10K GPU on x86.

### On Edge Devices

| Device | Use Case |
|---|---|
| NVIDIA Jetson | Robotics, industrial AI |
| Raspberry Pi 5 + NPU | Tiny models for IoT |
| Smart speakers | On-device wake word + simple commands |
| Cars | ADAS, voice assistants |

## The Software Stack

```
Model (GGUF, CoreML, ONNX format)
         ↓
Inference Engine (llama.cpp, MLX, CoreML, TFLite)
         ↓
Serving Layer (Ollama, LM Studio)
         ↓
Application (chat UI, coding agent, API)
```

### Key Tools

| Tool | Platform | What It Does |
|---|---|---|
| **Ollama** | macOS, Linux, Windows | One-command model download and serving. `ollama run llama3` |
| **LM Studio** | macOS, Linux, Windows | GUI for browsing, downloading, and chatting with local models |
| **llama.cpp** | Cross-platform | C++ inference engine; GGUF format; CPU + GPU |
| **MLX** | macOS (Apple Silicon) | Apple's framework optimized for unified memory |
| **CoreML** | Apple devices | Apple's production inference framework |
| **TensorFlow Lite** | Mobile, embedded | Google's mobile/edge inference framework |
| **ONNX Runtime** | Cross-platform | Microsoft's cross-framework inference |
| **MLC LLM** | Cross-platform | Machine Learning Compilation for universal deployment |

### Quantization Is Key

See [[Inference Optimization#Quantization]]. Reducing weight precision makes large models fit on small devices:

| Original | Quantized | Size | Quality Loss |
|---|---|---|---|
| LLaMA 3 8B (FP16) | 16 GB | — | — |
| LLaMA 3 8B (Q4_K_M) | ~5 GB | 3.2x smaller | Minimal |
| LLaMA 3 70B (FP16) | 140 GB | — | — |
| LLaMA 3 70B (Q4_K_M) | ~40 GB | 3.5x smaller | Small |

Q4 quantization (4-bit) is the sweet spot — major size reduction with minimal quality degradation.

## On-Device AI + [[AI Coding Harnesses]]

### Local Models with OpenCode

[[OpenCode]] natively supports local models:

```bash
# Start a local model
ollama pull deepseek-coder-v2:16b

# OpenCode auto-detects Ollama
opencode
# Select model: ollama/deepseek-coder-v2:16b
```

No API keys needed. No data leaves your machine. Completely free after the one-time hardware cost.

### Local Models with Other Tools

| Tool | Local Model Support |
|---|---|
| [[OpenCode]] | Native (Ollama, LM Studio, any OpenAI-compatible endpoint) |
| [[AI Coding Landscape\|Aider]] | Yes (Ollama, LM Studio) |
| [[AI Coding Landscape\|Continue.dev]] | Yes (Ollama, LM Studio, llama.cpp) |
| [[AI Coding Landscape\|Cline]] | Yes (via OpenAI-compatible API) |
| [[Claude Code]] | No (Claude models only) |
| [[OpenAI Codex]] | No (OpenAI models only) |

### Quality Trade-Off

Local models (7B–13B) vs API models (Claude Opus, GPT-5):

| Aspect | Local 13B | Cloud Frontier |
|---|---|---|
| Simple code completion | Good | Excellent |
| Multi-file refactoring | Weak | Strong |
| Complex reasoning | Limited | Strong |
| Speed (tokens/sec) | 20–60 t/s | 50–100 t/s |
| Cost per session | $0 | $1–$50 |
| Privacy | Total | Trust provider |

Best practice: use local models for routine tasks and privacy-sensitive work; switch to cloud for complex reasoning.

## Apple Intelligence

Apple's on-device AI strategy (2024+):
- **Foundation models** run on-device for writing tools, summarization, notification triage
- **Adapter system** — Small task-specific adapters loaded on top of a shared base model
- **Private Cloud Compute** — For tasks too complex for on-device, runs on Apple's secure cloud (never stored, independently auditable)
- **CoreML + MLX** — The inference stack

## The Trend

On-device AI is improving along three axes:
1. **Hardware** — More RAM, faster NPUs, unified memory architectures
2. **Models** — Better small models (Phi-4, Gemma 2, Qwen 2.5) that punch above their weight
3. **Optimization** — Better quantization, [[Speculative Decoding]], [[Mixture of Experts|sparse models]]

The convergence point: within 2–3 years, a laptop may run models competitive with today's cloud APIs entirely locally.

## Related

- [[Inference Optimization]]
- [[Open Source AI Models]]
- [[AI Hardware and Compute]]
- [[OpenCode]]
- [[AI Coding Landscape]]
- [[Tokens and Tokenization]]
- [[AI and Developer Productivity]]
