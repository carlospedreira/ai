# Beam Search and Sampling

Beam search and sampling are **decoding strategies** — algorithms that determine how a [[Large Language Models|Large Language Model]] selects the next token from its predicted probability distribution. The decoding strategy profoundly affects output quality, diversity, creativity, and reliability.

## The Decoding Problem

At each generation step, the model outputs a probability distribution over its entire vocabulary (~100K tokens):

```
"The capital of France is" →
  "Paris":     0.92
  "Lyon":      0.03
  "a":         0.01
  "the":       0.008
  "not":       0.002
  ...remaining 99,995 tokens share the rest
```

How do we pick the next token from this distribution?

## Greedy Decoding

Always pick the highest-probability token:

```
Step 1: P("Paris") = 0.92 → select "Paris"
Step 2: P(",") = 0.85 → select ","
Step 3: P("which") = 0.42 → select "which"
```

- **Pros:** Fast, deterministic, simple
- **Cons:** Can get stuck in repetitive loops; misses globally better sequences where an early low-probability token leads to a better overall output
- **Used for:** Structured output, [[Function Calling]], deterministic tasks

## Beam Search

Maintain k parallel hypotheses ("beams") and expand the most promising ones:

```
Beam width = 3

Step 1:
  Beam 1: "Paris"     (score: 0.92)
  Beam 2: "Lyon"      (score: 0.03)
  Beam 3: "a"         (score: 0.01)

Step 2: Expand each beam, keep top 3 overall:
  Beam 1: "Paris,"          (score: 0.92 × 0.85 = 0.782)
  Beam 2: "Paris."          (score: 0.92 × 0.10 = 0.092)
  Beam 3: "a beautiful"     (score: 0.01 × 0.70 = 0.007)

Step 3: Continue expanding...
```

- **Pros:** Finds higher-probability sequences than greedy; good for translation, summarization
- **Cons:** Tends toward generic, safe output; computationally expensive (k× cost); poor for creative/diverse generation
- **Used for:** Machine translation, speech recognition, structured generation

## Sampling Methods

Instead of picking the "best" token, **sample** from the distribution — introducing controlled randomness.

### Temperature Sampling

Scale the logits before softmax by a temperature parameter T:

```
P_adjusted(token) = softmax(logit / T)
```

| Temperature | Effect | Use Case |
|---|---|---|
| T = 0 | Equivalent to greedy (deterministic) | Factual Q&A, code, structured output |
| T = 0.3–0.7 | Slightly diverse, mostly focused | General coding, [[AI Coding Harnesses]] default |
| T = 1.0 | Original distribution | Balanced creativity |
| T = 1.5–2.0 | Very diverse, potentially chaotic | Creative writing, brainstorming |

```
T = 0.1: "Paris" → 0.999, "Lyon" → 0.001 (almost deterministic)
T = 1.0: "Paris" → 0.920, "Lyon" → 0.030 (original)
T = 2.0: "Paris" → 0.650, "Lyon" → 0.120 (flattened, more random)
```

### Top-k Sampling

Only consider the top k most probable tokens; zero out the rest:

```
k = 3:
  "Paris": 0.92 → renormalize → 0.958
  "Lyon":  0.03 → renormalize → 0.031
  "a":     0.01 → renormalize → 0.010
  Everything else: 0
```

- Prevents sampling very unlikely tokens (reduces gibberish)
- Fixed k is a blunt instrument — sometimes 5 tokens are reasonable, sometimes 50 are

### Top-p (Nucleus) Sampling

Include tokens until their cumulative probability reaches p:

```
p = 0.95:
  "Paris": 0.92  (cumulative: 0.92) ← include
  "Lyon":  0.03  (cumulative: 0.95) ← include (reaches threshold)
  "a":     0.01  → excluded
```

- **Adaptive** — When the model is confident (one dominant token), only 1–2 tokens are considered. When uncertain, many tokens are included.
- Most commonly used sampling method alongside temperature
- Default in most API providers

### Min-p Sampling

Include tokens with probability ≥ p × max_probability:

```
min_p = 0.1, max_prob = 0.92:
  threshold = 0.1 × 0.92 = 0.092
  "Paris": 0.92  ← above threshold, include
  "Lyon":  0.03  ← below threshold, exclude
```

- Cleaner than top-p for high-confidence distributions
- Growing in popularity for local inference

## Combined Strategies

In practice, multiple strategies are combined:

```
API call:
  temperature: 0.7
  top_p: 0.95
  top_k: 50
```

Apply in order: top-k first → top-p second → temperature scaling → sample.

## Repetition Penalties

Without intervention, LLMs tend to repeat themselves. Penalties counteract this:

| Penalty | How It Works |
|---|---|
| **Frequency penalty** | Reduce probability proportional to how often a token has appeared |
| **Presence penalty** | Flat reduction for any token that has appeared at all |
| **Repetition penalty** | Divide logits of repeated tokens by a factor (> 1.0) |

## Decoding in [[AI Coding Harnesses]]

Coding agents typically use **low temperature** (0.0–0.5) for:
- Code generation (deterministic, correct)
- [[Function Calling]] (must produce valid JSON)
- [[Structured Output]] (must match schema)

And **moderate temperature** (0.5–0.8) for:
- Natural language explanations
- Creative problem-solving approaches
- [[Chain of Thought]] reasoning

Most harnesses don't expose temperature controls to the user — they use internal defaults tuned for coding tasks.

## [[Speculative Decoding]] Connection

Speculative decoding uses a small draft model to generate candidate tokens (sampled from its distribution), then the large model verifies them. The acceptance criterion ensures the final distribution matches what the large model would have produced — regardless of sampling strategy.

## Related

- [[Large Language Models]]
- [[Tokens and Tokenization]]
- [[Training and Inference]]
- [[Inference Optimization]]
- [[Speculative Decoding]]
- [[Structured Output]]
- [[Hallucination and Grounding]]
