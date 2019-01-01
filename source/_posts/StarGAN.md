---
title: StarGAN
tags:
  - GAN
categories:
  - Computer Vision
mathjax: true
date: 2018-11-28 21:59:09
---

[StarGAN: Unified Generative Adversarial Networks
for Multi-Domain Image-to-Image Translation](https://arxiv.org/pdf/1711.09020.pdf)
-- Learning Notes


<!--more-->
# StarGAN

Cross-domain models need \\(k(k-1)\\) generators have to be trained
![](https://i.imgur.com/qsSPhf3.png)
StarGAN, a framework to address multi-domain image to image translation

## Multi-domain Image to Image
### Generator

\\(G(x,c)\rightarrow y\\)
target domain label c,x input,y output
randomly generate so G learns flexibly

### Discriminator
produces probability distributions both source and domain labels
\\(D: x\rightarrow {D_{src}(x),D_{cls}(x)}\\)

### Adversarial Loss

$$L_{adv}=E_x[logD_{src}(x)]+E_{x,c}[log(1-D_{src}(G(x,c)))]$$
where G generates an image G(x,c) condintion on c,
disriminator tries to maximize Loss,while generator tries to minimize

### Domain Classification Loss
our goal is to translate x to y in target domain c,
a domain classification loss of real images used to optimize D,
$$ L^r_{cls}=E_{x,c'}[-logD_{cls}(c'|x)] $$
original domain c'
for fake image uses to optimize G
$$ L^f_{cls}=E_{x,c}[-logD_{cls}(c|G(x,c))] $$
In other words, G tries to minimize this objective to generate
images that can be classified as the target domain c

### Reconstruction Loss
Because we cant guarantee that the image preserve the content of its inputs while changing only the domain-related part.
we apply a cycle consistency loss,
$$L_{rec}=E_{x,c,c'}[||x-G(G(x,c),c')||_1]$$

### Full Objective
$$L_D=-L_{adv}+\lambda_{cls}L^r_{cls}$$
$$L_G=L_{adv}+\lambda_{cls}L^f_{cls}+\lambda_{rec}L_{rec}$$
\\(\lambda_{cls}\\) and \\(\lambda_{rec}\\) are hyperparameter
control the relative importance between cls and rec Loss
The paper uses \\(\lambda_{cls}=1\\) and \\(\lambda_{rec}=10\\) for implementaion


![](https://i.imgur.com/mQUTQzv.png)

## Multi dataset
we use CelebA and RaFD, the problem is we can find labels such as hair color or gender in the former,
but no facial expressions such as 'happy','angry' vice versa.

### Mask Vector
a unifide version of labels as a vector
$$\widetilde{c}= [c1, ..., cn, m]$$
\\(c_i\\) represent vector for i-th dataset
one hot vector or binary attributes

![](https://i.imgur.com/jXOaT8V.png)
![](https://i.imgur.com/QYxnA4x.png)
## Implementation
### Improved GAN Training
for stabilization, 
WassersteinGAN objective with gradient penalty
$$L_{adv}=E_x[D_{src}x]-E_{x,c}[D_{src}(G(x,c))] - \lambda_{gp}E_{\hat x}[(||\nabla \hat xD_{src}(\hat x)||_2-1)^2]$$
where \\(\hat x\\) is sampled uniformly along a straight line between a pair of a real and a generated images
\\(\lambda_{gp}=10\\)
### Network Architecture
CycleGAN for backbone
instance normalization for generator, discriminator no normalization
We leverage PatchGANs for the discriminator network, which classifies
whether local image patches are real or fake.


## Result
![](https://i.imgur.com/xbLPZZP.png)