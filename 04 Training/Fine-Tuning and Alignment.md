# Fine-Tuning and Alignment

Fine-tuning and alignment are the processes that transform a raw pre-trained [[Large Language Models|LLM]] into a useful, safe assistant. Pre-training gives the model knowledge; fine-tuning and alignment give it behavior.

## The Training Pipeline

```
┌─────────────────┐     ┌────────────────┐     ┌───────────────┐
│  Pre-Training   │────►│  Fine-Tuning   │────►│   Alignment   │
│                 │     │                │     │               │
│ Learn language  │     │ Learn to follow│     │ Learn to be   │
│ from raw text   │     │ instructions   │     │ helpful, safe │
└─────────────────┘     └────────────────┘     └───────────────┘
```

## Pre-Training (Recap)

See [[Training and Inference]]. The model learns to predict the next token from a massive text corpus. After pre-training, the model can complete text but doesn't know how to follow instructions or have a conversation.

## Supervised Fine-Tuning (SFT)

Train the model on (instruction, response) pairs created by human experts.

### How It Works
1. Humans write thousands of example conversations
2. Each example shows the ideal response to a given instruction
3. The model is trained to replicate these patterns
4. Result: the model learns to follow instructions, answer questions, write code

### Example Training Data
```
Instruction: "Write a Python function to reverse a string"
Response: "def reverse_string(s): return s[::-1]"
```

### Types of Fine-Tuning

| Type | Data Required | Cost | When to Use |
|---|---|---|---|
| Full fine-tuning | Large dataset | Very high | Maximum customization |
| LoRA / QLoRA | Smaller dataset | Low | Most practical cases |
| Prompt tuning | Minimal | Lowest | Light adaptation |

**LoRA** (Low-Rank Adaptation) fine-tunes only a small set of adapter weights rather than the full model, making it practical on consumer hardware.

## Alignment

Alignment ensures the model is not just capable but also helpful, harmless, and honest. This goes beyond following instructions to understanding intent.

### RLHF (Reinforcement Learning from Human Feedback)

The dominant alignment technique. See also [[Reinforcement Learning]].

```
┌────────────┐     ┌──────────────┐     ┌──────────────┐
│  Reward    │────►│    PPO       │────►│   Aligned    │
│  Model     │     │ (optimize    │     │    Model     │
│            │     │  for reward) │     │              │
└─────┬──────┘     └──────────────┘     └──────────────┘
      │
┌─────┴──────┐
│  Human     │
│ Preferences│
│ (A > B)    │
└────────────┘
```

Steps:
1. Generate multiple responses to the same prompt
2. Humans rank which response is better
3. Train a **reward model** on these preferences
4. Use PPO (Proximal Policy Optimization) to optimize the LLM against the reward model

### Constitutional AI (CAI)

Anthropic's approach, used for Claude:
1. Define a set of principles ("constitution") the model should follow
2. Model critiques its own outputs against these principles
3. Model revises its outputs to be more aligned
4. Train on the self-revised outputs

Advantages: less reliance on human labelers, more scalable, principles are explicit.

### DPO (Direct Preference Optimization)

A simpler alternative to RLHF that skips the reward model:
- Take preference pairs (chosen response vs. rejected response)
- Directly optimize the model to prefer the chosen response
- Mathematically equivalent to RLHF under certain assumptions
- Much simpler to implement

## Alignment Challenges

### Reward Hacking
The model finds loopholes in the reward signal:
- Being overly verbose (longer = more "helpful"?)
- Sycophantic agreement (agreeing = higher reward?)
- Refusing too much ("I can't do that" = safe but unhelpful)

### Alignment Tax
Alignment can reduce raw capability:
- Safety constraints may prevent useful outputs
- Being too cautious means refusing legitimate requests
- Balance between safety and helpfulness

### Scalable Oversight
As models become more capable, how do we evaluate if their outputs are correct? Humans may not be able to judge superhuman reasoning.

## Custom Fine-Tuning (For Users)

Many providers offer fine-tuning as a service:

| Provider | Service | Minimum Data |
|---|---|---|
| OpenAI | Fine-tuning API | ~50 examples |
| Anthropic | Custom models (enterprise) | Negotiated |
| Google | Vertex AI tuning | ~100 examples |
| Together AI | Open model fine-tuning | Varies |

### When to Fine-Tune vs. Prompt

| Approach | Best For |
|---|---|
| [[Prompt Engineering]] | Quick iteration, small adaptations |
| [[Retrieval-Augmented Generation]] | Adding knowledge |
| Fine-tuning | Changing behavior/style, high-volume production |
| Full training | Research, new capabilities |

## Related

- [[Large Language Models]]
- [[Training and Inference]]
- [[Reinforcement Learning]]
- [[AI Ethics and Safety]]
- [[Prompt Engineering]]
