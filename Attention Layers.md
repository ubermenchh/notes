[video](https://youtu.be/2TT384U4vQg?si=HLHsuwNeh91d4BZh)
# Self-Attention
$$ Attention(Q, K, V) = softmax(\frac{QK^T}{\sqrt{d_{k}}})V$$

- Quadratic compute and memory complexity with respect to the input sequence length
- Inference with long sequence becomes very expensive.

# Multi-Head Attention

![](assets/Pasted%20image%2020240307094427.png)
**Algorithm**:
- Matrices $Q, K, V \in R^{N \times d}$ 
	- Load Q, K by blocks from HBM, compute $S = QK^T$, write S to HBM.
	- Read S from HBM, compute $P = softmax(S)$, write P to HBM.
	- Load P and V by blocks from HBM, compute $O = PV$, write O to HBM.
	- Return O.
- N: sequence length, d: embedding length, h: num of heads.
- Q, K, V and intermediate dot-product results (aka K, V cache) are stored in High Bandwidth Memory (HBM).
- Quadratic complexity for HBM access with respect to sequence length
- Memory becomes a bottleneck.
```
Each self-attention layer has its own set of values and keys.
```
# Multi-Query Attention (MQA)
- [paper](https://arxiv.org/abs/1911.02150) 
```
All self-attention layers share the same set of values and keys.
```
![](assets/Pasted%20image%2020240307095409.png)
- implemented in Falcon 7B
- smaller KV cache (10-100x)
- less pressure on memory and 12x faster decoding during inference
- reduced memory usage: batch size can be increased
- small accuracy drop
- models must be trained with MQA
- Tensor Parallelism requires KV replication

# Grouped Query Attention (GQA)
- [paper](https://arxiv.org/pdf/2305.13245.pdf)
![](assets/Pasted%20image%2020240307095843.png)
- implemented in LLaMa2 and Mistral
- Attention head groups share the same of keys and values
- Good compromise between speed and accuracy: almost as accurate as MHA and almost as fast as MQA
- MHA models can be uptrained to GQA
- better fit for tensor parallelism

# Sliding Window Attention
- [longformer](https://arxiv.org/abs/2004.05150v2), [mistral](https://arxiv.org/pdf/2310.06825v1.pdf)
![](assets/Pasted%20image%2020240307133036.png)
- SWA limits attention to a fixed window (4096 tokens)
- a token can only see window_size tokens from the prev layer 
- max theoretical context size = window_size * n_layers (131K)
- attention complexity is reduced from quadratic to linear

# Flash Attention
- [paper](https://arxiv.org/abs/2205.14135)
- Avoid reading and writing the attention matrix from and to HBM
	- Load Q and K from HBM once
	- Multiply Q and K, keep S in SRAM
	- Compute P incrementally in SRAM (tiling)
	- Write P back to HBM
- Parallelize over batch size and number of heads

- N: seq length, d: embedding length, M: size of SRAM (d<=M<=Nd)
- Flash Attention requires $O(N^2d^2M^{-1})$ HBM accesses
- $M=N:O(Nd^2)$ HBM accesses
- Memory complexity is now linear: 2-4x faster, 10-20x memory savings
- Both the forward and backward passes are optimized to accelerate training

# Flash Attention 2
- [paper](https://arxiv.org/abs/2307.08691)
- Reduce the number of non-matmul operations to maximize GPU throughput
- Optimizer operations for Multi-Query Attention and Grouped-Query Attention
- Increased parallelism (accros sequence length)
- 2x faster than Flash Attention, up to 9x faster than standard Attention

# Paged Attention
- [paper](https://arxiv.org/abs/2309.06180)
- the KV cache memory grows and shrinks dynamically for each inference request
- GPU memory fragmentation wastes memory and makes it difficult to increase batch size
- Paged Attention divided KV cache into fixed-size memory-aligned blocks (pages), similar to virtual memory pages in operating systems
- Allocating pages reduces internal and external memory fragmentation
- vLLM - [link](https://github.com/vllm-project/vllm)
