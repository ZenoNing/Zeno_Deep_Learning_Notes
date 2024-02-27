# Diffusion Model Derivation
## forward process

<img src="/2023/v2-d7feccfa52e3c129c31edbfeb085282d_r.png">

<p align="center">Markov Chain</p>

> <p align="center">"The future is independent of the past given the present."</p>

Each step: $q(x_t\ |\ x_{t-1})=\mathcal N(x_t;\ \sqrt{1-\beta_t}x_{t-1},\ {\beta_t}I)$

Which means $x_t=\sqrt{1-\beta_t}x_{t-1}+\sqrt{\beta_t}\epsilon$

Let $\alpha_t=1-\beta_t$, then $x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}\epsilon$
