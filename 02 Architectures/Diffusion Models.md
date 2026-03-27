# Diffusion Models

Diffusion models are a class of [[Generative AI]] that create data (typically images) by learning to reverse a gradual noising process. They have become the dominant architecture for image generation, surpassing GANs in quality, diversity, and controllability.

## The Core Idea

### Forward Process (Adding Noise)
Take a clean image and progressively add Gaussian noise over many steps until it becomes pure random noise:

```
Clean image → Slightly noisy → More noisy → ... → Pure noise
   x₀            x₁              x₂              xₜ
```

This is a fixed mathematical process — no learning required.

### Reverse Process (Removing Noise)
Train a [[Neural Networks|neural network]] to predict and remove the noise at each step:

```
Pure noise → Less noisy → Less noisy → ... → Clean image
   xₜ          xₜ₋₁         xₜ₋₂            x₀
```

The model learns: "given this noisy image, what noise was added?" By iteratively removing predicted noise, a clean image emerges from randomness.

## How Generation Works

```
1. Start with random noise
2. Ask the model: "what noise is in this?"
3. Subtract the predicted noise → slightly cleaner image
4. Repeat steps 2-3 for T steps (typically 20-50)
5. Result: a coherent, high-quality image
```

The magic: even though the model was trained on real images with known noise, at generation time it creates entirely new images that never existed.

## Conditioning (Text-to-Image)

To generate images from text prompts, the denoising model is conditioned on a text embedding:

```
"A cat wearing a top hat, oil painting"
        ↓ (text encoder, e.g., CLIP)
   Text embedding
        ↓ (guides denoising)
   Noise → Denoise → Denoise → ... → Image matching the description
```

The text embedding steers the denoising process toward images consistent with the description.

### Classifier-Free Guidance (CFG)

A technique that strengthens the influence of the text condition:
- Generate two predictions: one conditioned on text, one unconditioned
- Amplify the difference between them
- Higher CFG scale → more faithful to prompt, less diverse
- Typical range: 5–15

## Key Architectures

### U-Net Based (Original)

| Model | By | Notes |
|---|---|---|
| DDPM (2020) | UC Berkeley | Original diffusion model for images |
| Stable Diffusion 1.x–2.x | Stability AI | Open-source, latent diffusion |
| DALL-E 2 (2022) | OpenAI | CLIP + diffusion |
| Imagen (2022) | Google | Text-to-image, cascaded diffusion |

### Transformer Based (Current)

| Model | By | Notes |
|---|---|---|
| DALL-E 3 (2023) | OpenAI | Transformer-based, superior text rendering |
| Stable Diffusion 3 / SD3.5 | Stability AI | DiT (Diffusion Transformer) architecture |
| Flux | Black Forest Labs | DiT, state-of-the-art open source |
| Imagen 3 | Google | Transformer-based |

The field is migrating from U-Net to [[Transformers|Transformer]]-based denoisers (DiT — Diffusion Transformers), following the same pattern as NLP.

## Latent Diffusion

Running diffusion directly on pixel space is expensive (a 1024×1024 image is ~3M values). **Latent Diffusion Models (LDMs)** compress the image first:

```
Image (1024×1024×3) → Encoder → Latent (128×128×4) → Diffusion in latent space → Decoder → Image
```

- Diffusion operates on a ~64x smaller representation
- Dramatically faster training and inference
- Stable Diffusion is a latent diffusion model
- The encoder/decoder (VAE) is trained separately

## Diffusion Beyond Images

| Domain | Application | Examples |
|---|---|---|
| **Video** | Text-to-video generation | Sora (OpenAI), Runway Gen-3 |
| **Audio** | Music and speech generation | AudioLDM, Stable Audio |
| **3D** | Object and scene generation | Point-E, DreamFusion |
| **Molecular** | Drug design, protein structure | DiffDock |
| **Robotics** | Action planning | Diffusion Policy |

## Diffusion vs GANs vs Autoregressive

| Aspect | Diffusion | GANs | Autoregressive |
|---|---|---|---|
| Quality | Excellent | Excellent | Good |
| Diversity | High | Mode collapse risk | High |
| Training stability | Stable | Notoriously unstable | Stable |
| Speed | Slow (many steps) | Fast (one pass) | Slow (sequential) |
| Controllability | Strong (guidance) | Weak | Strong (prompting) |
| Current status | Dominant | Declining | Used for text/code |

## Sampling Speed

The main weakness: generating an image requires 20–50 forward passes through the network. Solutions:

- **DDIM** — Fewer steps with deterministic sampling
- **DPM-Solver** — Efficient ODE solvers, 10–20 steps
- **Consistency Models** — 1–2 step generation (OpenAI)
- **Latent Consistency Models** — Fast generation in latent space
- **Distillation** — Train a student model that needs fewer steps

## Impact

Diffusion models have transformed creative industries:
- **Art and design** — Midjourney, DALL-E used by millions
- **Photography** — Inpainting, outpainting, style transfer
- **Advertising** — Rapid concept generation
- **Gaming** — Texture and asset generation
- **Film** — Pre-visualization, concept art

And raised significant concerns about:
- Copyright of training data and generated images
- Deepfakes and misinformation
- Artist displacement
- Provenance and attribution

## Related

- [[Generative AI]]
- [[Neural Networks]]
- [[Transformers]]
- [[Computer Vision]]
- [[Multimodal AI]]
- [[AI Ethics and Safety]]
