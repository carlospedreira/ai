# Mixture of Depths

Mixture of Depths (MoD) is an efficiency technique where a [[Transformers|Transformer]] dynamically decides which tokens need full processing at each layer and which can skip it. While [[Mixture of Experts]] selects *which* parameters to use, Mixture of Depths selects *whether to compute at all* — making inference faster by doing less work on "easy" tokens.

## The Insight

Not all tokens need equal computation:
- Function words ("the", "a", "of") are contextually simple — their meaning rarely changes
- Content words ("authenticate", "deprecated", "null") need more processing
- Some sequence positions are already well-represented and gain little from another layer

Standard Transformers process every token through every layer regardless of difficulty. MoD routes easy tokens around layers via the [[Residual Connections|residual connection]]:

```
Standard Transformer:
  Token A → [Layer 1] → [Layer 2] → [Layer 3] → [Layer 4] → output
  Token B → [Layer 1] → [Layer 2] → [Layer 3] → [Layer 4] → output

Mixture of Depths:
  Token A → [Layer 1] → [SKIP]   → [Layer 3] → [SKIP]   → output
  Token B → [Layer 1] → [Layer 2] → [SKIP]   → [Layer 4] → output
                           ↑ router decides per token per layer
```

## How It Works

Each layer has a lightweight **router** that scores each token:

```
Score(token) = Linear(token_representation) → scalar

Top-k tokens (by score) → full processing through attention + FFN
Remaining tokens → skip layer (pass through via residual connection only)
```

### Capacity Ratio

The key hyperparameter: what fraction of tokens get processed at each layer.

```
Capacity = 0.5 → 50% of tokens skip each layer → ~2× faster
Capacity = 0.25 → 75% skip → ~4× faster (with quality cost)
Capacity = 1.0 → standard Transformer (no skipping)
```

The original paper (Raposo et al., 2024) showed that capacity ratios of 0.5 match standard Transformer quality while using significantly less compute.

## MoD vs MoE vs Standard

| Technique | What Varies | Per-Token Compute | Total Parameters |
|---|---|---|---|
| **Standard** | Nothing | All layers, full FFN | N params, all active |
| **[[Mixture of Experts\|MoE]]** | Which FFN expert | All layers, subset of FFN | M×N params, N active |
| **MoD** | Whether to compute at all | Subset of layers, full FFN | N params, fraction active |
| **MoE + MoD** | Both | Subset of layers, subset of FFN | M×N params, fraction of N active |

MoE and MoD are complementary — you can use both simultaneously for compounding efficiency gains.

## Why It Works

### Redundancy in Transformers

Deep Transformers have significant redundancy:
- Removing random layers from a trained model often barely affects performance
- Many layers perform near-identity transformations for most tokens
- [[Residual Connections]] mean the "default" operation is to pass input through unchanged

MoD exploits this — if a layer would have done little anyway, skip it.

### Information Density Varies

In any sequence, information is distributed unevenly:

```
"The | quick | brown | fox | jumped | over | the | lazy | dog |"
 low   mid    mid   high   high    low   low   mid  high

Processing effort should correlate with information density.
```

MoD learns to allocate compute where it matters.

## Training MoD

The router must be trained to make good skip/process decisions:

### The Capacity Constraint
During training, exactly k tokens are processed per layer (top-k by router score). This makes the compute budget deterministic — important for hardware efficiency.

### Auxiliary Loss
An additional loss term encourages balanced routing:
- Prevents the router from always selecting the same token positions
- Similar to the load-balancing loss in MoE

### End-to-End Training
The router is trained jointly with the rest of the model — it learns what "needs processing" means for each layer in the context of the full model.

## Practical Impact

### Inference Speed
```
Standard 70B model:  100 tokens/sec
MoD (capacity 0.5):  ~170 tokens/sec  (1.7× faster)
MoD (capacity 0.25): ~230 tokens/sec  (2.3× faster, slight quality drop)
```

For [[AI Coding Harnesses]], faster inference means faster [[Agentic Coding Paradigm#The Agent Loop|agent loop]] iterations — directly translating to quicker task completion.

### Memory
MoD doesn't reduce memory (all weights are still loaded), but it reduces FLOPs per token. This means:
- Same GPU can serve more concurrent requests
- Lower latency per token
- Better throughput scaling

### Combined with Other Optimizations

| Technique | Combined Effect |
|---|---|
| MoD + [[Mixture of Experts\|MoE]] | Skip layers AND use subset of parameters — extreme efficiency |
| MoD + [[Speculative Decoding]] | Draft model benefits from both speedups |
| MoD + [[Inference Optimization#Quantization\|Quantization]] | Fewer FLOPs on smaller weights |
| MoD + [[Sparse Attention]] | Skip layers AND reduce attention cost |

## Limitations

- **Training overhead** — Router adds complexity and can be unstable
- **No memory savings** — All weights still needed (unlike MoE pruning)
- **Routing accuracy** — Skipping important tokens degrades quality
- **Hardware utilization** — Variable compute per token can reduce GPU efficiency compared to uniform batches
- **Early research** — Not yet widely deployed in production models (as of 2026)

## Relation to Early Exit

A related concept: **early exit** lets tokens exit the network at different layers:

```
Token A: exits after layer 4 (confident prediction)
Token B: exits after layer 8
Token C: processes all 32 layers (difficult token)
```

MoD is per-layer selective; early exit is per-token progressive. Both reduce unnecessary computation.

## Related

- [[Mixture of Experts]]
- [[Transformers]]
- [[Residual Connections]]
- [[Inference Optimization]]
- [[Scaling Laws]]
- [[Sparse Attention]]
- [[AI Hardware and Compute]]
