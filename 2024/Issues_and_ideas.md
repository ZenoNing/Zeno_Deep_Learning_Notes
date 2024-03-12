# Issues

## Issue1 MAT's training process

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_LaMa.png"></div>

Training this model  requires high performance computational resources. According to the Author's statement in their GitHub's repository [<sup>1</sup>](#refer-anchor-1) , training on CelebA-HQ512 datasets [<sup>2</sup>](#refer-anchor-2) (which contains 24,813 training images) needs to cost 8 Nvidia V100 GPUs for 4-5 days. 

In our tentative try, using 2 Nvidia 2080ti GPUs, it costs about 40 days to process 1/3 of the training. In Author's default configuration, `--batch_size=32`, however, because of the VRAM's limitation, we can only set `--batch_size=2`, to let the programme work properly. 

In our approach, the FID metric coverages to 4.9 . We haven't tried to train this model on 256x256 image resolution yet. 

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_MAT.png"></div>

<div align="center"><img src="https://raw.githubusercontent.com/ZenoNing/Zeno_Deep_Learning_Notes/main/2024/Architecture_RePaint.png"></div>


# References

<div id="refer-anchor-1"></div>

- [1] [MAT: Mask-Aware Transformer for Large Hole Image Inpainting](https://github.com/fenglinglwb/MAT/issues/23)

<div id="refer-anchor-1"></div>

- [2] [Large-scale CelebFaces Attributes (CelebA) Dataset]([https://github.com/fenglinglwb/MAT/issues/23](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)
