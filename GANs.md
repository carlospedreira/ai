# GANs

Generative Adversarial Networks (GANs) are a [[Generative AI]] architecture where two [[Neural Networks]] compete against each other — a generator that creates fake data and a discriminator that tries to distinguish fakes from real data. This adversarial training produces remarkably realistic outputs.

## The Adversarial Game

```
Random Noise → Generator → Fake Image
                              ↓
Real Image → ┐         Discriminator → "Real" or "Fake"
              └──────────────↑
```

### Generator
- Takes random noise as input
- Produces synthetic data (images, audio, etc.)
- Goal: fool the discriminator into thinking its output is real
- Gets better at creating realistic fakes over time

### Discriminator
- Receives both real and generated data
- Classifies each as "real" or "fake"
- Goal: correctly identify fakes
- Gets better at detecting fakes over time

### The Nash Equilibrium
Training converges (ideally) when:
- The generator produces data indistinguishable from real data
- The discriminator outputs 50% for everything (can't tell the difference)

In practice, reaching this equilibrium is notoriously difficult.

## How Training Works

```
Round 1: Generator creates terrible fakes
         Discriminator easily spots them → strong feedback to generator

Round 100: Generator creates decent fakes
           Discriminator still catches subtle artifacts → targeted feedback

Round 10000: Generator creates near-perfect fakes
             Discriminator struggles to tell the difference
```

The loss function encodes this minimax game:
```
min_G max_D  E[log D(x)] + E[log(1 - D(G(z)))]
```

The generator minimizes this while the discriminator maximizes it.

## Notable GAN Architectures

| GAN Variant | Year | Innovation |
|---|---|---|
| **Original GAN** | 2014 | Introduced the adversarial framework (Ian Goodfellow) |
| **DCGAN** | 2015 | Deep convolutional architecture, stabilized training |
| **Pix2Pix** | 2016 | Paired image-to-image translation |
| **CycleGAN** | 2017 | Unpaired image translation (horse ↔ zebra) |
| **Progressive GAN** | 2017 | Gradually increase resolution during training |
| **StyleGAN** | 2018 | Style-based generation, high-quality faces |
| **StyleGAN2** | 2019 | Removed artifacts, near-photorealistic output |
| **StyleGAN3** | 2021 | Alias-free generation |
| **GigaGAN** | 2023 | Scaled GANs to compete with diffusion models |

## GANs vs Diffusion Models

[[Diffusion Models]] have largely replaced GANs for image generation:

| Aspect | GANs | Diffusion Models |
|---|---|---|
| Image quality | Excellent | Excellent |
| Training stability | Notoriously unstable | Stable |
| Mode diversity | Mode collapse risk | High diversity |
| Speed | Fast (single pass) | Slow (many denoising steps) |
| Controllability | Difficult | Strong (classifier guidance, text conditioning) |
| Text-to-image | Weak | Dominant |
| Status (2026) | Declining in image gen | Dominant |

## Training Challenges

### Mode Collapse
The generator learns to produce only a few types of output that fool the discriminator, ignoring the full diversity of the data:
```
Training data: cats, dogs, birds, fish
Generator output: only cats (but very realistic ones)
```

### Training Instability
The generator and discriminator can oscillate rather than converge:
- Discriminator gets too strong → generator gets no useful gradient signal
- Generator gets too strong → discriminator gives random feedback
- Balancing the two requires careful tuning

### Evaluation Difficulty
No easy loss metric tracks "how good are the generated samples." Common metrics:
- **FID (Fréchet Inception Distance)** — Lower is better; compares distributions
- **IS (Inception Score)** — Higher is better; measures quality and diversity
- **Human evaluation** — Still the gold standard

## Where GANs Still Excel

Despite diffusion models' dominance, GANs remain useful for:

| Application | Why GANs |
|---|---|
| **Real-time generation** | Single forward pass, no iterative denoising |
| **Video super-resolution** | Fast, high-quality upscaling |
| **Data augmentation** | Generate additional training examples |
| **Image editing** | Style transfer, domain adaptation |
| **3D generation** | Some architectures still GAN-based |

## Cultural Impact

- **Deepfakes** — Face-swapping technology built on GAN architectures
- **AI art debate** — GANs were the first models to raise questions about AI creativity
- **"This Person Does Not Exist"** — StyleGAN demo that went viral, showing photorealistic faces of non-existent people

## Related

- [[Generative AI]]
- [[Diffusion Models]]
- [[Neural Networks]]
- [[Deep Learning]]
- [[Computer Vision]]
- [[AI Ethics and Safety]]
