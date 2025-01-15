---
title: "How much memory do we need for LLM?"
categories:
  - articles
tags:
  - LLM
  - Memory
excerpt: "Memory requirements for LLM"
---

LLM is super computationally intensive depending on the model size. To run LLM locally and without powerful GPU, we need to consider the memory requirements for LLM. And for the same model, the memory requirement would be different for inference and training. In the following, we will discuss these two cases, separately. And we will also discuss the potential ways to reduce the memory requirements for LLM.


Before the detaild disscussion, there is a simple equation to estimate the memory requirements for LLM:

| Use Case | Memory Requirement |
| --- | --- |
| Inference | number of parameters * precision |
| Training/Fine-tuning | 4-6 times the inference memory |

Here, the precision is the size of the data type used to store the model parameters. The reference table is as follows:

| Precision | Size | Description |
| --- | --- | --- |
| float32 | 4 Bytes | 32-bit floating point |
| float16 | 2 Bytes | 16-bit floating point |
| int8 | 1 Byte | 8-bit integer |
| int4 | 0.5 Bytes | 4-bit integer |

## Inference

For inference, the memory required for LLM come from the following three components:

### Model Size

We need to load the model into the memory. It is the major memory consumer which will depend on the model size and the precision. It would be the multiplication of the number of parameters and the precision.

$$
\text{Model Size} = \text{Number of Parameters} \times \text{Precision}
$$


### KV Cache

KV Cache is used to store the key-value pairs for the attention mechanism. It is used to speed up the inference in the decoding phase. The technique to cache the previous Keys and Vaules and only calcuate the attention for the new token. It would lead to much faster matrix multiplcations and make the inference faster. Those cached keys and values would occupy the additional memory.

It can be quantified by the following equation:

$$
\begin{aligned}
\text{KV Cache} = & 2 \times \text{Batch Size} \times \text{Seq. Length} \\
& \times \text{Precision} \times \text{Number of Layers} \\
& \times \text{Hidden Size}
\end{aligned}
$$

Here, if we are familiar with the transformer architecture, we would notice that the hidden size is the multiple of the number of attention heads and the head size.


### Activation Memory

Activation memory comes from the cost of any tensors (“activations”) which need to be saved to perform forward pass during inference and the backward pass during training. It is the outputs of each layer in the neural network. They should be in float32 precision. And compared to the previous two, this is more complex. In LLM, the basic layer is the transformer layer. Therefore, the activation memory would come from two parts: 1. the output of the attention layer and the feedforward layer. 2. the self-attention scores. Therefore, the activation memory can be quantified by the following equation:

$$
\begin{multline}
\text{Activation Memory} = \alpha \times \text{Batch Size} \times \text{Seq. Length} \times \text{Hidden Size} \\
+ \beta \times \text{Batch Size} \times \text{number of attention heads} \times \text{Seq. Length}^2
\end{multline}
$$

The alpha and beta would depend on the model architecture and implementation choices. For example, in a vanilla transformer, we would have $$\alpha=34$$ and $$\beta=5$$. While in a transformer with flash attention, $$\beta=0$$ and $$\alpha=2$$.


## Training

For training, the memory required for LLM would be the sum of the model size, the KV cache, and the activation memory plus two new components including optimizer states and gradient memory.

$$
\begin{split}
\text{Memory Required} = & \text{Model Size} + \text{KV Cache} \\
& + \text{Activation Memory} \\
& + \text{Optimizer States} \\
& + \text{Gradient Memory}
\end{split}
$$

### Optimizer States

For neural network, optimization algorithms require resources to store the associated parameters. Therefore, the memory required here depends on the optimizer which will decide the number of states and their precision. For example, Adam optimizer requires two states, the first moment and the second moment, which are in float32 precision while SGD optimizer requires only one state, the gradient, which is in float32 precision. Therefore, the memory required for optimizer states would be the multiplication of the number of states and the precision and the number of parameters. 

$$
\text{Optimizer States} = \text{Number of States} \times \text{Precision} \times \text{Number of Parameters}
$$

In most cases, the optimizer states would be in float32 precision. Therefore, for Adam optimizer, the memory required would be: 8 bytes * number of parameters while SGD optimizer would be 4 bytes * the number of parameters.


### Gradient Memory

In backpropagation, gradient values would be computed and stored. And during optimization, they must be stored in float32 to maintain numerical stability. Therefore, the memory required for gradient memory would be as follows:

$$
\text{Gradient Memory} = \text{Number of Parameters} \times 4 Bytes
$$

There is an open source project called [llm-memory](https://llm-system-requirements.streamlit.app/) which can help you to estimate the memory requirements for LLM.

## References

{% include references.html keys="nvidia2023mastering,escobar2023memory,huggingface2024memory" %}



