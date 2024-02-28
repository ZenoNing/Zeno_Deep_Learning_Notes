# Diffusion Model Derivation

## 1. forward process

<img src="/2023/v2-d7feccfa52e3c129c31edbfeb085282d_r.png">

<p align="center">Markov Chain</p>

> <p align="center">"The future is independent of the past given the present."</p>

Each step: $q(x_t\ |\ x_{t-1})=\mathcal N(x_t;\ \sqrt{1-\beta_t}x_{t-1},\ {\beta_t}I)$

Which means $x_t=\sqrt{1-\beta_t}x_{t-1}+\sqrt{\beta_t}\epsilon$

Let $\alpha_t=1-\beta_t$, then $x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}\epsilon$

$$x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}\epsilon=\sqrt{\alpha_t}(\sqrt{\alpha_{t-1}}x_{t-2}+\sqrt{1-\alpha_{t-1}}\epsilon)+\sqrt{1-\alpha_t}\epsilon=\sqrt{\alpha_t}\sqrt{\alpha_{t-1}}x_{t-2}+\sqrt{\alpha_t}\sqrt{1-\alpha_{t-1}}\epsilon+\sqrt{1-\alpha_t}\epsilon$$

According to the Additive property of a Normal Distribution, $$X_1\propto \sqrt{\alpha_t}\sqrt{1-\alpha_{t-1}}\epsilon = \mathcal{N}(0,\ \alpha_t(1-\alpha_{t-1})),$$

$$X_2\propto \sqrt{1-\alpha_t}\epsilon = \mathcal{N}(0,\ 1-\alpha_t)$$

So, $$X=X_1+X_2=\mathcal{N}(0,\ \alpha_t-\alpha_t \alpha_{t-1}+1-\alpha_t)=\mathcal{N}(0,\ 1-\alpha_t \alpha_{t-1})$$

So let $x_t=\sqrt{\alpha_t \alpha_{t-1}}x_{t-2}+\sqrt{1-\alpha_t \alpha_{t-1}}\epsilon$

According to Mathematical Induction, $$x_t=\sqrt{\alpha_t \alpha_{t-1} \ldots \alpha_1}x_0+\sqrt{1-\alpha_t \alpha_{t-1} \ldots \alpha_1}\epsilon$$

Let $\bar{\alpha_t}=\alpha_t \alpha_{t-1} \ldots \alpha_1$, so $x_t=\sqrt{\bar{\alpha_t}}x_0+\sqrt{1-\bar{\alpha_t}}\epsilon$

Which means we can directly get $x_t$ from $x_0$ using only one step.

# 2. reverse step

We must predict $x_{t-1}$ from $x_t$, using Bayes' theorem.

Conditional Probability:


