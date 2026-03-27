# World Models

World models are AI systems that learn internal representations of how the world (or an environment) works — enabling prediction, planning, and reasoning about future states without direct experience. They represent one of the most active frontiers in AI research and are considered by many as a necessary step toward more capable and general AI.

## The Concept

A world model learns to:
1. **Represent** the current state of an environment
2. **Predict** what happens next given an action
3. **Plan** by simulating future trajectories internally

```
Current State + Action → World Model → Predicted Next State
                                     → Predicted Reward/Outcome
```

This is "imagination" — the ability to think about consequences before acting. Humans do this effortlessly ("if I push this glass, it will fall and break"). AI is learning to do it.

## World Models in Different Domains

### Robotics and Embodied AI

A robot that can predict physics:
```
"If I push this block left, it will slide, hit the wall, and stop"
→ Plan the push to position the block precisely
```

No need for trial-and-error in the real world — the model simulates outcomes internally.

### Video Prediction

Models like Sora (OpenAI) learn world models implicitly:
```
"A ball rolling down a hill"
→ The model predicts: ball accelerates, follows gravity, bounces at the bottom
→ Generates physically plausible video frames
```

Video generation models must understand physics, object permanence, and causality to produce coherent clips. This makes them implicit world models.

### Game Environments

| System | World Model Use |
|---|---|
| **MuZero** (DeepMind, 2020) | Learns game dynamics without being told the rules; achieves superhuman play in Go, chess, Shogi, and Atari |
| **Dreamer** series | Learns environment dynamics from pixels; plans in "dream" space |
| **GAIA-1** (Wayve) | Autonomous driving world model — predicts road scenes |

### Language as a World Model

A [[Large Language Models|Large Language Model]] can be viewed as a world model of *text*:

```
Given: "Alice picked up the red ball and put it in the box."
Query: "Where is the red ball?"
Answer: "In the box." (requires modeling state changes from text)
```

The model maintains an internal representation of entities and their states — a form of world modeling over text.

Whether LLMs are "true" world models (with genuine understanding) or sophisticated pattern matchers is one of the deepest debates in AI.

## The LeCun Vision

Yann LeCun (Meta, Turing Award winner) has been the most vocal advocate for world models as the path to human-level AI:

### Joint Embedding Predictive Architecture (JEPA)

LeCun argues that next-token prediction ([[Causal vs Masked Language Modeling|CLM]]) is fundamentally limited because:
- It predicts in token space (text), not in representation space (meaning)
- It wastes capacity modeling surface-level variation (exact wording) rather than abstract structure

JEPA instead:
1. Encodes observations into a latent representation
2. Predicts future representations (not future tokens)
3. Learns abstract, structured models of the world

```
CLM:  "The cat" → predict token "sat" (surface prediction)
JEPA: [cat scene] → predict [cat sitting scene] (representation prediction)
```

### V-JEPA (Video JEPA)

Meta's video model that learns by predicting masked regions of video in representation space:
- Doesn't generate pixels — predicts abstract features
- Learns about object permanence, motion, physics
- Closer to how humans learn from observation (we don't predict every pixel)

## World Models and Planning

The key advantage of world models: **planning by simulation**.

### Model-Free Approach (current LLM agents)
```
Observe → Act → Observe result → Act → ... (trial and error in the real world)
```

### Model-Based Approach (with world model)
```
Observe → Simulate 10 possible actions in the world model →
Pick the action with the best predicted outcome → Act once
```

This is more efficient — the agent can evaluate many plans mentally before committing to one. It's analogous to how a chess player thinks ahead: "If I move here, they'll respond there, then I can..."

### Monte Carlo Tree Search (MCTS)

The classic algorithm combining world models with search:
```
1. From current state, simulate a random action sequence (using world model)
2. Evaluate the outcome
3. Repeat many times
4. Choose the action that led to the best outcomes most often
```

Used by AlphaGo, AlphaZero, and MuZero. Each simulation is a "rollout" through the world model.

## World Models and [[AI Coding Harnesses]]

Current coding agents use a **model-free** approach — they act, observe, and react. A world model for coding could:

```
"If I refactor this function, will the tests pass?"
→ World model predicts test outcomes without running them
→ Only execute the refactoring that the model predicts will succeed
```

This would dramatically speed up the [[Agentic Coding Paradigm#The Agent Loop|agent loop]] by replacing expensive real-world tool calls with fast mental simulation.

Elements of this already exist:
- [[Chain of Thought|Extended thinking]] is a form of mental simulation
- Plan-then-execute ([[Agentic Design Patterns#Pattern 2 Plan-Then-Execute]]) separates prediction from action
- [[AI-Powered Testing|Mutation testing]] tests the model's understanding of code behavior

## Open Questions

1. **Do LLMs already have world models?** — They can reason about cause and effect in text; is this "understanding" or pattern matching?
2. **Representation vs generation** — Is predicting in latent space (JEPA) fundamentally better than predicting tokens (GPT)?
3. **Grounding** — Can a text-only model have a world model, or does it need sensory experience?
4. **Scalability** — Can world models scale to the complexity of real-world software systems?
5. **Compositionality** — Can world models combine known concepts to predict novel situations?

## Related

- [[Artificial Intelligence]]
- [[Reinforcement Learning]]
- [[Large Language Models]]
- [[Diffusion Models]]
- [[Generative AI]]
- [[Chain of Thought]]
- [[Agentic Design Patterns]]
- [[AI for Science]]
- [[Emergent Abilities]]
