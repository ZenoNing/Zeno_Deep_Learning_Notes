# Issues

## Issue1 MAT's training process

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_MAT.png"></div>

Training this model  requires high-performance computational resources. According to the Author's statement in their GitHub's repository [<sup>1</sup>](#refer-anchor-1), training on CelebA-HQ512 datasets [<sup>2</sup>](#refer-anchor-2) (which contains 24,813 training images) needs to cost 8 Nvidia V100 GPUs for 4-5 days. 

In our tentative try, using 2 Nvidia 2080ti GPUs, it costs about 40 days to process 1/3 of the training. In Author's default configuration, `--batch_size=32`, however, because of the VRAM's limitation, we can only set `--batch_size=2`, to let the programme run properly. 

In our approach, the FID metric [<sup>5</sup>](#refer-anchor-5) converges to 4.9 . We haven't tried to train this model on 256x256 image resolution yet. 

## Issue2 RePaint's training process

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_RePaint.png"></div>

According to the paper and the Author's statement in their GitHub repository [<sup>3</sup>](#refer-anchor-3), This approach is an inference scheme. They do not train or finetune the diffusion model but condition pre-trained models. 

Based on the above reason, we have found their pre-trained models on guided-diffusion [<sup>4</sup>](#refer-anchor-4) and want to train on CelebA-HQ256 datasets [<sup>2</sup>](#refer-anchor-2) with guided-diffusion's [<sup>4</sup>](#refer-anchor-4) released code to evaluate it's performance using FID [<sup>5</sup>](#refer-anchor-5) and LPIPS [<sup>6</sup>](#refer-anchor-6) metrics.

However, training on their pre-trained models requires high-performance computational resources. Basically, that's due to the Author's configuration and VRAM's limitation. If we want to train on their pre-trained models, we need to follow their configurations entirely, otherwise, the program won't run properly and we will get an error of mismatching between our custom configuration and the pre-trained models' architecture. 

After plenty of debug processes, we have found we passed the matching of hyperparameters. However, we got the next error of CUDA out of memory. Basically, this error means the VRAM is not sufficient for loading the enormous hyperparameters, or the datasets. With the ablation study, we found this error is not due to the datasets since we have already tried to set the `--batch_size=1` and it still didn't work. 

After further investigation, we found that this is basically due to the hyperparameter `--num_channels=256`. If we set the `--num_channels=64` and train the model not using their pre-trained models but from scratch, the program will run properly. However, without using their pre-trained models, it's theoretically impossible to implement the same results as they did.

## Issue3 LaMa's training process

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_LaMa.png"></div>

We have already started training this model on CelebA-HQ256 datasets [<sup>2</sup>](#refer-anchor-2) thanks to their released code [<sup>7</sup>](#refer-anchor-7), pre-trained models, and detailed configurations (I must say the Authors are very kind people since they almost answer any kind of issues in their repository and find the solution for the questioners. Based on their tireless efforts, the entire repository has now become quite complete). My highest respect and appreciation!  **At this moment, the whole model is in training.**
 
# Ideas

**The backbone paper: Scalable Diffusion Models with Transformers [<sup>10</sup>](#refer-anchor-10)**

## Transformers:

disadvantages:

a great many of parameters; requirement of large amounts of training data; slow convergence process; have to introduce an extra part which called StyleGAN to get pluralistic generation

advantages:

remarkable scaling properties under increasing model size, training compute and data; efficiently handle dependencies across larger distances (which denotes extracting global feature more efficiently in inpainting tasks, compared to UNet based on CNN architecture)

## Diffusion Models:

disadvantages:

staying intact of the UNet design, The architecture still has the potential to be developed and enhanced

advantages:

inherent pluralistic generation; high generative quality

*The knowledge of Classifier-free guidance [<sup>8</sup>](#refer-anchor-8)* 

The original loss prediction in DDPM:

$$\epsilon_\theta (x_t, t)$$

The loss, AKA the optimization objective:

$$\mathcal{L}=MSE{\Vert \epsilon_t-\epsilon_\theta (x_t, t)\Vert^2}$$

Classifier guidance diffusion [<sup>4</sup>](#refer-anchor-4):

$$\epsilon_\theta (x_t, t, c)$$

Where $c$ denotes prior constraints (in the original paper, it is class condition; in inpainting task, it should be the known parts of the image)

$$\hat{\epsilon_\theta} (x_t, t, c)=(1-w)\epsilon_\theta (x_t, t, c)-w\epsilon_\theta (x_t, t)$$

(we can use this formula to control the constraints so that we can make a trade-off between diversity, which means emphasize the class; and creativity. *how to deploy it into inpainting tasks: unknown*)

*The knowledge of LDM (Latent Diffusion Models)[<sup>9</sup>](#refer-anchor-9)*

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_LDM.png"></div>

The idea is pretty simple but worked, just to use variational auto-encoder (VAE) architecture, compressing the original image's dimension into a latent space, and do diffusion process and reverse process in the latent space so that we can not only saving computational resources, but also make the computation of training diffusion models not prohibitive.

**IDEA**: In the DiT [<sup>10</sup>](#refer-anchor-10) paper, a way to replace the Latent Diffusion Models' classical UNet architecure by transformer blocks has been found. Until now there's no worked implementations of inpainting task with this architecture. We can try to explore the possibility to deploy this novel architecture into inpainting tasks and do some optimization to fit the task. 

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_DiT.png"></div>

How to fit the inpainting task (future experiment study):

1. Using Multi-head contextual attention (MCA) [<sup>1</sup>](#refer-anchor-1)to replace the Multi-head self-attention, conducts longrange dependency modeling efficiently by exploiting valid tokens, indicated by a dynamic mask.
2. Using resample scheme [<sup>3</sup>](#refer-anchor-3), to make the inpainted part semantically correct

# References

<div id="refer-anchor-1"></div>

- [1] [MAT: Mask-Aware Transformer for Large Hole Image Inpainting](https://github.com/fenglinglwb/MAT/issues/23)

<div id="refer-anchor-2"></div>

- [2] [Large-scale CelebFaces Attributes (CelebA) Dataset](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)
  
<div id="refer-anchor-3"></div>

- [3] [RePaint: Inpainting using Denoising Diffusion Probabilistic Models](https://github.com/andreas128/RePaint?tab=readme-ov-file#details-on-data)

<div id="refer-anchor-4"></div>

- [4] [Diffusion Models Beat GANS on Image Synthesis](https://github.com/openai/guided-diffusion)
  
<div id="refer-anchor-5"></div>

- [5] [GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium](https://papers.nips.cc/paper_files/paper/2017/hash/8a1d694707eb0fefe65871369074926d-Abstract.html)
  
<div id="refer-anchor-6"></div>

- [6] [The Unreasonable Effectiveness of Deep Features as a Perceptual Metric](https://github.com/richzhang/PerceptualSimilarity)
  
<div id="refer-anchor-7"></div>

- [7] [LaMa: Resolution-robust Large Mask Inpainting with Fourier Convolutions](https://github.com/advimman/lama)

<div id="refer-anchor-8"></div>

- [8] [Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598)

<div id="refer-anchor-9"></div>

- [9] [High-Resolution Image Synthesis with Latent Diffusion Models](https://github.com/CompVis/latent-diffusion)

<div id="refer-anchor-10"></div>

- [10] [Scalable Diffusion Models with Transformers](https://github.com/facebookresearch/DiT)
