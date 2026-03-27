# Computer Vision

Computer Vision (CV) is the field of [[Artificial Intelligence]] focused on enabling machines to interpret and understand visual information from images and video.

## Evolution

### Classical CV (pre-2012)
- Hand-crafted features (SIFT, HOG, Haar cascades)
- Traditional [[Machine Learning]] classifiers on extracted features

### Deep Learning CV (2012–present)
- Convolutional Neural Networks (CNNs) learn features directly from pixels
- **AlexNet (2012)** winning ImageNet marked the start of the [[Deep Learning]] revolution in CV
- [[Transformers]] (Vision Transformers / ViT) are increasingly used

## Core Tasks

| Task | Description |
|---|---|
| Image classification | What is in this image? |
| Object detection | Where are the objects? (bounding boxes) |
| Semantic segmentation | Label every pixel by category |
| Instance segmentation | Label every pixel by individual object |
| Pose estimation | Detect body/hand/face keypoints |
| Image generation | Create new images from text or noise ([[Generative AI]]) |
| OCR | Extract text from images |
| Depth estimation | Infer 3D structure from 2D images |

## Key Architectures

| Architecture | Year | Contribution |
|---|---|---|
| AlexNet | 2012 | Proved deep CNNs work for vision |
| VGG | 2014 | Deeper networks with small filters |
| ResNet | 2015 | Residual connections for very deep networks |
| YOLO | 2016 | Real-time object detection |
| Vision Transformer (ViT) | 2020 | Applied [[Transformers]] to images |
| CLIP | 2021 | Connected images and text in shared space |

## Multimodal AI

Modern systems combine vision with language:
- **CLIP** — Match images to text descriptions
- **Multimodal LLMs** — [[Large Language Models]] that can see (GPT-4V, Claude with vision, Gemini)
- **Text-to-image** — Generate images from prompts (DALL-E, Stable Diffusion, Midjourney)

## Applications

- Autonomous vehicles
- Medical imaging (X-ray, MRI analysis)
- Surveillance and security
- Manufacturing quality control
- Augmented / virtual reality

## Related

- [[Deep Learning]]
- [[Neural Networks]]
- [[Generative AI]]
