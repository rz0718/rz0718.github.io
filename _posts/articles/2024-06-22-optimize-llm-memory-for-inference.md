---
title: "Memory Optimization for LLM Inference"
categories:
  - articles
tags:
  - LLM
  - Inference
excerpt: "Less memory for inference"
---

LLM inference is a memory-intensive process which exhibit higher latency and lower throughput. From a high-level perspective, the inference process of LLM (for decoder-only architecture) could be divided into two parts:

1. The first part is the embedding process, where the input text is converted into a sequence of tokens and also compute the intermediate results for the decoding process, i.e., keys and values.
2. The second part is the decoding process, where the model generates the output text. It is an auto-regressive process, which means the model generates the output text token by token.

The decoing process is the most memory-intensive part, where the model needs to store weights, keys, values and activations for all the sequence tokens. And if you are familiar with the KV cache mechanism, you will know that the model needs to store the KV cache for all the sequence tokens. The memory is sacraficed for the speed. Therefore, most of the memory optimization techniques are focused on the decoding process.

In this article, we will discuss three major types of memory optimization techniques for LLM inference:

1. **Weights Optimization**
2. **Attention Mechanism Optimization**
3. **Model Parallelism**

## Weights Optimization

Those techniques are focused on the modification of the model weights to reduce the memory requirements. The most common technique is the quantization, which is the process of reducing the precision of the model weights. The quantization is a trade-off between the memory and the accuracy of the model. From the performance perspective, the quantization would make lose the precision of the values, which would lead to the performance degradation. But from the memory perspective, the quantization would significantly reduce the memory requirements.

The quantization can be applied to the model weights, activations or both. However, there are some GPU which does not have the dedicated hardware support for the lower precision such as INT8 and FP16. Therefore, sometimes, you might experience the longer inference time when you apply the quantization. More details about the quantization can be found here {% include cite.html key="huggingface2024quantization" %}.

## Attention Mechanism Optimization

The core computation in the transformer block is the attention mechanism. And in the original paper, to enhance the capability of the attention mechanism, the authors proposed the multi-head attention mechanism. Multiple attention heads are used to learn projection of the inputs into Q, K and V matrices. One kind of memory optimization techniques is focused on the attention mechanism by reducing the K and V matrices across the attention heads. It includes the following multi-query attention and grouped-query attention. 

### *Multi-Query Attention*

Key and values matrices are shared across the attention heads while the query matrices are still different for each attention head. As as result, the amount of data (keys and values) that need to be stored is reduced.

### *Grouped-query Attention*

Grouped-query attention is a variant of the multi-query attention, where the query matrices are grouped into multiple groups. Each group shares the same key and value matrices. This technique can further reduce the amount of data (keys and values) that need to be stored. We can think GQA is the middle ground between the multi-query attention and the original multi-head attention.

The above approaches can reduce the number of key and values heads that are stored. As a trade-off, the performance of the model would be degraded potentially.

Another kind of memory optimization techniques is focused on a more efficient hardware utilization for the attention mechanism. A few examples are flash attention and paged attention.

### *Flash Attention*

Flash attention is trying to reduce the data transfer between HBM and on-chip SRAM in GPU. Flash attention only loads keys, queries, and values once, fuses the operations of the attention mechanism, and write them back. Not all models support the flash attention. You can refer to the details in this paper {% include cite.html key="dao2022flash" %}.

### *Paged Attention*

Paged attention is focused on KV cache management. Paged attention firstly partitionzed the KV cache into blocks and then, accessed via a lookup table. Via this technique, the KV cache does not need to be stored in contiguous memory,.The memory efficiency can increase GPU utilization on memory-bound workloads. The details could be found in this project {% include cite.html key="vllm" %}.

## Model Parallelism

The model parallelism is a technique that distributes the model over multiple devices. By spreading the memory and compute footprint, larger models can be deployed. There are several ways of parallelizing the model based on how the model weights are split including the tensor parallelism, pipeline parallelism and sequence parallelism.

### *Pipeline Parallelism*

Pipeline parallelism is to split the model vertically. Each chunk would contain a stack of sub-layers that is executed on a separted device. The model is sequentially partitioned and a subset of all layers are executed on each device. The outputs of a group of operations on one device are passed to the next, which continues executing the subsequent chunk.

The main limitation of this method is that, due to the sequential nature of the processing, some devices or layers may remain idle while waiting for the output (activations, gradients) of previous layers.

### *Tensor Parallelism*

Tensor parallelism is a technique to split the individual layers of the model into multiple parts which can be executed in parallel via mulitple GPUs. In the context of LLM, the major computation blocks that can be optimized via tensor parallelism are the self-attention and the feed-forward layers. For example, when multiplying the input tensors with the first weight tensor, the matrix multiplication is equivalent to splitting the weight tensor column-wise, multiplying each column with the input separately, and then concatenating the separate outputs. Than, for self-attention, the split could be done on the head dimension naturally.

The limitation of this method is that, it can only be applied to a few operations in the LLM which can be divided into independent, manageable blocks. However, layernorm and dropout are not suitable for this method while they also occupy a considerable amount of memory to store activations.

### *Sequence Parallelism*

To sovle the limitation of the tensor parallelism, sequence parallelism is proposed. Sequence parallelism is to split the model across the sequence dimension for the above operations including dropout and layernorm. Those operations can be partitioned along the sequence dimension and make them more memory-efficient.

## References

{% include references.html keys="nvidia2023mastering,escobar2023memory,huggingface2024memory,huggingface2024quantization,dao2022flash,vllm" %}



