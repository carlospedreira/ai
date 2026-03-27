# Multimodal AI

Multimodal AI refers to systems that can process and generate multiple types of data — text, images, audio, video, and code — within a single model. Modern frontier [[Large Language Models]] are increasingly multimodal, enabling richer interactions and new capabilities.

## What "Multimodal" Means

| Modality | Input | Output |
|---|---|---|
| Text | Read/understand text | Generate text |
| Images | Analyze photos, screenshots, diagrams | Generate images |
| Audio | Transcribe speech, understand sounds | Generate speech, music |
| Video | Understand video content | Generate video |
| Code | Read/understand code | Generate/edit code |

A multimodal model handles some or all of these within a single architecture, rather than requiring separate models for each.

## How Multimodal Models Work

### Vision-Language Models

The most common multimodal combination. Images are converted to tokens using a **vision encoder** (often a Vision Transformer / ViT), then processed alongside text tokens by the [[Transformers|Transformer]]:

```
Image → Vision Encoder → Image Tokens ─┐
                                        ├─→ Transformer → Output
Text  → Tokenizer      → Text Tokens  ─┘
```

The model learns to relate visual and textual information during training, enabling it to answer questions about images, describe visual content, and reason across modalities.

### Audio Models

Audio is typically converted to a spectrogram or discrete audio tokens, then processed by the same transformer:
- **Speech-to-text** — Whisper (OpenAI), converting audio to text
- **Text-to-speech** — Generate natural-sounding speech from text
- **Audio understanding** — Classify sounds, transcribe music

### Native Multimodal vs. Pipeline

| Approach | How It Works | Example |
|---|---|---|
| **Native** | Single model processes all modalities | GPT-4o, Gemini 2.5 |
| **Pipeline** | Separate models chained together | Whisper → GPT-4 → TTS |

Native multimodal models are more capable because they can reason across modalities simultaneously, rather than losing information at each handoff.

## Major Multimodal Models

| Model | Modalities | Notable Capability |
|---|---|---|
| GPT-4o / GPT-5 | Text, image, audio, video | Real-time voice conversation |
| Claude (Opus/Sonnet) | Text, image | Strong visual reasoning, PDF analysis |
| Gemini 2.5 Pro | Text, image, audio, video | 1M token context, native audio |
| LLaMA 3.2 Vision | Text, image | Open-source multimodal |
| Qwen-VL | Text, image | Open-source, strong OCR |

## Multimodal AI in Coding

Multimodal capabilities have practical applications in [[AI Coding Harnesses]]:

### Screenshot-to-Code
Paste a screenshot of a UI mockup, and the agent implements it:
```
"Here's a screenshot of the design. Implement this page using React and Tailwind."
```
[[OpenAI Codex]] supports this via `--image` flag. [[Claude Code]] accepts images in prompts.

### Diagram Understanding
Feed architecture diagrams, flowcharts, or ERDs to the agent:
```
"Here's our system architecture diagram. Add a new payment microservice
that connects to the API gateway and the database."
```

### Error Screenshot Debugging
Paste a screenshot of an error dialog or browser console:
```
"This is what the user sees. Fix the frontend error."
```

### Whiteboard-to-Code
Photograph a whiteboard sketch and ask the agent to implement the algorithm or design shown.

## Multimodal Generation

### Text-to-Image
- **DALL-E 3** (OpenAI) — Generates images from text descriptions
- **Stable Diffusion** — Open-source image generation
- **Midjourney** — High-quality artistic image generation

### Text-to-Video
- **Sora** (OpenAI) — Generate video from text prompts
- **Runway Gen-3** — Video generation and editing

### Text-to-Audio
- **ElevenLabs** — Voice cloning and generation
- **Suno** — Music generation from text

## Why Multimodality Matters

1. **Richer communication** — Developers can show, not just tell
2. **Accessibility** — Voice interaction, image description
3. **New workflows** — Design → code pipelines, visual debugging
4. **Unified models** — One model replaces multiple specialized tools
5. **Real-world grounding** — Models understand the physical/visual world

## Related

- [[Large Language Models]]
- [[Computer Vision]]
- [[Generative AI]]
- [[Transformers]]
- [[AI Coding Harnesses]]
