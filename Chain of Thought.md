# Chain of Thought

Chain of Thought (CoT) is a [[Prompt Engineering]] technique where the model is encouraged to reason step by step before producing a final answer. It dramatically improves performance on math, logic, coding, and multi-step reasoning tasks.

## The Discovery

Wei et al. (2022, Google) showed that simply adding "Let's think step by step" to a prompt could improve accuracy on math problems from ~18% to ~79% with PaLM 540B. The model was always *capable* of reasoning — it just needed to be told to show its work.

## How It Works

### Without CoT
```
Q: If a store has 23 apples and sells 17, then receives 12 more, how many does it have?
A: 28  ← wrong (model rushes to answer)
```

### With CoT
```
Q: If a store has 23 apples and sells 17, then receives 12 more, how many does it have?
A: Let me work through this step by step.
   - Start with 23 apples
   - Sell 17: 23 - 17 = 6
   - Receive 12: 6 + 12 = 18
   The store has 18 apples.  ← correct
```

The intermediate steps act as a **scratchpad** — each step becomes context for the next, keeping the model's reasoning on track.

## Variants

### Zero-Shot CoT
No examples needed. Just append a trigger phrase:
- "Let's think step by step."
- "Think carefully before answering."
- "Reason through this problem."

### Few-Shot CoT
Provide examples that demonstrate step-by-step reasoning:
```
Q: [example problem]
A: Step 1: ... Step 2: ... Step 3: ... Answer: ...

Q: [actual problem]
A:
```

### Self-Consistency
Generate multiple CoT reasoning paths, take the majority answer:
```
Path 1: ... → Answer: 18
Path 2: ... → Answer: 18
Path 3: ... → Answer: 17
Majority → 18 ✓
```

Reduces errors from individual reasoning mistakes.

### Tree of Thought
Explore branching reasoning paths, evaluate which is most promising before committing:
```
         Problem
        /   |   \
    Approach Approach Approach
       A       B       C
       |       |       |
     Eval    Eval    Eval
       \       |
      Best path(s)
          |
       Continue
```

Used for problems with multiple valid approaches.

## Extended Thinking in Modern Models

Modern frontier models have **internalized** chain-of-thought into their architecture:

### Claude's Extended Thinking
[[Claude Code]] uses effort levels (`low`, `medium`, `high`, `max`) to control how much internal reasoning Claude does before responding. The thinking happens in a special block that's billed separately.

### OpenAI's o-series (o1, o3, o4-mini)
Trained specifically for extended reasoning. The model spends variable compute on "thinking tokens" before producing an answer. This is [[Scaling Laws|test-time compute scaling]].

### Key Difference
- **Prompt-based CoT** (2022): User asks the model to reason
- **Extended thinking** (2024+): The model is trained to reason internally, controlled by the harness

## CoT in Coding Agents

Chain-of-thought is fundamental to the [[Agentic Coding Paradigm]]. When a coding agent tackles a task:

```
Think: "I need to understand the auth module first"
Act:   read_file("src/auth/index.ts")
Think: "I see it uses JWT. The bug is likely in token validation"
Act:   grep("validateToken", "src/")
Think: "Found it. The expiry check uses < instead of <="
Act:   edit_file(...)
Think: "Now I should run the tests to verify"
Act:   bash("npm test")
```

Each "Think" step is chain-of-thought reasoning that guides the next action. Without it, the agent would act randomly.

## When CoT Helps Most

| Task Type | CoT Impact |
|---|---|
| Arithmetic / math | Very high — prevents calculation errors |
| Multi-step logic | Very high — maintains state across steps |
| Code debugging | High — traces execution flow |
| Planning | High — decomposes complex goals |
| Factual recall | Low — the answer is either known or not |
| Creative writing | Low — reasoning isn't the bottleneck |

## Limitations

- **Longer outputs** — More tokens = higher cost and latency
- **Unfaithful reasoning** — The model may arrive at the right answer for wrong reasons
- **Overthinking** — Simple questions don't benefit from elaborate reasoning
- **Token cost** — Extended thinking tokens are billed (sometimes at higher rates)

## Related

- [[Prompt Engineering]]
- [[Large Language Models]]
- [[Agentic Coding Paradigm]]
- [[Claude Code]]
- [[Scaling Laws]]
- [[Tokens and Tokenization]]
