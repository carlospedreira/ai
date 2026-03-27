# Large Language Models

Large Language Models (LLMs) are [[Neural Networks|neural networks]] — typically [[Transformers|decoder-only Transformers]] — trained on vast amounts of text to predict the next token in a sequence. Despite this simple objective, they develop broad capabilities in reasoning, writing, coding, and more.

## How They Work

1. **Pre-training** — The model is trained on a massive text corpus (books, websites, code) to predict the next word. This phase requires enormous compute and data.
2. **Fine-tuning / RLHF** — The pre-trained model is further trained to follow instructions, be helpful, and avoid harmful outputs. Techniques include:
   - Supervised fine-tuning (SFT) on high-quality demonstrations
   - Reinforcement Learning from Human Feedback (RLHF)
   - Constitutional AI (CAI) and other alignment methods
3. **[[Training and Inference|Inference]]** — The model generates text by predicting one token at a time, sampling from the probability distribution over its vocabulary.

## Key Properties

- **Emergent abilities** — Capabilities that appear at sufficient scale (e.g., chain-of-thought reasoning, few-shot learning)
- **In-context learning** — Ability to adapt to a task from examples provided in the prompt, without weight updates
- **Scaling laws** — Predictable performance improvement with more parameters, data, and compute

## Parameters and Scale

| Model | Parameters | Organization |
|---|---|---|
| GPT-3 | 175B | OpenAI |
| Claude 3.5 Sonnet | Undisclosed | Anthropic |
| LLaMA 3 | 8B–405B | Meta |
| Gemini | Undisclosed | Google |

## Capabilities

- Text generation and summarization
- Question answering and reasoning
- Code generation and debugging
- Translation
- Tool use and agentic behavior

## Limitations

- **Hallucination** — Generating confident but incorrect information
- **Context window limits** — Can only process a fixed amount of text at once
- **Knowledge cutoff** — Training data has a fixed end date
- **Reasoning gaps** — Can fail on novel logical or mathematical problems

## Related

- [[Transformers]]
- [[Generative AI]]
- [[Prompt Engineering]]
- [[AI Ethics and Safety]]
- [[Training and Inference]]
