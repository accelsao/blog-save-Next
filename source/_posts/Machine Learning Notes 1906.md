---
title: Machine Learning Notes 1906
tags:
  - Meta Learning
  - Representation Learning
  - Variational Autoencoders
  - Geometric Deep Learning
  - stochastic processes
categories:
  - Machine Learning
mathjax: true
date: 2019-06-02 07:03:21
---

Subjects | `cs.LG` `stat.ML` `cs.CV`
Conference | [ICML 2019 Oral](https://icml.cc/Conferences/2019/Schedule?type=Oral), [CVPR 2019](http://cvpr2019.thecvf.com/program/main_conference)
Meta-Learning 綜述 | [Learning to Learn with Gradients](http://people.eecs.berkeley.edu/~cbfinn/_files/dissertation.pdf)

<!--more-->

# TO READ

## Topics
`Uncertainty in Deep Learning`
`stochastic processes`
`Hermitian adjoint`
`Legendre transformation`
`Lebesgue measures`
[`Importance sampling`](https://statweb.stanford.edu/~owen/mc/Ch-var-is.pdf)
[`Hölder's inequality`](https://en.wikipedia.org/wiki/H%C3%B6lder%27s_inequality)
[`Markov's inequality`](https://en.wikipedia.org/wiki/Markov%27s_inequality)
`Gaussian Process Regression`
[`2-satisfiability`](https://en.wikipedia.org/wiki/2-satisfiability)
`Disentangled Representation Learning`
`Gaussian Process`
`Variational Autoencoders`
[`Jacobi's formula`](https://en.wikipedia.org/wiki/Jacobi's_formula)
[`Taylor series`](https://en.wikipedia.org/wiki/Taylor_series)
`Geometric deep learning on graphs and manifolds`
`Meta-Learning`

## Papers
`ANODE: Unconditionally Accurate Memory-Efficient Gradients for Neural ODEs`
`Neural ODEs`
`On Statistical Optimality of Variational Bayes`
[Gauge Equivariant Convolutional Networks and the Icosahedral CNN](https://arxiv.org/abs/1902.04615)
[Reparameterizing Distributions on Lie Groups](https://arxiv.org/abs/1903.02958)
[A Theory of Fermat Paths for Non-Line-of-Sight Shape Reconstruction](http://imaging.cs.cmu.edu/fermat_paths/assets/cvpr2019.pdf) `CVPR 2019 Best Paper`
[Towards a Definition of Disentangled Representations](https://arxiv.org/abs/1812.02230)
[Wasserstein Reinforcement Learning](https://arxiv.org/abs/1906.04349)
[Few-Shot Unsupervised Image-to-Image Translation](https://arxiv.org/abs/1905.01723)
[Reweighted Expectation Maximization](https://arxiv.org/abs/1906.05850)
[Functional Regularisation for Continual Learning](https://arxiv.org/abs/1901.11356)
[Disentangling Factors of Variation Using Few Labels](https://arxiv.org/abs/1905.01258)
[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)
[Geometric Losses for Distributional Learning](https://arxiv.org/abs/1905.06005)
[An Introduction to Variational Autoencoders](https://arxiv.org/abs/1906.02691)
[DeepMDP: Learning Continuous Latent Space Models for Representation Learning](https://arxiv.org/abs/1906.02736)
[Residual Flows for Invertible Generative Modeling](https://arxiv.org/abs/1906.02735)
[Bayesian Deep Convolutional Networks with Many Channels are Gaussian Processes](https://arxiv.org/abs/1810.05148)
[Computational Optimal Transport](https://arxiv.org/abs/1803.00567)
[Modeling Uncertainty by Learning a Hierarchy of Deep Neural Connections](https://arxiv.org/abs/1905.13195)
[Online Meta-Learning](https://arxiv.org/abs/1902.08438)
[Advances in Variational Inference](https://arxiv.org/abs/1711.05597)
[On Variational Bounds of Mutual Information](https://arxiv.org/abs/1905.06922)

## Projects
[autonomousvision/occupancy_networks](https://github.com/autonomousvision/occupancy_networks)
[hjwdzh/TextureNet](https://github.com/hjwdzh/TextureNet)
[google/TensorNetwork](https://github.com/google/TensorNetwork)
[google/mannequinchallenge](https://github.com/google/mannequinchallenge)
[dragen1860/MAML-Pytorch](https://github.com/dragen1860/MAML-Pytorch)
[NVlabs/FUNIT](https://github.com/nvlabs/FUNIT/)
[khurramjaved96/mrcl](https://github.com/Khurramjaved96/mrcl)
[y2l/mini-imagenet-tools](https://github.com/y2l/mini-imagenet-tools)

## WorkShop & Tutorial
[Hand-waving and interpretive dance: an introductory course on tensor networks](https://iopscience.iop.org/article/10.1088/1751-8121/aa6dc3)
[PDE-constrained optimization and the adjoint method](https://cs.stanford.edu/~ambrad/adjoint_tutorial.pdf)
[A Primer on PAC-Bayesian Learning](https://bguedj.github.io/icml2019/index.html)
[Variational Bayes and beyond: Bayesian inference for big data](http://www.tamarabroderick.com/tutorial_2018_icml.html)
[GrandBallroom_Dec4_1_A Primer on Optimal Transport](https://vimeo.com/248504509)
[Gaussian Processes - Neil Lawrence's Talks](http://inverseprobability.com/talks/notes/gaussian-processes.html)
[Meta-Learning: from Few-Shot Learning to Rapid  Reinforcement Learning - ICML 2019 Tutorial](https://sites.google.com/view/icml19metalearning)
[The 2nd Learning from Limited Labeled Data (LLD) Workshop](https://lld-workshop.github.io/)
[Uncertainty & Robustness in Deep Learning](https://sites.google.com/view/udlworkshop2019/accepted-papers?authuser=0)
[Theoretical Physics for Deep Learning](https://sites.google.com/view/icml2019phys4dl/accepted-papers?authuser=0)

# Notes

## [Learning the Depths of Moving People by Watching Frozen People](https://arxiv.org/abs/1904.11111)

network is based on [`Single-Image Depth Perception in the Wild`](https://arxiv.org/abs/1604.03901)
![](https://i.imgur.com/3sc2KtZ.png)
如下圖 hourglass結構 回傳predict depth map and confidence map

### 參考
[MegaDepth: Learning Single-View Depth Prediction from Internet Photos](https://arxiv.org/abs/1804.00607)


## [Automated Scalable Bayesian Inference via Hilbert Coresets](https://arxiv.org/abs/1710.05053)
![](https://i.imgur.com/fdoLeep.png)

## A Conceptual Introduction to Hamiltonian Monte Carlo
為了保持在typical set, 加入momentum的概念是HMC的主要概念 kinetic energy+ potential energy

## [The Functional Neural Process](https://arxiv.org/abs/1906.08324)
exchangable stochastic processes
![](https://i.imgur.com/oBVRZQL.png)

## [Fundamentals of Recurrent Neural Network (RNN) and Long Short-Term Memory (LSTM) Network](https://arxiv.org/abs/1808.03314)
The detail math in RNN and LSTM.

## [Rates of Convergence for Sparse Variational Gaussian Process Regression](https://arxiv.org/abs/1903.03571)
Use upper bounds of KL divergence that depend on the quality of Nysrom approximation to the data covarience matrix.
It proves that the greater the smoothness of functions in the prior and the greater the concentration of observations in input space, the sparser an approximation can be made.

## [Disentangled State Space Representations](https://arxiv.org/abs/1906.03255)

![](https://i.imgur.com/SLdUOsz.png)

A Lower bound derivation
KL term 是否有誤? 應該為negative term吧
$$
KL(q(S|X)||p_{0}(S)) = \int_{S}q(S|X)log\frac{p_{0}(S)}{q(S|X)}
$$

## [Self-Supervised Exploration via Disagreement](https://arxiv.org/abs/1906.04161)

![](https://i.imgur.com/rijTjKh.png)

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





