![[Pasted image 20240203213326.png]]
### Architecture
- based on transformers [paper](https://arxiv.org/abs/1706.03762) with some additional improvements
1. RMSNorm [1910.07467.pdf (arxiv.org)](https://arxiv.org/pdf/1910.07467.pdf) (from GPT3)
	- RMSNorm only focuses on re-scaling invariance because they believe that LayerNorm only worked because of this rather than re-centering invariance.
	- so the paper proposes technique to regularize the summed inputs simply according to the root mean squared (RMS) statistic
	- $\bar{a_{i}} = \frac{a_i}{RMS(a)}g_i$, where $RMS(a) = \sqrt{\frac{1}{n}\sum^n_{i=1}a^2_i}$
	- RMSNorm is basically LayerNorm but without the means
2. Rotary Embeddings [2104.09864v5.pdf (arxiv.org)](https://arxiv.org/pdf/2104.09864v5.pdf) (from GPTNeo)
	- RoPE encodes the absolute position with a rotation matrix and meanwhile incorporates the explicit relative position dependency in self-attention formulation.
3. SwiGLU [2002.05202.pdf (arxiv.org)](https://arxiv.org/pdf/2002.05202.pdf) 
	- $SwiGLU(x, W, v, b, c, /beta) = Swish_{\beta}(xW + b) \otimes (xV + c)$ 
	- ```def swish(x, b): return x * sigmoid(b*x)```

## Optimizer
- AdamW
- $\beta_{1}=0.9, \beta_2=0.95$ 
- cosine learning scheduler
- weight_decay = 0.1 & gradient_clipping = 0.1

![[Pasted image 20240203213226.png]]

