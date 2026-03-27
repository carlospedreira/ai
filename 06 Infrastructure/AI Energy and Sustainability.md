# AI Energy and Sustainability

The energy consumption of training and running [[Large Language Models]] has become a significant environmental and economic concern. As models grow and AI adoption accelerates, the industry faces questions about sustainability, carbon footprint, and the balance between AI capability and resource consumption.

## The Scale of AI Energy Use

### Training

| Model (Estimated) | Training Energy | CO₂ Equivalent |
|---|---|---|
| GPT-3 (2020) | ~1,300 MWh | ~500 tonnes CO₂ |
| GPT-4 (2023) | ~10,000–50,000 MWh | ~5,000–25,000 tonnes CO₂ |
| Frontier models (2025) | 50,000–200,000 MWh | Varies by grid carbon intensity |

For context:
- Average US household uses ~10.5 MWh per year
- Training GPT-3 ≈ powering ~120 homes for a year
- Training a frontier 2025 model ≈ powering ~5,000–20,000 homes for a year

### Inference

Training happens once; inference happens billions of times:

| Query Type | Energy per Query (Estimated) |
|---|---|
| Google search | ~0.3 Wh |
| ChatGPT query | ~3–10 Wh |
| Coding agent session (30 min) | ~50–200 Wh |
| Autonomous agent task (1 hour) | ~200–1,000 Wh |

An AI-powered search uses ~10–30× the energy of a traditional search. At billions of queries per day, this adds up.

### Data Center Growth

AI has driven explosive data center expansion:
- Global data center electricity consumption: ~460 TWh (2024), projected ~1,000 TWh by 2027
- AI's share is growing from ~10% to ~30%+ of data center energy
- Major AI labs (OpenAI, Google, Microsoft, Meta) are signing multi-GW power deals

## What Consumes the Energy

### GPU Power
A single [[AI Hardware and Compute|NVIDIA H100]] draws ~700W at full load:
- A 10,000-GPU training cluster: ~7 MW continuous
- A 100,000-GPU cluster: ~70 MW — equivalent to a small city
- Plus cooling, networking, storage, and overhead (PUE ~1.1–1.3)

### The Compute Hierarchy

```
Energy per operation:

Compute (GPU FLOP)  ←── Cheapest
Memory access (HBM) ←── ~10× more expensive
Data movement (NVLink) ← ~100× more expensive
Network (InfiniBand)  ← ~1000× more expensive

AI training is dominated by memory access and data movement, not raw compute
```

This is why [[Inference Optimization]] techniques that reduce memory access (Flash Attention, quantization) have outsized energy impact.

## Efficiency Improvements

### Algorithmic Efficiency

The energy per unit of AI capability has been dropping dramatically:

| Year | Model | Performance Level | Training Compute |
|---|---|---|---|
| 2020 | GPT-3 | Baseline | ~3.6×10²³ FLOPs |
| 2023 | LLaMA 2 70B | ~GPT-3 level | ~10²⁴ FLOPs (but much better training) |
| 2024 | Mistral 7B | ~GPT-3 level | ~10²² FLOPs |
| 2025 | Phi-4 3.8B | ~GPT-3 level | ~10²¹ FLOPs |

Achieving GPT-3-level performance now costs ~100× less compute than in 2020. But frontier performance keeps pushing to higher levels, consuming ever more energy.

### Hardware Efficiency

Each GPU generation improves energy efficiency:
- **A100 → H100:** ~3× more efficient per FLOP
- **H100 → B200:** ~2.5× more efficient for LLM inference
- Specialized chips (Groq LPU, Google TPU) optimize for specific workloads

### [[Inference Optimization]] Techniques

| Technique | Energy Impact |
|---|---|
| **[[Inference Optimization#Quantization\|Quantization]]** (FP16 → INT4) | ~2–4× less energy per inference |
| **[[Speculative Decoding]]** | ~2× fewer forward passes |
| **[[Prompt Caching]]** | ~90% less computation for cached prefixes |
| **[[Mixture of Experts\|MoE]]** | Only ~12% of parameters active → proportional energy savings |
| **[[Inference Optimization#Batching\|Batching]]** | Better GPU utilization → less idle power |
| **Distillation** | Smaller model, same quality → less energy ([[Knowledge Distillation]]) |

### [[On-Device AI]]

Running small models locally avoids data center energy entirely:
- A MacBook running a 7B model uses ~30W
- The same query to a cloud API uses ~3–10W at the data center *plus* network energy
- For privacy-sensitive tasks, local is both greener and more private

## Power Sources

AI companies are increasingly seeking clean energy:

| Company | Energy Strategy |
|---|---|
| **Google** | 24/7 carbon-free energy goal; large solar/wind portfolio |
| **Microsoft** | Carbon negative by 2030; nuclear power deals (Three Mile Island restart) |
| **Amazon** | Largest corporate buyer of renewable energy; nuclear investments |
| **Meta** | 100% renewable energy for operations |
| **OpenAI** | Nuclear partnerships; exploring fusion |
| **Anthropic** | Focus on efficiency; clean energy procurement |

### Nuclear Renaissance
AI's energy demands have sparked renewed interest in nuclear power:
- Small modular reactors (SMRs) designed for data centers
- Microsoft's deal to restart Three Mile Island Unit 1
- Amazon and Google investing in nuclear startups
- Nuclear provides reliable, high-density, low-carbon baseload power

## The Rebound Effect (Jevons Paradox)

As AI becomes more efficient, usage increases — often faster than efficiency improves:

```
Efficiency improves 3× → price drops 3× → usage increases 10× → total energy increases 3.3×
```

This means efficiency alone won't solve the sustainability problem if demand grows exponentially.

## What Developers Can Do

1. **Choose the right model size** — Don't use Opus when Haiku suffices (see [[Tokenomics of AI]])
2. **Use [[Prompt Caching]]** — Avoid reprocessing identical prefixes
3. **Use [[Inference Optimization|quantized]] models** when quality permits
4. **Run locally** when possible ([[On-Device AI]])
5. **Batch non-urgent work** — [[Batch Processing and Streaming|Batch API]] allows off-peak scheduling
6. **Write efficient prompts** — Concise prompts = fewer tokens = less compute (see [[Context Engineering]])
7. **Choose green providers** — Some cloud regions have lower carbon intensity

## The Trade-Off

AI provides real value — coding agents save developer hours, medical AI improves diagnoses, climate models help predict disasters. The question isn't whether to use AI, but how to do so sustainably:

```
Value delivered
─────────────── = AI sustainability ratio
Energy consumed
```

The goal is to maximize this ratio through better models, better hardware, cleaner energy, and smarter usage.

## Related

- [[AI Hardware and Compute]]
- [[Inference Optimization]]
- [[Scaling Laws]]
- [[On-Device AI]]
- [[Tokenomics of AI]]
- [[AI Ethics and Safety]]
- [[AI Regulation]]
- [[The Bitter Lesson]]
