# Reinforcement Learning from Human Feedback

RLHF is the technique that transforms a raw pre-trained [[Large Language Models|LLM]] into a helpful, harmless assistant. It aligns the model's behavior with human preferences using [[Reinforcement Learning]]. RLHF is the reason models like Claude, ChatGPT, and Gemini are conversational and follow instructions rather than just predicting text.

## The Three-Stage Pipeline

### Stage 1: Pre-Training
See [[Training and Inference]]. Train the base model on trillions of tokens of text. After this stage, the model can complete text but doesn't follow instructions or have a conversational style.

### Stage 2: Supervised Fine-Tuning (SFT)
Train on human-written (instruction, response) pairs:
```
Instruction: "Write a haiku about programming"
Response:    "Bugs hide in the code / Debugging late through the night / One semicolon"
```

After SFT, the model follows instructions but may still produce harmful, verbose, or sycophantic responses.

### Stage 3: RLHF
The key innovation. Three sub-steps:

#### a) Collect Comparison Data
Generate multiple responses to the same prompt. Human raters rank which response is better:
```
Prompt: "Is it safe to mix bleach and ammonia?"

Response A: "No, mixing bleach and ammonia produces toxic chloramine gas..."
Response B: "Sure! Here's how to mix them safely..."

Human label: A >> B
```

#### b) Train a Reward Model
A separate neural network learns to predict human preferences:
```
Reward Model(prompt, response) → score
```

Trained on the comparison data so that preferred responses get higher scores.

#### c) Optimize the Policy with RL
Use PPO (Proximal Policy Optimization) to fine-tune the LLM to maximize the reward model's score:
```
LLM generates response → Reward Model scores it → PPO updates LLM weights
                                                    to increase score
```

A KL divergence penalty prevents the LLM from straying too far from the SFT model (which would cause reward hacking — finding exploits in the reward model).

## Why Not Just Use SFT?

| Aspect | SFT Only | SFT + RLHF |
|---|---|---|
| Learns from | Individual "correct" responses | Relative preferences (A > B) |
| Captures | Surface patterns | Subtle quality differences |
| Handles ambiguity | Poorly (one right answer) | Well (learns what's "better") |
| Diversity | Can mode-collapse to one style | Maintains diverse, high-quality output |

It's easier for humans to say "A is better than B" than to write the perfect response from scratch. RLHF exploits this.

## Alternatives and Evolution

### DPO (Direct Preference Optimization)
See [[Fine-Tuning and Alignment]]. Skips the reward model entirely — optimizes directly on preference pairs. Simpler, cheaper, and increasingly popular.

### Constitutional AI (CAI)
Anthropic's approach used for Claude:
1. Define a set of principles ("constitution")
2. Model generates responses, then critiques itself against the constitution
3. Model revises its responses
4. Train on the self-revised data
5. Optionally apply RLHF on top

Advantages: more scalable than pure human feedback, principles are explicit and auditable.

### RLAIF (RL from AI Feedback)
Use a strong AI model to generate preference labels instead of humans:
- Much cheaper and faster to scale
- Can generate millions of comparisons
- Quality depends on the labeling model's judgment

### Process Reward Models (PRM)
Instead of rewarding the final answer, reward each intermediate reasoning step:
```
Step 1: Correct reasoning → reward
Step 2: Wrong turn → penalty
Step 3: Recovers → reward
```

Better for [[Chain of Thought]] reasoning tasks (math, code) where the process matters as much as the answer.

### Iterative RLHF
Run RLHF in cycles:
1. Deploy model → collect real user feedback
2. Train new reward model on updated preferences
3. Run RLHF again
4. Deploy improved model → repeat

Each cycle improves both the reward model and the policy.

## Challenges

### Reward Hacking
The model finds loopholes in the reward model:
- Being excessively verbose (more text = higher reward?)
- Sycophantic agreement (telling users what they want to hear)
- Hedging everything ("As an AI, I should note that...")

### Reward Model Quality
The reward model is only as good as its training data:
- Human raters disagree on what's "better"
- Cultural biases in the rater pool affect model behavior
- Reward model may not generalize to edge cases

### Alignment Tax
Safety-aligned models sometimes refuse reasonable requests or add unnecessary caveats. Finding the balance between safety and helpfulness is an ongoing challenge.

### Scalable Oversight
As models become more capable, humans may not be able to judge response quality for complex tasks. See [[AI Safety and Alignment Research#Scalable Oversight]].

## RLHF in Practice

| Company | Approach | Key Detail |
|---|---|---|
| **Anthropic** | Constitutional AI + RLHF | Explicit principles, self-critique |
| **OpenAI** | RLHF + iterative refinement | Large-scale human feedback collection |
| **Google** | RLHF + various techniques | Applied to Gemini family |
| **Meta** | RLHF for LLaMA chat variants | Open-source aligned models |

## RLHF for Coding

RLHF is also applied to coding capabilities:
- Human raters evaluate code quality, correctness, and helpfulness
- [[AI Benchmarks|SWE-bench]] and HumanEval results drive preference collection
- Code-specific reward models evaluate whether generated code compiles, passes tests, follows conventions
- [[AI Code Review]] data can feed back into RLHF training

## Related

- [[Reinforcement Learning]]
- [[Fine-Tuning and Alignment]]
- [[AI Safety and Alignment Research]]
- [[Large Language Models]]
- [[Data Labeling and Annotation]]
- [[Chain of Thought]]
- [[Synthetic Data]]
