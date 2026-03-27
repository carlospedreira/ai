# Reinforcement Learning

Reinforcement Learning (RL) is a [[Machine Learning]] paradigm where an **agent** learns to make decisions by interacting with an **environment** and receiving **rewards** or **penalties**.

## Core Loop

```
Agent → Action → Environment → State + Reward → Agent → ...
```

The agent's goal is to learn a **policy** — a strategy that maximizes cumulative reward over time.

## Key Concepts

- **Agent** — The learner / decision-maker
- **Environment** — The world the agent operates in
- **State** — The current situation
- **Action** — What the agent can do
- **Reward** — Feedback signal (positive or negative)
- **Policy** — Mapping from states to actions
- **Value function** — Expected future reward from a state
- **Episode** — One complete run from start to terminal state

## Exploration vs Exploitation

A fundamental tradeoff:
- **Exploration** — Try new actions to discover potentially better strategies
- **Exploitation** — Use the best known strategy to maximize reward

## Algorithms

| Algorithm | Type | Notes |
|---|---|---|
| Q-Learning | Value-based | Learns value of state-action pairs |
| Deep Q-Network (DQN) | Value-based + [[Deep Learning]] | Q-learning with [[Neural Networks]] |
| Policy Gradient | Policy-based | Directly optimize the policy |
| PPO (Proximal Policy Optimization) | Policy-based | Stable training, widely used |
| Actor-Critic | Hybrid | Combines value and policy methods |

## Notable Applications

- **Game playing** — AlphaGo (Go), OpenAI Five (Dota 2), AlphaStar (StarCraft)
- **Robotics** — Learning locomotion, manipulation, navigation
- **RLHF** — Reinforcement Learning from Human Feedback, used to align [[Large Language Models]] with human preferences
- **Resource management** — Data center cooling, chip design

## RL in LLM Alignment

RLHF is a critical step in making [[Large Language Models]] helpful and safe:

1. Train a **reward model** from human preference data
2. Use PPO (or similar) to optimize the LLM's outputs against the reward model
3. The LLM learns to generate responses that humans rate highly

## Related

- [[Machine Learning]]
- [[Supervised vs Unsupervised Learning]]
- [[Large Language Models]]
- [[AI Ethics and Safety]]
