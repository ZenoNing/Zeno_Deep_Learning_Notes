# Diffusion Model Derivation

## 1. forward process

<div align="center"><img src="/2023/v2-d7feccfa52e3c129c31edbfeb085282d_r.png"></div>

<p align="center">Markov Chain</p>

> <p align="center">"The future is independent of the past given the present."</p>

Each step: $q(x_t|x_{t-1})=\mathcal N(x_t;\ \sqrt{1-\beta_t}x_{t-1},\ {\beta_t}I)$

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

<div align="center"><img src="/2023/Conditional_Probability.png" width="60%" height="60%"></div>

$P(A|B)=\cfrac{P(AB)}{P(B)}$, Similarly, $P(B|A)=\cfrac{P(AB)}{P(A)}$.

Thus, we can get to Bayes' theorem: $$P(B|A)=\cfrac{P(A|B)P(B)}{P(A)}$$

$P(B|A)$: Posterior Probability

$P(A|B)$: Likelihood

$P(B)$: Prior Probability

$P(A)$: Marginal Probability

According to Bayes' theorem, we can get: $$P(x_{t-1}|x_t)=\cfrac{P(x_t|x_{t-1})P(x_{t-1})}{P(x_t)}$$

Further, on the condition of known $x_0$:
$$P(x_{t-1}|x_t,\ x_0)=\cfrac{P(x_t|x_{t-1},x_0)P(x_{t-1}|x_0)}{P(x_t|x_0)}$$

According to Markov Chain's property (The future is independent of the past given the present): $P(x_{t-1}|x_t,\ x_0)=P(x_{t-1}|x_t)$

Thus, $$P(x_{t-1}|x_t)=\cfrac{\mathcal{N}(\sqrt{\alpha_t}x_{t-1},\ 1-\alpha_t) \mathcal{N}(\sqrt{\bar{\alpha_{t-1}}}x_0,\ 1-\bar{\alpha_{t-1}})}{\mathcal{N}(\sqrt{\bar{\alpha_t}}x_0,\ 1-\bar{\alpha_t})}$$

According to Normal Distribution: $\mathcal{N}=\cfrac{1}{\sqrt{2\pi}\sigma}e^{-\cfrac{(x-\mu)^2}{2\sigma^2}}$

$$P(x_{t-1}|x_t,\ x_0)=A\exp(-\cfrac{1}{2}\cdot
[\cfrac{(x_t-\sqrt{\alpha_t}x_{t-1})^2}{1-\alpha_t}+
\cfrac{(x_{t-1}-\sqrt{\bar{\alpha_{t-1}}}x_0)^2}{1-\bar{\alpha_{t-1}}}-
\cfrac{(x_{t}-\sqrt{\bar{\alpha_{t}}}x_0)^2}{1-\bar{\alpha_t}}])$$

(Note: $A$ is the index that we don't much care.)

Transform the above equation, as we need to focus on $x_{t-1}$ (we only care about $x_{t-1}$, $x_t$ and $x_0$ are known distribution):

$$P(x_{t-1}|x_t,\ x_0)=A\exp(-\cfrac{1}{2}\cdot[
(\cfrac{\alpha_t}{1-\alpha_t}+\cfrac{1}{1-\bar{\alpha_{t-1}}})x_{t-1}^2-
(\cfrac{2\sqrt{\alpha_t}x_t}{1-\alpha_t}+\cfrac{2\sqrt{\bar{\alpha_{t-1}}}x_0}{1-\bar{\alpha_{t-1}}})x_{t-1}+
C(x_t, x_0)
])$$

Since $P(x_{t-1}|x_t,\ x_0)$ should be a Normal Distribution, its format is like: $$\exp(-\cfrac{x^2+\mu^2-2\mu x}{2\sigma^2})$$

Thus, $$\cfrac{1}{\sigma^2}=\cfrac{\alpha_t}{1-\alpha_t}+\cfrac{1}{1-\bar{\alpha_{t-1}}}=\cfrac{1-\bar{\alpha_t}}{(1-\alpha_t)(1-\bar{\alpha_{t-1}})},\ 
\sigma^2=\cfrac{(1-\alpha_t)(1-\bar{\alpha_{t-1}})}{1-\bar{\alpha_t}} $$

plug it in $$\mu=\cfrac{1}{2}\sigma^2(\cfrac{2\sqrt{\alpha_t}}{1-\alpha_t}x_t+\cfrac{2\sqrt{\bar{\alpha_{t-1}}}}{1-\bar{\alpha_{t-1}}}x_0)$$

We can get $$\mu=\cfrac{(1-\bar{\alpha_{t-1}})\sqrt{\alpha_t}}{1-\bar{\alpha_t}}x_t+\cfrac{(1-\alpha_t)\sqrt{\bar{\alpha_{t-1}}}}{1-\bar{\alpha_t}}x_0$$

Since $x_t=\sqrt{\bar{\alpha_t}}x_0+\sqrt{1-\bar{\alpha_t}}\epsilon$, we can substitute $x_0=\cfrac{x_t-\sqrt{1-\bar{\alpha_t}}\epsilon}{\sqrt{\bar{\alpha_t}}}$ into the above equation:

$$
\begin{align}
\mu &= \cfrac{(1-\bar{\alpha_{t-1}})\sqrt{\alpha_t}}{1-\bar{\alpha_t}}x_t+\cfrac{(1-\alpha_t)\sqrt{\bar{\alpha_{t-1}}}}{1-\bar{\alpha_t}}\cdot \cfrac{x_t-\sqrt{1-\bar{\alpha_t}}\epsilon}{\sqrt{\bar{\alpha_t}}}\\
&= \cfrac{(1-\bar{\alpha_{t-1}})\sqrt{\alpha_t}}{1-\bar{\alpha_t}}x_t+\cfrac{1-\alpha_t}{(1-\bar{\alpha_t})\sqrt{\alpha_t}}xt-\cfrac{1-\alpha_t}{\sqrt{1-\bar{\alpha_t}}\sqrt{\alpha_t}}\epsilon\\
&= \cfrac{1}{\sqrt{\alpha_t}}\left(\cfrac{\alpha_t-\bar{\alpha_t}+1-\alpha_t}{1-\bar{\alpha_t}}x_t-\cfrac{1-\alpha_t}{\sqrt{1-\bar{\alpha_t}}}\epsilon\right)\\
&= \cfrac{1}{\sqrt{\alpha_t}}\left(x_t-\cfrac{1-\alpha_t}{\sqrt{1-\bar{\alpha_t}}}\epsilon\right)
\end{align}
$$

In $\sigma$ and $\mu$, there's nothing we don't know but $\epsilon$, that's why we need to train a Diffusion Model to predict it, completing the reverse process.
