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
