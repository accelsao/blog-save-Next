---
title: Machine Learning Notes 1906
tags:
  - Meta Learning
  - Representation Learning
  - Variational Autoencoders
  - Geometric Deep Learning
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
[`2-satisfiability`](https://en.wikipedia.org/wiki/2-satisfiability)
`Disentangled Representation Learning`
`Gaussian Process`
`Variational Autoencoders`
[`Jacobi's formula`](https://en.wikipedia.org/wiki/Jacobi's_formula)
[`Taylor series`](https://en.wikipedia.org/wiki/Taylor_series)
`Geometric deep learning on graphs and manifolds`
`Meta-Learning`


## Papers
[Rates of Convergence for Sparse Variational Gaussian Process Regression](https://arxiv.org/abs/1903.03571)
[Disentangling Factors of Variation Using Few Labels](https://arxiv.org/abs/1905.01258)
[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)
[Geometric Losses for Distributional Learning](https://arxiv.org/abs/1905.06005)
[An Introduction to Variational Autoencoders](https://arxiv.org/abs/1906.02691)
[Disentangled State Space Representations](https://arxiv.org/abs/1906.03255)
[DeepMDP: Learning Continuous Latent Space Models for Representation Learning](https://arxiv.org/abs/1906.02736)
[Residual Flows for Invertible Generative Modeling](https://arxiv.org/abs/1906.02735)
[Bayesian Deep Convolutional Networks with Many Channels are Gaussian Processes](https://arxiv.org/abs/1810.05148)
[Computational Optimal Transport](https://arxiv.org/abs/1803.00567)
[Modeling Uncertainty by Learning a Hierarchy of Deep Neural Connections](https://arxiv.org/abs/1905.13195)
[Online Meta-Learning](https://arxiv.org/abs/1902.08438)
[Advances in Variational Inference](https://arxiv.org/abs/1711.05597)
[On Variational Bounds of Mutual Information](https://arxiv.org/abs/1905.06922)

## Projects
[khurramjaved96/mrcl](https://github.com/Khurramjaved96/mrcl)
[dragen1860/MAML-Pytorch](https://github.com/dragen1860/MAML-Pytorch)
[y2l/mini-imagenet-tools](https://github.com/y2l/mini-imagenet-tools)

## WorkShop
[The 2nd Learning from Limited Labeled Data (LLD) Workshop](https://lld-workshop.github.io/)
[Uncertainty & Robustness in Deep Learning](https://sites.google.com/view/udlworkshop2019/accepted-papers?authuser=0)
[Theoretical Physics for Deep Learning](https://sites.google.com/view/icml2019phys4dl/accepted-papers?authuser=0)

# Notes

## [Functional Regularisation for Continual Learning](https://arxiv.org/abs/1901.11356)

(i) Apply data distillation and variational sparse GP inference in sequential fashion for function space regularisation in CL

Q1. why sample to weight space

## [Invertible Residual Networks](https://arxiv.org/abs/1811.00995)

### Formula Derivatives
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

## [Meta-Learning Representations for Continual Learning](https://arxiv.org/abs/1905.12588)
Q1. TLN 具體是什麼

## [Challenging Common Assumptions in the Unsupervised Learning of Disentangled Representations](https://arxiv.org/abs/1811.12359)
`ICML 2019 Best Paper`

(i) A factorising aggregated posterior does not seem to necessarily imply that the dimensions in the representation are uncorrelated.
(ii) Random seeds and hyperparameters seem to matter more than model but tunning seem to require supervision.
(iii) We didnt observe that increased disentanglement implies a decreased sample complexity of learning downstream tasks.

Theorem 1 show that unsupervised distentanglement learning is fundamentally impossible for arbitrary generative models.

Q1. factorized prior? non-factorized prior?
Q2. why Theorem 1 shows that we need inductive biases on data set? (詳見 B Proof of Theorem 1)





