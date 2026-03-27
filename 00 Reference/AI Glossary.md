# AI Glossary

A quick-reference glossary of terms used throughout this vault. Each term links to its full note where available.

## A

- **[[Attention Mechanism]]** — Mechanism allowing models to focus on relevant parts of input
- **Agent** — An AI system that uses tools to act autonomously. See [[AI Agents and Sub-Agents]]
- **[[Fine-Tuning and Alignment|Alignment]]** — Ensuring AI systems behave as intended
- **Autoregressive** — Generating one token at a time, each conditioned on previous tokens

## B

- **Backpropagation** — Algorithm for computing gradients through a neural network. See [[Loss Functions and Optimization]]
- **Batch size** — Number of examples processed before a weight update. See [[Training and Inference]]
- **BPE** — Byte-Pair Encoding, dominant [[Tokens and Tokenization|tokenization]] algorithm

## C

- **[[Chain of Thought]]** — Prompting technique where the model reasons step by step
- **CLAUDE.md** — Project instruction file for [[Claude Code]]
- **[[Context Management]]** — Strategies for managing what an LLM can see
- **[[Context Engineering]]** — Designing optimal context window contents
- **Constitutional AI** — Anthropic's alignment approach using explicit principles. See [[AI Safety and Alignment Research]]
- **Cross-entropy** — Standard [[Loss Functions and Optimization|loss function]] for language models

## D

- **[[Diffusion Models]]** — Generative models that create by denoising
- **[[Knowledge Distillation|Distillation]]** — Transferring knowledge from large to small models
- **DPO** — Direct Preference Optimization. See [[Fine-Tuning and Alignment]]

## E

- **[[Embeddings and Vector Search|Embeddings]]** — Dense vector representations of data
- **Epoch** — One full pass through the training data
- **Extended thinking** — Internal reasoning before responding. See [[Chain of Thought]]

## F

- **[[Federated Learning]]** — Training across devices without sharing raw data
- **[[Fine-Tuning and Alignment]]** — Adapting a pre-trained model to a specific task
- **Few-shot** — Providing examples in the prompt. See [[Prompt Engineering]]
- **Flash Attention** — Efficient [[Attention Mechanism]] implementation

## G

- **GAN** — Generative Adversarial Network. See [[Generative AI]]
- **Gradient descent** — Optimization algorithm. See [[Loss Functions and Optimization]]
- **Grounding** — Anchoring model output in real data. See [[Hallucination and Grounding]]
- **GGUF** — Model format for local inference via llama.cpp. See [[Inference Optimization]]

## H

- **[[Hallucination and Grounding|Hallucination]]** — Model generating confident but incorrect information
- **HNSW** — Approximate nearest neighbor algorithm. See [[Embeddings and Vector Search]]
- **Hooks** — Lifecycle event handlers in [[Claude Code]]

## I

- **In-context learning** — Adapting to a task from examples in the prompt
- **[[Inference Optimization]]** — Making models faster and cheaper to run

## K

- **KV caching** — Reusing key/value computations during generation. See [[Inference Optimization]]

## L

- **[[Large Language Models|LLM]]** — Large Language Model
- **LoRA** — Low-Rank Adaptation for efficient fine-tuning. See [[Transfer Learning]]
- **[[Loss Functions and Optimization|Loss function]]** — Measures how wrong predictions are

## M

- **[[Model Context Protocol|MCP]]** — Protocol for connecting AI tools to external services
- **[[Mixture of Experts|MoE]]** — Architecture where only a subset of parameters activate per token
- **[[Multimodal AI]]** — Models processing multiple data types

## N

- **[[Neural Networks]]** — Computational models inspired by biological neurons

## O

- **[[Open Source AI Models]]** — Models with publicly available weights

## P

- **[[Positional Encoding]]** — How [[Transformers]] encode token order
- **[[Prompt Engineering]]** — Crafting effective inputs for LLMs
- **[[Prompt Injection]]** — Security attack via malicious instructions in data

## Q

- **Quantization** — Reducing weight precision. See [[Inference Optimization]]

## R

- **[[Retrieval-Augmented Generation|RAG]]** — Augmenting LLMs with retrieved documents
- **ReAct** — Reason + Act agent pattern. See [[Agentic Design Patterns]]
- **RLHF** — Reinforcement Learning from Human Feedback. See [[Fine-Tuning and Alignment]]
- **RoPE** — Rotary Position Embeddings. See [[Positional Encoding]]

## S

- **[[Scaling Laws]]** — Predictable improvement with more compute/data/parameters
- **Self-attention** — Tokens attending to each other. See [[Attention Mechanism]]
- **SFT** — Supervised Fine-Tuning. See [[Fine-Tuning and Alignment]]
- **[[Structured Output]]** — Constraining LLM output to a schema
- **Sub-agent** — Child agent spawned by a parent. See [[AI Agents and Sub-Agents]]
- **SWE-bench** — Benchmark for coding agents. See [[AI Benchmarks]]
- **[[Synthetic Data]]** — Artificially generated training data

## T

- **[[Tokens and Tokenization]]** — How text is split into units for processing
- **[[Transformers]]** — The dominant neural network architecture
- **[[Transfer Learning]]** — Adapting a model trained on one task to another
- **[[Tool Use in AI]]** — LLMs calling external functions

## V

- **[[Vibe Coding]]** — Coding by describing intent, trusting AI output
- **Vector database** — Storage for [[Embeddings and Vector Search|embeddings]]. See [[Retrieval-Augmented Generation]]

## Z

- **Zero-shot** — No examples, just instructions. See [[Prompt Engineering]]
