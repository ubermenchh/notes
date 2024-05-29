# RMSNorm

- a vanilla network might suffer from ***Internal Covariate Shift*** issue, where the current layer's input distribution changes as previous layers are updated.
- to reduce this LayerNorm normalizes the inputs to fix their mean and std,
$$ \bar{a_i} = \frac{a_i - \mu}{\sigma}g_i $$
$$ y_i = f(\bar{a_i} + b_i) $$ 