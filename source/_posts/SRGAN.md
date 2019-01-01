---
title: SRGAN
tags:
  - Image Super-Resolution
categories:
  - GAN
mathjax: true
date: 2018-12-04 18:58:20
---

[Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network](https://arxiv.org/pdf/1609.04802.pdf)
--Learning Notes
<!--more-->

# Intro
One of SR problem is that texture detail in the reconstructed SR images is typically absent
SR algorithms is commonly the minimization of the mean squared error(MSE) between the recovered HR image and the ground truth(GT)
This is convenient as minimizing MSE also maximizes the peak signal to nose ratio(PSNR), which is a common measure used to evaluate and compare SR algorithms
However,to capture high texture detail is very difficult as they are defined based on pixel-wise image difference
![](https://i.imgur.com/t7l8oHd.png)

## Perceptual Loss Function
perceptual loss \\(l^{SR}\\) is critical for the performance of our generator
\\(l^{SR}\\) is modeled based on MES(mean square error)
we define it as:
$$
\begin{equation} \label{eq1}
l^{SR} = \underbrace{l^{SR}_X}_\text{content loss} + \underbrace{10^{-3}l^{SR}_{GEN}}_\text{adverserial loss}
\end{equation}
$$

### Content loss
The pixel-wise MSE loss:
$$
\begin{equation} \label{eq2}
l^{SR}_{MSE}=\frac{1}{r^2WH}\sum_{x=1}^{rW}\sum_{y=1}^{rH}(I^{HR}_{x,y}-G_{\theta_G}(I^{LR})_{x,y})^2
\end{equation}
$$

Instead of relying on pixel-wise losses, we define VGG loss. With \\(\phi_{i,j}\\) we indicate the feature map obtained by the j-th convolution before the i-th maxpooling layer in VGG19.
Then define loss as euclidean distance between feature of reconstructed image \\(G_{\theta_G}(I^{LR})\\) and \\(I^{HR}\\)
$$
\begin{equation} \label{eq3}
l^{SR}_{VGG/i.j} = \frac{1}{W_{i,j}H_{i,j}}\sum_{x=1}^{W_{i,j}}\sum_{y=1}^{H_{i,j}}(\phi_{i,j}(I^{HR})_{x,y}-\phi_{i,j}(G_{\theta_G}(I^{LR}))_{x,y})^2
\end{equation}
$$
### Adversarial loss
The generative loss \\(l^{SR}\\) is defined based on the probabilities of the discriminator \\(D_{\theta_D}(G_{\theta_G}(I^{LR}))\\)
$$
\begin{equation} \label{eq4}
l^{SR}_{Gen}=\sum_{n=1}^N -log\ D_{\theta_D}(G_{\theta_G}(I^{LR}))
\end{equation}
$$