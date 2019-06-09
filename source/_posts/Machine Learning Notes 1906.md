---
title: Machine Learning Notes 1906
tags:
  - Meta-Learning
  - Representation Learning
categories:
  - Machine Learning
mathjax: true
date: 2019-06-02 07:03:21
---

Subjects | `cs.LG` `stat.ML` `cs.CV`
Conference | [ICML 2019 Oral](https://icml.cc/Conferences/2019/Schedule?type=Oral), [CVPR 2019](http://cvpr2019.thecvf.com/program/main_conference#program_schedule)
Meta-Learning 綜述 | [Learning to Learn with Gradients](http://people.eecs.berkeley.edu/~cbfinn/_files/dissertation.pdf)

<!--more-->

# TO READ

## Topics
[`Jacobi's formula`](https://en.wikipedia.org/wiki/Jacobi's_formula)
[`Taylor series`](https://en.wikipedia.org/wiki/Taylor_series)
`Geometric deep learning on graphs and manifolds`
`Meta-Learning`

## Papers
[Residual Flows for Invertible Generative Modeling](https://arxiv.org/abs/1906.02735)
[An Introduction to Variational Autoencoders](https://arxiv.org/abs/1906.02691)
[Hamiltonian descent for composite objectives](https://arxiv.org/abs/1906.02608)
[Bayesian Deep Convolutional Networks with Many Channels are Gaussian Processes](https://arxiv.org/abs/1810.05148)
[Computational Optimal Transport](https://arxiv.org/abs/1803.00567)
[On Training Recurrent Neural Networks for Lifelong Learning](https://arxiv.org/abs/1811.07017)
[Modeling Uncertainty by Learning a Hierarchy of Deep Neural Connections](https://arxiv.org/abs/1905.13195)
[Online Meta-Learning](https://arxiv.org/abs/1902.08438)
[Advances in Variational Inference](https://arxiv.org/abs/1711.05597)
[On Variational Bounds of Mutual Information](https://arxiv.org/abs/1905.06922)

## Projects
[dragen1860/MAML-Pytorch](https://github.com/dragen1860/MAML-Pytorch)
[y2l/mini-imagenet-tools](https://github.com/y2l/mini-imagenet-tools)

## WorkShop
[The 2nd Learning from Limited Labeled Data (LLD) Workshop](https://lld-workshop.github.io/)
[Uncertainty & Robustness in Deep Learning](https://sites.google.com/view/udlworkshop2019/accepted-papers?authuser=0)
[Theoretical Physics for Deep Learning](https://sites.google.com/view/icml2019phys4dl/accepted-papers?authuser=0)

# Notes

## [Invertible Residual Networks](https://arxiv.org/abs/1811.00995)
Proof. \\(ln(1-x) = -\sum^{\infty}_{n=1}\frac{x^n}{n}\\)
$$
\frac{d}{dx}ln(1-x) = \frac{-1}{ln(1-x)} \\
-(1-x)^{-1} = -(1 + x + x^2 ...) \\
$$
$$
\begin{align}
ln(1-x) &= \int -(1 + x + x^2 ...) \\
&= -(x + \frac{x^2}{2} + \frac{x^3}{3} ...) \\
&= -\sum^{\infty}_{n=1}\frac{x^n}{n}
\end{align}
$$

## [Generating Diverse High-Fidelity Images with VQ-VAE-2](https://arxiv.org/abs/1906.00446)

**Paper似乎沒出現Algoirthm3 但內文有 可能還需要等新版本**

It demonstrates a multiscale hierarchical organization of VQ-VAE with quality that rivals BigGAN-deep, while not suffering from mode collapse and lack of diversity.
It requires sampling an autoregression model in compressed latent space which is faster than sampling in pixel space
Stage 1. Hierarchical Latent Codes Learning
Stage 2: Learning Priors over Latent Codes
using large conditioning stacks in top-level prior instead of self-attention due to high memory constraints in practical
 
![](https://i.imgur.com/KsT8jxq.png)
![](https://i.imgur.com/geOYOHJ.png)

## [Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)
一個簡單暴力的 meta-learning 方法

## [Geometric Losses for Distributional Learning](https://arxiv.org/abs/1905.06005)
公式很吃力 需補數學

## [Meta-Learning Representations for Continual Learning](https://arxiv.org/abs/1905.12588)
Q1. TLN 具體是什麼
[khurramjaved96/mrcl](https://github.com/Khurramjaved96/mrcl)

## [Challenging Common Assumptions in the Unsupervised Learning of Disentangled Representations](https://arxiv.org/abs/1811.12359)
The disantanglement scores of unsupervised models are heavily influenced by randomness(random seed) and the choice of the hyperparameter(regularization strength).
The objective function appears to have less impact.



