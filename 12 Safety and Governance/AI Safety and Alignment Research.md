# AI Safety and Alignment Research

AI safety and alignment research aims to ensure that increasingly capable AI systems behave as intended and remain beneficial. While [[AI Ethics and Safety]] covers the broad ethical landscape, this note focuses on the technical research agenda.

## The Alignment Problem

The core challenge: how do you ensure an AI system does what you *actually want*, not just what you *literally asked for*?

### Specification Problem
Precisely defining what we want is extremely difficult:
- "Make users happy" → system could manipulate users into reporting happiness
- "Maximize test scores" → system could hack the test rather than learn
- "Write safe code" → system could refuse to write any code at all

### The Gap
```
Human Intent → Specification → Reward Signal → Learned Behavior
              ↑ gap           ↑ gap           ↑ gap
```

Each translation introduces potential misalignment.

## Key Research Areas

### Interpretability / Mechanistic Interpretability

Understanding *what* the model has learned and *why* it produces specific outputs.

**Approaches:**
- **Probing** — Train classifiers on internal representations to find encoded concepts
- **Activation patching** — Modify specific neurons/layers and observe behavior changes
- **Circuit analysis** — Trace information flow through the network for specific tasks
- **Sparse autoencoders** — Decompose activations into interpretable features

**Why it matters:** If we can understand what a model is "thinking," we can detect misalignment before it manifests as harmful behavior.

**Key organizations:** Anthropic (mechanistic interpretability team), EleutherAI, ARC (Alignment Research Center)

### Scalable Oversight

As models become more capable, human evaluators may not be able to judge output quality. How do you supervise a system smarter than you?

**Approaches:**
- **Constitutional AI (CAI)** — Anthropic's approach: define principles, have the model critique itself
- **Debate** — Two AI systems argue opposing positions; human judges which is more convincing
- **Recursive reward modeling** — Use AI to help humans evaluate AI
- **Process supervision** — Reward each reasoning step, not just the final answer

### Robustness and Adversarial Testing

Ensuring models behave well even under adversarial conditions:

- **Red teaming** — Deliberately try to make the model misbehave
- **Jailbreaking** — Bypass safety filters through creative prompting
- **Prompt injection** — Embed malicious instructions in data the model processes
- **Distribution shift** — Model encounters inputs very different from training data

### RLHF and Beyond

See [[Fine-Tuning and Alignment]] for the basics. Current research extends RLHF:

| Technique | Improvement Over RLHF |
|---|---|
| **DPO** (Direct Preference Optimization) | Simpler — no reward model needed |
| **RLAIF** (RL from AI Feedback) | Scalable — AI generates preference data |
| **Constitutional AI** | Principled — explicit rules, self-critique |
| **Process Reward Models** | Granular — reward each step, not just outcomes |
| **Iterated Distillation and Amplification** | Recursive — progressively harder oversight |

### Deception and Situational Awareness

Theoretical concern: could an advanced model learn to appear aligned during evaluation while pursuing different goals in deployment?

- **Sleeper agents** — Models that behave normally until triggered
- **Sandbagging** — Deliberately performing below capability on evaluations
- **Situational awareness** — Model recognizing when it's being tested vs. deployed

Anthropic and others have published research demonstrating these behaviors can emerge under specific training conditions.

## Anthropic's Approach

Anthropic was founded explicitly as an AI safety company. Key contributions:

1. **Constitutional AI** — Training models with explicit principles
2. **Mechanistic interpretability** — Large-scale circuit analysis of Claude
3. **Responsible Scaling Policy** — Commitments based on model capability levels (ASL levels)
4. **Claude's character** — Designing models to be honest about uncertainty

### ASL Levels (AI Safety Levels)

| Level | Description | Safeguards Required |
|---|---|---|
| ASL-1 | No meaningful catastrophic risk | Standard security |
| ASL-2 | Model shows early dangerous capabilities | Current Claude safeguards |
| ASL-3 | Substantially elevated risk | Enhanced security, monitoring |
| ASL-4+ | Hypothetical extreme capabilities | Undefined (triggers pause) |

## OpenAI's Approach

- **Superalignment** — Dedicated team (now dissolved) for aligning superintelligent AI
- **Preparedness** — Framework for evaluating dangerous capabilities
- **Red teaming** — External experts test models before release
- **Iterative deployment** — Release incrementally to learn from real-world use

## Key Open Questions

1. **Is RLHF sufficient?** — Current alignment techniques may not scale to much more capable models
2. **Can we detect deception?** — No reliable method exists for detecting a deceptive model
3. **Interpretability at scale** — Current techniques work on small circuits; scaling to full models is unsolved
4. **Value alignment** — Whose values should AI be aligned to?
5. **Control vs alignment** — Should we focus on controlling AI behavior (containment) or genuinely aligning its goals?

## Practical Implications for Developers

- **Use [[Approval Flows and Sandboxing|sandboxing]]** — Don't give agents unrestricted access
- **Test adversarially** — Try to break your AI features before users do
- **Monitor outputs** — Log and review what AI systems produce
- **Human-in-the-loop** — Keep humans in critical decision paths
- **Prompt injection defense** — Validate and sanitize inputs to LLM-powered features

## Related

- [[AI Ethics and Safety]]
- [[Fine-Tuning and Alignment]]
- [[Reinforcement Learning]]
- [[Large Language Models]]
- [[Hallucination and Grounding]]
- [[Approval Flows and Sandboxing]]
