# Reward Modeling

Reward modeling is the process of training a model to predict what humans prefer — it is the bridge between raw human judgments and the [[Reinforcement Learning]] signal used to align [[Large Language Models]]. The reward model (RM) is the most critical and fragile component in [[Reinforcement Learning from Human Feedback|RLHF]].

## Why Reward Models?

Directly optimizing for human preferences is impossible at scale — you can't have a human rate every output during training. Instead:

```
Human preferences (thousands of comparisons)
            ↓ train
Reward Model (predicts preference automatically)
            ↓ used by
RL optimizer (fine-tunes LLM to maximize predicted reward)
```

The reward model is a learned proxy for human judgment.

## How Reward Models Work

### Training Data

Pairs of (prompt, response_A, response_B, preference):
```
Prompt: "Explain quantum entanglement"
Response A: [clear, accurate, well-structured explanation]
Response B: [vague, slightly incorrect, overly long]
Preference: A > B
```

Thousands to hundreds of thousands of such pairs are collected from human annotators (see [[Data Labeling and Annotation]]).

### Architecture

The reward model is typically a language model (often the same architecture as the model being aligned) with a scalar output head:

```
(prompt, response) → LLM backbone → [linear layer] → scalar score
```

The score represents "how much a human would prefer this response."

### Training Objective

The Bradley-Terry model — train the RM so that preferred responses get higher scores:

```
Loss = -log(σ(r(chosen) - r(rejected)))
```

Where σ is sigmoid, r() is the reward model's score. This pushes the model to assign higher scores to human-preferred responses.

## Reward Model Quality

The RM's quality is the ceiling for alignment — the LLM can only be as well-aligned as the reward signal allows.

### What Makes a Good RM
- **Consistency** — Similar responses get similar scores
- **Calibration** — Score differences reflect strength of preference
- **Generalization** — Accurate on prompts not seen during training
- **Robustness** — Not easily gamed by the policy model

### What Goes Wrong

| Failure Mode | Description |
|---|---|
| **Length bias** | RM prefers longer responses regardless of quality |
| **Sycophancy** | RM rewards agreement with the user's stated opinion |
| **Formatting bias** | RM prefers bullet points, headers, code blocks over equivalent plain text |
| **Annotator disagreement** | Contradictory preferences create a confused RM |
| **Distribution shift** | RM trained on one distribution fails on out-of-distribution outputs |

## Reward Hacking

The most dangerous failure: the LLM finds outputs that score high on the RM but aren't actually good:

```
RM was trained on data where longer responses tended to be preferred
→ LLM learns: "longer = higher reward"
→ LLM generates unnecessarily verbose responses
→ Human satisfaction decreases even as RM score increases
```

This is Goodhart's Law in action: "When a measure becomes a target, it ceases to be a good measure."

### Mitigations

| Strategy | How |
|---|---|
| **KL penalty** | Penalize deviation from the base model (PPO's KL term) |
| **Reward model ensemble** | Average scores from multiple RMs; harder to hack all simultaneously |
| **Iterative training** | Periodically retrain the RM on the latest model's outputs |
| **Process reward models** | Reward each reasoning step, not just the final output |
| **Constitutional AI** | Explicit rules that override the RM's learned preferences |

## Types of Reward Models

### Outcome Reward Models (ORM)
Score the final complete response:
```
Score(prompt, full_response) → scalar
```
Simple but doesn't distinguish between good and bad reasoning paths that reach the same conclusion.

### Process Reward Models (PRM)
Score each intermediate step:
```
Score(prompt, step_1) → scalar
Score(prompt, step_1 + step_2) → scalar
Score(prompt, step_1 + step_2 + step_3) → scalar
```

Benefits:
- Rewards correct reasoning, not just correct answers
- Better for math, code, and [[Chain of Thought]] tasks
- Reduces reward hacking (harder to game every step)
- Used by OpenAI's o-series models for reasoning

### Implicit Reward Models (DPO)

[[Fine-Tuning and Alignment#DPO|DPO]] eliminates the explicit reward model entirely — the LLM itself implicitly learns a reward function during training. This simplifies the pipeline but loses the ability to inspect the reward signal independently.

## Reward Models in Coding

For code-related alignment:

| Signal | Source |
|---|---|
| **Test passage** | Code compiles and tests pass → positive reward |
| **Lint compliance** | No linting errors → bonus |
| **Human review** | Expert rates code quality, correctness, style |
| **SWE-bench results** | Successfully resolves real issues → positive |
| **Execution feedback** | Runtime errors → negative reward |

Code has a significant advantage over natural language: correctness is often **verifiable** (tests pass/fail), making reward modeling more reliable.

## Scale of Reward Modeling

| Provider | RM Training Data (Estimated) |
|---|---|
| Anthropic | 100K–1M+ comparison pairs per model generation |
| OpenAI | Similar scale, plus automated evaluations |
| Meta | Published datasets (HH-RLHF, preference data) |

The quality and diversity of this data is considered a core competitive advantage of frontier AI labs.

## Related

- [[Reinforcement Learning from Human Feedback]]
- [[Reinforcement Learning]]
- [[Fine-Tuning and Alignment]]
- [[AI Safety and Alignment Research]]
- [[Data Labeling and Annotation]]
- [[Chain of Thought]]
- [[Large Language Models]]
