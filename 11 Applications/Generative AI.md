# Generative AI

Generative AI refers to [[Artificial Intelligence]] systems that create new content — text, images, audio, video, or code — rather than just analyzing or classifying existing data.

## How It Differs from Discriminative AI

| Type | Question | Example |
|---|---|---|
| Discriminative | "Is this a cat or dog?" | Classifier |
| Generative | "Create an image of a cat" | Generator |

Discriminative models learn boundaries between categories. Generative models learn the underlying distribution of the data so they can produce new samples.

## Major Modalities

### Text Generation
- [[Large Language Models]] (GPT, Claude, LLaMA, Gemini)
- Powered by [[Transformers]]
- Applications: writing, coding, reasoning, conversation

### Image Generation
- **Diffusion models** — Stable Diffusion, DALL-E 3, Midjourney
- **GANs** (Generative Adversarial Networks) — StyleGAN
- Process: start from noise, iteratively refine into a coherent image

### Audio Generation
- Text-to-speech (TTS)
- Music generation
- Voice cloning

### Video Generation
- Text-to-video (Sora, Runway)
- Extends image generation with temporal consistency

### Code Generation
- Specialized or general LLMs trained on code
- Autocomplete, full function generation, debugging

## Key Architectures

| Architecture | Mechanism | Used For |
|---|---|---|
| [[Transformers]] | Next-token prediction | Text, code |
| GANs | Generator vs. discriminator | Images, video |
| Diffusion Models | Iterative denoising | Images, video, audio |
| VAEs | Encode-decode with latent space | Images, anomaly detection |

## Implications

- **Creative tools** — Augment human creativity in art, writing, design
- **Productivity** — Automate routine content creation
- **Deepfakes** — Realistic fake media raises trust and safety concerns
- **Copyright** — Questions about ownership of AI-generated and training content
- **Job impact** — Changes how knowledge work is done

See [[AI Ethics and Safety]] for deeper discussion.

## Related

- [[Large Language Models]]
- [[Deep Learning]]
- [[Computer Vision]]
- [[Prompt Engineering]]
