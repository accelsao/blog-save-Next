---
title: Deep Learning Paper Reading List 1904
tags:
	- Autoregressive Model
	- Deep Learning
	- Disentangled Representations
	- Few Shot Learning
	- Generative Adversarial Network
	- Lipschitz Continuity
	- Meta-Learning
	- Metric Learning
	- Mutual Information
	- Object Detection
	- Representation Learning
	- Unsupervised Learning
	- Unsupervised Learning
	- Variational Autoencoders
	- Variational Inference
	- Visual Concepts
	- Wasserstein Metric
categories:
	- Reading Notes
mathjax: false
date: 2019-04-01 11:50:11
---

SOTA, 思路, 實驗, Open Problems
Subjects | Machine Learning, Computer Vision and Pattern Recognition

[ICLR 2019 Schedule](https://iclr.cc/Conferences/2019/Schedule)
[Pytorch Doc](https://pytorch.org/docs/stable/index.html)

<!--more-->

# SCAN: Learning Hierarchical Compositional Visual Concepts
authors | arxiv | tag
--- |
Irina Higgins, Nicolas Sonnerat, Loic Matthey, Arka Pal, Christopher P Burgess, Matko Bosnjak, Murray Shanahan, Matthew Botvinick, Demis Hassabis, Alexander Lerchner | [https://arxiv.org/abs/1707.03389](https://arxiv.org/abs/1707.03389) | Visual Concepts

# Isolating Sources of Disentanglement in VAEs
authors | arxiv | tag
--- |
Ricky T. Q. Chen, Xuechen Li, Roger Grosse, David Duvenaud | [https://arxiv.org/abs/1802.04942](https://arxiv.org/abs/1802.04942) | Variational Autoencoder, Disentangled Representations

# Tutorial on Variational Autoencoders
authors | arxiv | tag
--- |
Carl Doersch | [https://arxiv.org/abs/1606.05908](https://arxiv.org/abs/1606.05908) | Variational Autoencoders

# Challenging Common Assumptions in the Unsupervised Learning of Disentangled Representations
authors | arxiv | blog
--- |
Francesco Locatello, Stefan Bauer, Mario Lucic, Gunnar Rätsch, Sylvain Gelly, Bernhard Schölkopf, Olivier Bachem | [https://arxiv.org/abs/1811.12359](https://arxiv.org/abs/1811.12359) | [google ai](https://ai.googleblog.com/2019/04/evaluating-unsupervised-learning-of.html)

# Generating Long Sequences with Sparse Transformers
authors | blog
--- |
Rewon Child, Scott Gray, Alec Radford, Ilya Sutskever | [Generative Modeling with Sparse Transformers](https://openai.com/blog/sparse-transformer/)


# FineGAN: Unsupervised Hierarchical Disentanglement for Fine-Grained Object Generation and Discovery
authors | arxiv | tag | comment | code
--- |
Krishna Kumar Singh, Utkarsh Ojha, Yong Jae Lee | [https://arxiv.org/abs/1811.11155](https://arxiv.org/abs/1811.11155) | Generative Neural Network | CVPR 2019 Oral | [kkanshul/finegan](https://github.com/kkanshul/finegan)
## Notes
![](https://i.imgur.com/13iWyQn.png)


# CornerNet-Lite: Efficient Keypoint Based Object Detection
authors | arxiv | tag | code
--- |
Hei Law, Yun Teng, Olga Russakovsky, Jia Deng | [https://arxiv.org/abs/1904.08900](https://arxiv.org/abs/1904.08900) | Object Detection | [princeton-vl/CornerNet-Lite](https://github.com/princeton-vl/CornerNet-Lite)

# CornerNet: Detecting Objects as Paired Keypoints
authors | arxiv | tag | code | comment
--- |
Hei Law, Jia Deng | [https://arxiv.org/abs/1808.01244](https://arxiv.org/abs/1808.01244) | Object Detection | [princeton-vl/CornerNet](https://github.com/princeton-vl/CornerNet) | ECCV 2018

# CenterNet: Keypoint Triplets for Object Detection
authors | arxiv | tag | code
--- |
Kaiwen Duan, Song Bai, Lingxi Xie, Honggang Qi, Qingming Huang, Qi Tian | [https://arxiv.org/abs/1904.08189](https://arxiv.org/abs/1904.08189) | Object Detection | [Duankaiwen/CenterNet](https://github.com/Duankaiwen/CenterNet)

# Which Training Methods for GANs do actually Converge?
authors | arxiv | comment | tag | code
--- |
Lars Mescheder, Andreas Geiger, Sebastian Nowozin | [https://arxiv.org/abs/1801.04406](https://arxiv.org/abs/1801.04406) | ICML 2018 | Generative Adversarial Network | [LMescheder/GAN_stability](https://github.com/LMescheder/GAN_stability)

# Semantic Image Synthesis with Spatially-Adaptive Normalization
authors | arxiv | comment
--- |
Taesung Park, Ming-Yu Liu, Ting-Chun Wang, Jun-Yan Zhu | [https://arxiv.org/abs/1903.07291](https://arxiv.org/abs/1903.07291) | CVPR 2019 oral


# A Tutorial on Distance Metric Learning: Mathematical Foundations, Algorithms and Software
authors | arxiv | tag
--- |
Juan Luis Suárez, Salvador García, Francisco Herrera | [https://arxiv.org/abs/1812.05944](https://arxiv.org/abs/1812.05944) | Metric Learning

# Dynamic Few-Shot Visual Learning without Forgetting
authors | arxiv | tag | comment | code
--- |
Spyros Gidaris, Nikos Komodakis | [https://arxiv.org/abs/1804.09458](https://arxiv.org/abs/1804.09458) | Few Shot Learning | CVPR 2018 | [gidariss/FewShotWithoutForgetting](https://github.com/gidariss/FewShotWithoutForgetting)

# Low-Shot Learning with Imprinted Weights
authors | arxiv | comment
--- |
Hang Qi, Matthew Brown, David G. Lowe | [https://arxiv.org/abs/1712.07136](https://arxiv.org/abs/1712.07136) | CVPR 2018

# Non-local Neural Networks
authors | arxiv | comment | code
--- |
Xiaolong Wang, Ross Girshick, Abhinav Gupta, Kaiming He | [https://arxiv.org/abs/1711.07971](https://arxiv.org/abs/1711.07971) | CVPR 2018 | [video-nonlocal-net](https://github.com/facebookresearch/video-nonlocal-net)

# Large-Scale Long-Tailed Recognition in an Open World
authors | arxiv | tag | comment | project
--- |
Large-Scale Long-Tailed Recognition in an Open World | [https://arxiv.org/abs/1904.05160](https://arxiv.org/abs/1904.05160) | Generative Neural Network | CVPR 2019 oral | [project](https://liuziwei7.github.io/projects/LongTail.html)

# Spectral Normalization for Generative Adversarial Networks
authors | arxiv | tag | comment
--- |
Takeru Miyato, Toshiki Kataoka, Masanori Koyama, Yuichi Yoshida | [https://arxiv.org/abs/1802.05957](https://arxiv.org/abs/1802.05957) | Generative Neural Network | ICLR 2018 oral

# Self-Attention Generative Adversarial Networks
authors | arxiv | tag | code
--- |
Han Zhang, Ian Goodfellow, Dimitris Metaxas, Augustus Odena | [https://arxiv.org/abs/1805.08318](https://arxiv.org/abs/1805.08318) | Generative Neural Network | [heykeetae/Self-Attention-GAN](https://github.com/heykeetae/Self-Attention-GAN)

# Large Scale GAN Training for High Fidelity Natural Image Synthesis
authors | arxiv | tag | comment | code
--- |
Andrew Brock, Jeff Donahue, Karen Simonyan | [https://arxiv.org/abs/1809.11096](https://arxiv.org/abs/1809.11096) | Generative Neural Network | ICLR 2019 oral | [ajbrock/BigGAN-PyTorch](https://github.com/ajbrock/BigGAN-PyTorch)

# Meta-Learning Update Rules for Unsupervised Representation Learning
authors | arxiv | tag
--- |
Luke Metz, Niru Maheswaranathan, Brian Cheung, Jascha Sohl-Dickstein | [https://arxiv.org/abs/1804.00222](https://arxiv.org/abs/1804.00222) | Meta-Learning, Representation Learning, Unsupervised Learning

# Variational Inference: A Review for Statisticians
authors | arxiv | tag
--- |
David M. Blei, Alp Kucukelbir, Jon D. McAuliffe | [https://arxiv.org/abs/1601.00670](https://arxiv.org/abs/1601.00670) | Variational Inference

# Continual Lifelong Learning with Neural Networks: A Review
authors | arxiv | tag
--- |
German I. Parisi, Ronald Kemker, Jose L. Part, Christopher Kanan, Stefan Wermter | [https://arxiv.org/abs/1802.07569](https://arxiv.org/abs/1802.07569) | Continual Lifelong Learning

# Learning deep representations by mutual information estimation and maximization
authors | arxiv | tag | comment | openreview
--- |
R Devon Hjelm, Alex Fedorov, Samuel Lavoie-Marchildon, Karan Grewal, Phil Bachman, Adam Trischler, Yoshua Bengio | [https://arxiv.org/abs/1808.06670](https://arxiv.org/abs/1808.06670) | Representation Learning, Unsupervised Learning, Deep Learning | ICLR 2019 oral | [https://openreview.net/forum?id=Bklr3j0cKX](https://openreview.net/forum?id=Bklr3j0cKX)


# Generating High Fidelity Images with Subscale Pixel Networks and Multidimensional Upscaling
authors | arxiv | tag | comment | openreview
--- |
Jacob Menick, Nal Kalchbrenner | [https://arxiv.org/abs/1812.01608](https://arxiv.org/abs/1812.01608) | Autoregressive Model | ICLR 2019 oral | [https://openreview.net/forum?id=HylzTiC5Km](https://openreview.net/forum?id=HylzTiC5Km)
## Notes
This paper shows autogression model can generate high fidelity images

# Wasserstein Dependency Measure for Representation Learning
authors | arxiv | tag
--- |
Sherjil Ozair, Corey Lynch, Yoshua Bengio, Aaron van den Oord, Sergey Levine, Pierre Sermanet | [https://arxiv.org/abs/1903.11780](https://arxiv.org/abs/1903.11780) | Representation Learning, Lipschitz Continuity, Wasserstein Metric, Mutual Information


# Superposition of many models into one
authors | Brian Cheung, Alex Terekhov, Yubei Chen, Pulkit Agrawal, Bruno Olshausen
arxiv | [https://arxiv.org/abs/1902.05522](https://arxiv.org/abs/1902.05522)

# How Does Batch Normalization Help Optimization?
Authors | Shibani Santurkar, Dimitris Tsipras, Andrew Ilyas, Aleksander Madry
arxiv | [https://arxiv.org/abs/1805.11604](https://arxiv.org/abs/1805.11604)
comment | NeurIPS 2018 oral
tag | Deep Learning

# Neural Ordinary Differential Equations
Authors | Ricky T. Q. Chen, Yulia Rubanova, Jesse Bettencourt, David Duvenaud
arxiv | [https://arxiv.org/abs/1806.07366](https://arxiv.org/abs/1806.07366)
comment | NeurIPS 2018 oral


# Lipschitz Generative Adversarial Nets
Authors | Zhiming Zhou, Jiadong Liang, Yuxuan Song, Lantao Yu, Hongwei Wang, Weinan Zhang, Yong Yu, Zhihua Zhang
arxiv | [https://arxiv.org/abs/1902.05687](https://arxiv.org/abs/1902.05687)
tag | Generative Adversarial Network, Lipschitz Continuity

## Review
提出`gradient uninformativeness`的概念


