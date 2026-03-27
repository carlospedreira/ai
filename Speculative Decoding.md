# Speculative Decoding

Speculative decoding is an [[Inference Optimization]] technique that uses a small, fast "draft" model to generate candidate tokens, which are then verified in parallel by the large "target" model. It produces outputs identical to the target model but 2–3x faster.

## The Problem: Sequential Generation

LLMs generate tokens one at a time (autoregressively):
```
Token 1 → Token 2 → Token 3 → Token 4 → Token 5
  ~30ms     ~30ms     ~30ms     ~30ms     ~30ms = 150ms total
```

Each token requires a full forward pass through the model. The GPU is underutilized because it's doing one small computation at a time, waiting for the result before starting the next.

## The Insight

Verification is much cheaper than generation. A large model can verify N tokens in roughly the same time it takes to generate 1 token — because all N tokens can be processed in a single parallel forward pass.

## How It Works

```
Step 1: Draft model generates K candidate tokens (fast)
        "The cat sat on the" → Draft: ["warm", "sunny", "old", "red", "mat"]

Step 2: Target model verifies all K tokens in ONE forward pass (parallel)
        Token 1: "warm" ✓ (matches target distribution)
        Token 2: "sunny" ✓
        Token 3: "old" ✗ (target would have said "soft")
        → Accept "warm", "sunny"; reject from "old" onward
        → Sample correction token from target: "soft"

Step 3: Result: 3 tokens generated in ~2 forward passes instead of ~3
```

### The Key Guarantee
Speculative decoding produces **mathematically identical** output to running the target model alone. It's not an approximation — it's a speed trick that preserves exactness.

The verification step uses a modified acceptance criterion (based on the ratio of draft and target probabilities) that ensures the overall token distribution is unchanged.

## The Math

For each drafted token:
```
Accept with probability: min(1, p_target(token) / p_draft(token))
```

If the draft model's choice has high probability under the target model, it's accepted. If the draft model is overconfident about a token the target model doesn't like, it's rejected.

When a token is rejected, a correction is sampled from a modified distribution that accounts for the rejected draft.

## Draft Model Selection

The draft model must be:
- **Much faster** than the target (otherwise no speedup)
- **Reasonably aligned** with the target (otherwise most tokens get rejected)
- **Same vocabulary** (tokens must be compatible)

| Target Model | Draft Model | Typical Acceptance Rate |
|---|---|---|
| LLaMA 70B | LLaMA 8B | 70–85% |
| GPT-4 class | GPT-4-mini class | ~80% |
| Claude Opus | Claude Haiku | ~75% |

Higher acceptance rate → more tokens accepted per step → greater speedup.

## Variants

### Standard Speculative Decoding
As described above — separate draft and target models.

### Self-Speculative Decoding
Use a subset of the target model's own layers as the draft:
- Skip every other layer → fast approximate draft
- No separate model needed
- Slightly lower acceptance rate

### Medusa
Add multiple prediction heads to the target model itself:
- Each head predicts a future token position
- All heads run in one forward pass
- Verify candidates from multiple heads simultaneously
- No separate draft model needed

### Eagle / Lookahead Decoding
Predict multiple future tokens using the target model's hidden states from previous steps, then verify in parallel.

## Speedup Analysis

```
Without speculative decoding:
  5 tokens × 1 forward pass each = 5 forward passes

With speculative decoding (K=5, acceptance rate=80%):
  Draft: 5 tokens in 1 pass (cheap)
  Verify: 1 pass (accepts ~4 tokens + 1 correction)
  Result: ~5 tokens in ~2 passes = ~2.5x speedup
```

Actual speedup depends on:
- **Draft model speed** relative to target
- **Acceptance rate** — higher = more tokens per verification step
- **K (speculation length)** — more candidates = higher potential gain but more wasted work if rejected early
- **Hardware** — GPU utilization matters

Typical real-world speedup: **1.5–3x** for generation-heavy tasks.

## When It Helps Most

| Scenario | Benefit |
|---|---|
| Long text generation | High — many tokens to accelerate |
| Code generation | High — code is fairly predictable (draft model matches well) |
| Short Q&A | Low — few output tokens, overhead matters more |
| Batch inference | Low — batching already improves GPU utilization |

## Speculative Decoding in Practice

- Used internally by major API providers (Anthropic, OpenAI, Google) — users don't see it directly, just faster responses
- Available in vLLM, Hugging Face TGI, and other [[Inference Optimization]] servers
- Being explored for [[AI Coding Harnesses]] where generation speed directly impacts the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] speed

## Related

- [[Inference Optimization]]
- [[Training and Inference]]
- [[Large Language Models]]
- [[Tokens and Tokenization]]
- [[AI Hardware and Compute]]
