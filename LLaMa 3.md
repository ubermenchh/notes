# RMSNorm

- a vanilla network might suffer from ***Internal Covariate Shift*** issue, where the current layer's input distribution changes as previous layers are updated.
- to reduce this LayerNorm normalizes the inputs to fix their mean and std,
$$ \bar{a_i} = \frac{a_i - \mu}{\sigma}g_i $$
$$ y_i = f(\bar{a_i} + b_i) $$ RMSNorm,
$$ \bar{a_i} = \frac{a_i}{RMS(a)}g_i $$
$$ RMS(a) = \sqrt{\frac{1}{n}\sum_{i=1}^{n}a^2_i} $$
- RMSNorm simplifies LayerNorm by totally removing the mean stat
- when mean of summed inputs is zero, RMSNorm $\Leftrightarrow$ LayerNorm 
- RMSNorm does not re-center summed inputs as in LayerNorm