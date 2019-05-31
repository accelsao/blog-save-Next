---
title: Machine Learning Paper Reading List 1905
tags:
	- Machine Learning
	- Gaussian Processes
	- Neural Processes
	- Kernel Methods
	- Variational Inference
	- Optimal Transport
	- Bayesian Neural Networks
	- Self-Supervised Learning
	- Disentangled Representations
	- VAE
	- Bayesian Optimization
	- Gaussian Processes
categories:
	- Machine Learning
mathjax: false
date: 2019-05-01 15:35:04
---

Subjects | `cs.LG` `stat.ML` `cs.CV`

Conference | [ICML 2019 Oral](https://icml.cc/Conferences/2019/Schedule?type=Oral), [CVPR 2019](http://cvpr2019.thecvf.com/program/main_conference#program_schedule)
WorkShop | [Self-Supervised Learning ICML 2019](https://sites.google.com/view/self-supervised-icml2019)
Meta-Learning 綜述 | [Learning to Learn with Gradients](http://people.eecs.berkeley.edu/~cbfinn/_files/dissertation.pdf)

<!--more-->

# TO READ

## Topics
Bayesian DL
Meta-learning
Disentangled Representations
Variational Inference
[Convergence of random variables](https://en.wikipedia.org/wiki/Convergence_of_random_variables)

## Papers
[Gauge Equivariant Convolutional Networks and the Icosahedral CNN](https://arxiv.org/abs/1902.04615)
[Stein Points](https://arxiv.org/abs/1803.10161)
[Stein Point Markov Chain Monte Carlo](https://arxiv.org/abs/1905.03673)
[A Contrastive Divergence for Combining Variational Inference and MCMC]https://arxiv.org/abs/1905.04062()
[Unsupervised Learning by Competing Hidden Units](https://arxiv.org/abs/1806.10181)
[Correlated Variational Auto-Encoders](https://arxiv.org/abs/1905.05335)
[Data-Efficient Image Recognition with Contrastive Predictive Coding](https://arxiv.org/abs/1905.09272)
[Learning to Convolve: A Generalized Weight-Tying Approach](https://arxiv.org/abs/1905.04663)
[Wasserstein variational gradient descent: From semi-discrete optimal transport to ensemble variational inference](https://arxiv.org/abs/1811.02827)
[Wasserstein Dependency Measure for Representation Learning](https://arxiv.org/abs/1903.11780)
[Disentangling Factors of Variation Using Few Labels](https://arxiv.org/abs/1905.01258)
[Shake-Shake regularization](https://arxiv.org/abs/1705.07485)

Probabilistic Model-Agnostic Meta-Learning
Meta-Learning Probabilistic Inference for Prediction
Bayesian Model-Agnostic Meta-Learning
Efficient Off-Policy Meta-Reinforcement Learning via Probabilistic Context Variables
Meta-Learning with Differentiable Convex Optimization
On Variational Bounds of Mutual Information
[Meta-Learning Update Rules for Unsupervised Representation Learning](https://arxiv.org/abs/1804.00222)
[A Tutorial on Bayesian Optimization](https://arxiv.org/abs/1807.02811)
[Unsupervised Data Augmentation](https://arxiv.org/abs/1904.12848)
[Meta-learning of Sequential Strategies](https://arxiv.org/abs/1905.03030)
[Disentangling by Factorising](https://arxiv.org/abs/1802.05983)
[Bayesian Optimization using Deep Gaussian Processes](https://arxiv.org/abs/1905.03350)
[Advances in Variational Inference](https://arxiv.org/abs/1711.05597)
[The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks](https://arxiv.org/abs/1803.03635)
[Kernel Implicit Variational Inference](https://arxiv.org/abs/1705.10119)
[On Variational Bounds of Mutual Information](https://arxiv.org/abs/1905.06922v1)
[MINE: Mutual Information Neural Estimation](https://arxiv.org/abs/1801.04062)
[Learning deep representations by mutual information estimation and maximization](https://arxiv.org/abs/1808.06670)

## Project
[google-research/mixmatch](https://github.com/google-research/mixmatch/blob/master/mixup.py)
[zju3dv/pvnet](https://github.com/zju3dv/pvnet)

# [Computational Optimal Transport](https://arxiv.org/abs/1803.00567)
`Optimal Transport`
## Notes
None

# [MixMatch: A Holistic Approach to Semi-Supervised Learning](https://arxiv.org/abs/1905.02249)
`Semi-supervised learning`
## Notes
A **holistic** approach to incorporates Consistency Regularization, Entropy Minimization and Traditional Regularization
mixing labels and unlabels examples by MIXUP


# Isolating Sources of Disentanglement in VAEs
Arxiv | Tags
--- |
[https://arxiv.org/abs/1802.04942](https://arxiv.org/abs/1802.04942) | VAE,  Disentangled Representations

# Gaussian Differential Privacy
Arxiv | Tags
--- |
[https://arxiv.org/abs/1905.02383](https://arxiv.org/abs/1905.02383) | ?

# Does Data Augmentation Lead to Positive Margin?
Arxiv | Tags
--- |
https://arxiv.org/abs/1905.03177 | ?

# Meta-learning of Sequential Strategies
Arxiv | Tags
--- |
[https://arxiv.org/abs/1905.03030](https://arxiv.org/abs/1905.03030) | ?

# Few-Shot Unsupervised Image-to-Image Translation
Arxiv | Blog
--- |
[https://arxiv.org/abs/1905.01723](https://arxiv.org/abs/1905.01723) | [nvlabs-FUNIT](https://nvlabs.github.io/FUNIT/)


# Adversarial Examples Are Not Bugs, They Are Features
Arxiv | Blog
--- |
[https://arxiv.org/abs/1905.02175](https://arxiv.org/abs/1905.02175) | [blog](http://gradientscience.org/adv/)

# Scaling and Benchmarking Self-Supervised Visual Representation Learning
Arxiv | Tags
--- |
[https://arxiv.org/abs/1905.01235](https://arxiv.org/abs/1905.01235) | Self-Supervised Learning

# Functional Variational Bayesian Neural Networks
Arxiv | OpenReview | Tag
--- |
[https://arxiv.org/abs/1903.05779](https://arxiv.org/abs/1903.05779) | [ICLR 2019](https://openreview.net/forum?id=rkxacs0qY7) | Bayesian Neural Networks

# Challenging Common Assumptions in the Unsupervised Learning of Disentangled Representations
authors | arxiv | blog
--- |
Francesco Locatello, Stefan Bauer, Mario Lucic, Gunnar Ratsch, Sylvain Gelly, Bernhard Scholkopf, Olivier Bachem | [https://arxiv.org/abs/1811.12359](https://arxiv.org/abs/1811.12359) | [google ai](https://ai.googleblog.com/2019/04/evaluating-unsupervised-learning-of.html)


# Representation Learning with Contrastive Predictive Coding
authors | arxiv
--- |
Aaron van den Oord, Yazhe Li, Oriol Vinyals | [https://arxiv.org/abs/1807.03748](https://arxiv.org/abs/1807.03748)

# Variational Inference: A Review for Statisticians
authors | arxiv | tag
--- |
David M. Blei, Alp Kucukelbir, Jon D. McAuliffe | [https://arxiv.org/abs/1601.00670](https://arxiv.org/abs/1601.00670) | Variational Inference

# Neural Processes
authors | arxiv | tag
--- |
Marta Garnelo, Jonathan Schwarz, Dan Rosenbaum, Fabio Viola, Danilo J. Rezende, S.M. Ali Eslami, Yee Whye Teh | [https://arxiv.org/abs/1807.01622](https://arxiv.org/abs/1807.01622) | Gaussian process, Neural Processes

# Gaussian Processes and Kernel Methods: A Review on Connections and Equivalences
authors | arxiv | tag
--- |
Motonobu Kanagawa, Philipp Hennig, Dino Sejdinovic, Bharath K Sriperumbudur | [https://arxiv.org/abs/1807.02582](https://arxiv.org/abs/1807.02582) | Gaussian Processes, Kernel Methods
