# Convolutional Neural Networks

Convolutional Neural Networks (CNNs) are a class of [[Neural Networks]] designed to process data with spatial structure — primarily images, but also audio, time series, and other grid-like data. They dominated [[Computer Vision]] from 2012 until the rise of Vision [[Transformers]] around 2020.

## The Core Idea

Instead of connecting every neuron to every input (fully connected), CNNs use small **filters** (kernels) that slide across the input, detecting local patterns:

```
Input Image     Filter (3×3)     Feature Map
┌─────────┐     ┌───────┐       ┌──────┐
│ . . . . │     │ 1 0 1 │       │      │
│ . X . . │  *  │ 0 1 0 │  =    │  X   │  (detects a pattern)
│ . . . . │     │ 1 0 1 │       │      │
│ . . . . │     └───────┘       └──────┘
└─────────┘
```

Early layers detect simple patterns (edges, corners). Deeper layers combine these into complex features (eyes, wheels, text characters).

## Key Concepts

### Convolution
Slide a small filter across the input, computing dot products at each position. Each filter produces one feature map. Multiple filters = multiple feature maps (channels).

### Pooling
Reduce spatial dimensions by taking the max or average of small regions:
```
[4 2]     Max Pool
[1 3]  →  [4]  (2×2 → 1×1)
```

Reduces computation and provides some translation invariance.

### Stride
How far the filter moves at each step. Stride 1 = one pixel at a time. Stride 2 = skip every other position (halves the output size).

### Padding
Add zeros around the input border so the filter can process edge pixels. "Same" padding preserves spatial dimensions.

## Architecture

A typical CNN:
```
Input → [Conv → ReLU → Pool] × N → Flatten → Fully Connected → Output
         ↑ feature extraction        ↑ classification
```

Each conv block learns increasingly abstract features:
- Layer 1: edges, colors, textures
- Layer 2: corners, simple shapes
- Layer 3: object parts (eyes, wheels)
- Layer 4+: whole objects, scenes

## Landmark Architectures

| Architecture | Year | Layers | Innovation |
|---|---|---|---|
| **LeNet-5** | 1998 | 7 | Pioneer CNN for digit recognition |
| **AlexNet** | 2012 | 8 | Won ImageNet, sparked the [[Deep Learning]] revolution |
| **VGG** | 2014 | 16–19 | Showed depth matters (small 3×3 filters throughout) |
| **GoogLeNet/Inception** | 2014 | 22 | Multi-scale parallel convolutions |
| **ResNet** | 2015 | 50–152 | [[Residual Connections]] enabled very deep networks |
| **EfficientNet** | 2019 | Variable | Optimal scaling of width, depth, resolution |
| **ConvNeXt** | 2022 | Variable | Modernized CNN matching Vision Transformer performance |

## CNNs vs Vision Transformers

Vision [[Transformers]] (ViT) split images into patches and process them with self-[[Attention Mechanism|attention]]:

| Aspect | CNNs | Vision Transformers |
|---|---|---|
| Inductive bias | Local patterns (translation equivariance) | Global patterns (no spatial bias) |
| Data efficiency | Better with small datasets | Need large datasets |
| Compute at scale | Less efficient | Scales better |
| State of the art | Declining (but competitive with ConvNeXt) | Dominant since 2020 |
| Interpretability | Feature maps are visualizable | Attention maps less intuitive |

In practice, many modern architectures are **hybrids** — using convolutions in early layers for efficiency and attention in later layers for global reasoning.

## Beyond Images

CNNs are also used for:

| Domain | Application |
|---|---|
| **Audio** | Spectrogram processing, speech recognition |
| **Text** | Text classification (TextCNN), character-level models |
| **Time series** | 1D convolutions for sensor data, financial data |
| **3D data** | 3D convolutions for video, medical scans (CT, MRI) |
| **Graphs** | Graph convolutions for molecular structure |

## The 1×1 Convolution Trick

A filter of size 1×1 doesn't capture spatial patterns — instead it:
- Mixes information across channels
- Reduces or expands channel count (cheaper than full convolutions)
- Acts like a per-pixel fully connected layer

Used extensively in Inception, ResNet, and efficient architectures.

## Related

- [[Computer Vision]]
- [[Neural Networks]]
- [[Deep Learning]]
- [[Residual Connections]]
- [[Activation Functions]]
- [[Transformers]]
