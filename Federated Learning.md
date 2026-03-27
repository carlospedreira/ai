# Federated Learning

Federated learning is a [[Machine Learning]] technique where a model is trained across multiple decentralized devices or servers holding local data, without exchanging the raw data itself. Only model updates (gradients) are shared, keeping data private.

## The Problem It Solves

Traditional ML requires centralizing all training data:

```
Device A data ─┐
Device B data ──┤→ Central Server → Train Model
Device C data ──┘
```

This is problematic when:
- Data is sensitive (medical records, financial data, personal messages)
- Data is too large to transmit (edge devices, IoT)
- Regulations prevent data sharing (GDPR, HIPAA)
- Organizations don't trust each other with raw data

## How It Works

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│Device A  │  │Device B  │  │Device C  │
│Local data│  │Local data│  │Local data│
│Train     │  │Train     │  │Train     │
│locally   │  │locally   │  │locally   │
└────┬─────┘  └────┬─────┘  └────┬─────┘
     │ gradients    │              │
     └──────────────┼──────────────┘
                    ▼
            ┌───────────────┐
            │   Aggregator  │
            │ Average grads │
            │ Update global │
            │    model      │
            └───────┬───────┘
                    │ updated model
     ┌──────────────┼──────────────┐
     ▼              ▼              ▼
┌─────────┐  ┌─────────┐  ┌─────────┐
│Device A  │  │Device B  │  │Device C  │
│(repeat)  │  │(repeat)  │  │(repeat)  │
└──────────┘  └──────────┘  └──────────┘
```

### The FedAvg Algorithm

1. Server sends global model to all participants
2. Each participant trains on their local data for a few steps
3. Participants send updated weights (or gradients) back to the server
4. Server averages all updates → new global model
5. Repeat

## Key Properties

| Property | Traditional ML | Federated Learning |
|---|---|---|
| Data location | Centralized | Stays on device |
| Privacy | Low (data is shared) | High (only gradients shared) |
| Communication | Upload raw data once | Multiple rounds of gradient exchange |
| Data heterogeneity | Uniform (curated dataset) | Non-IID (each device has different data) |
| Compute | Centralized | Distributed across devices |

## Applications

### Mobile Keyboard Prediction
Google's Gboard uses federated learning to improve next-word prediction:
- Each phone trains on the user's typing patterns locally
- Only gradient updates are sent to Google
- User's actual messages never leave the device

### Healthcare
Hospitals train models on patient data without sharing records:
- Rare disease detection across multiple institutions
- Drug interaction prediction
- Medical imaging diagnosis

### Financial Services
Banks collaborate on fraud detection without sharing customer transactions.

### Edge AI
IoT devices and autonomous vehicles learn from local sensor data without uploading everything to the cloud.

## Privacy Enhancements

Federated learning alone doesn't guarantee privacy — gradients can leak information about training data. Additional techniques:

| Technique | What It Does |
|---|---|
| **Differential Privacy** | Add calibrated noise to gradients to prevent data reconstruction |
| **Secure Aggregation** | Encrypt individual gradients so the server only sees the aggregate |
| **Homomorphic Encryption** | Compute on encrypted data without decrypting |
| **Trusted Execution Environments** | Hardware-isolated processing (Intel SGX, ARM TrustZone) |

## Challenges

### Non-IID Data
Each device has different data distributions (e.g., one phone mostly types in English, another in Spanish). This makes averaging gradients less effective than training on a uniform dataset.

### Communication Overhead
Exchanging gradients over many rounds is expensive on bandwidth-limited connections (mobile networks).

### Stragglers
Slow devices hold up the entire round. Solutions: asynchronous aggregation, device sampling.

### Model Poisoning
A malicious participant can send corrupted gradients to sabotage the global model. Byzantine-robust aggregation algorithms help but add complexity.

## Federated Learning and LLMs

Federated learning for [[Large Language Models]] is an active research area:
- **Federated fine-tuning** — Multiple organizations fine-tune a shared base model on private data
- **Federated RLHF** — Collect preference data across organizations without sharing responses
- **Privacy-preserving inference** — Keep prompts and responses private from the API provider

This is particularly relevant for enterprises that want AI capabilities without sending proprietary data to third-party APIs — complementing the [[Open Source AI Models]] approach of running models locally.

## Related

- [[Machine Learning]]
- [[Training and Inference]]
- [[AI Ethics and Safety]]
- [[AI Regulation]]
- [[Fine-Tuning and Alignment]]
