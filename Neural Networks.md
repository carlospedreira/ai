# Neural Networks

Neural networks are computational models inspired by the structure of biological neurons. They are the foundation of [[Deep Learning]] and modern [[Artificial Intelligence]].

## Structure

A neural network consists of layers of interconnected nodes (neurons):

```
Input Layer → Hidden Layer(s) → Output Layer
```

Each connection has a **weight**, and each neuron has a **bias**. The network processes data by:

1. Multiplying inputs by weights
2. Summing the results and adding bias
3. Passing the sum through an **activation function**
4. Forwarding the output to the next layer

## Activation Functions

Activation functions introduce non-linearity, allowing the network to learn complex patterns:

- **ReLU** (Rectified Linear Unit) — `max(0, x)` — most common in hidden layers
- **Sigmoid** — Maps values to (0, 1) — used in binary classification
- **Softmax** — Converts outputs to probabilities — used in multi-class classification
- **Tanh** — Maps values to (-1, 1)
- **GELU** — Used in [[Transformers]]

## Training

Networks learn through **backpropagation** and **gradient descent**:

1. **Forward pass** — Input flows through the network to produce an output
2. **Loss calculation** — Compare output to expected result using a loss function
3. **Backward pass** — Compute gradients of the loss with respect to each weight
4. **Weight update** — Adjust weights in the direction that reduces loss

This cycle repeats over many iterations (**epochs**) across the training data. See [[Training and Inference]] for more.

## Types

- **Feedforward** — Information flows in one direction
- **Convolutional (CNNs)** — Use filters to detect spatial patterns ([[Computer Vision]])
- **Recurrent (RNNs)** — Have loops for sequential data (largely replaced by [[Transformers]])
- **Transformer-based** — Use attention mechanisms ([[Transformers]])

## Key Concepts

- **Parameters** — The weights and biases the network learns
- **Hyperparameters** — Settings chosen by the developer (learning rate, layer count, etc.)
- **Dropout** — Randomly disabling neurons during training to prevent overfitting
- **Batch normalization** — Normalizing layer inputs to stabilize training

## Related

- [[Deep Learning]]
- [[Training and Inference]]
- [[Transformers]]
