---
title: CycleGAN
tags:
  - null
categories:
  - null
mathjax: true
date: 2018-12-09 14:12:29
---

[Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks](https://arxiv.org/pdf/1703.10593.pdf)
--Learning Notes
<!--more-->

## Adversarial Loss
$$
\begin{equation} \label{eq1}
L_{GAN}(G,D_Y,X,Y)=E_{y\sim p_{data}\ (y)}[log\ D_Y(y)]+E_{x\sim p_{data}\ (x)}[1-log\ D_Y(G(x))]
\end{equation}
$$
so as \\(L_{GAN}(F,D_X,Y,X)\\)

## Cycle Consistency Loss
$$
\begin{equation} \label{eq2}
L_{cyc}(G,F)=E_{x\sim p_{data}\ (x)}[||F(G(x)-x||_1)]+E_{y\sim p_{data}\ (y)}[||G(F(y)-y||_1)]

\end{equation}
$$

## Objective
$$
\begin{equation} \label{eq3}
L(G,F,X,Y)=L_{GAN}(G,D_Y,X,Y)+L_{GAN}(F,D_X,Y,X)+\lambda L_{cyc}(G,F)

\end{equation}
$$
where \\(\lambda \\) control the relative between two objectives
we aim to solve
$$
\begin{equation} \label{eq4}
G^*,F^*=arg\min_{G,F}\max_{D_X,D_Y}L(G,F,D_X,D_Y)

\end{equation}
$$

# Implementation
## Network Architecture
### 70X70 PatchGan
[Question: PatchGAN Discriminator #39](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/39)
## Training details
We change log-likekihood objective to least square loss, it is more stable and generates higher result
Second, to reduce mode oscillation ,we update discriminators using the 50 previously created images
