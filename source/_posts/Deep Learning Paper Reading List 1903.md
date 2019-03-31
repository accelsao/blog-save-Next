---
title: Deep Learning Paper Reading List 1903
tags:
  - Computer Vision
  - Deep Reinforcement Learning
  - Unsupervised Learning
  - CVPR 2019
  - ICLR 2019
  - Weakly-Supervised Learning
  - 3D Human Pose Estimation
  - Self-supervised Learning
  - Generative Adversarial Network
  - Deep Learning
  - Representation Learning
categories:
  - Learning Notes
mathjax: false
date: 2019-03-25 11:53:13
---

紀錄papers
SOTA,思路,實驗,Open problem 

<!--more-->

# Unsupervised Part-Based Disentangling of Object Shape and Appearance
作者: Dominik Lorenz, Leonard Bereska, Timo Milbich, Björn Ommer
arxiv: [https://arxiv.org/abs/1903.06946](https://arxiv.org/abs/1903.06946)
comment: CVPR 2019 Oral
tag: Unsupervised Learning
## Review
把unlabel image用part-base shape和appearance處理
![](https://i.imgur.com/kaSCJT8.png)



# Weakly-Supervise Discovery of Geometry-Aware Representation for 3D Human Pose Estimation
作者: Xipeng Chen, Kwan-Yee Lin, Wentao Liu, Chen Qian, Liang Lin
arxiv: [https://arxiv.org/abs/1903.08839](https://arxiv.org/abs/1903.08839)
comment: CVPR 2019 Oral
tag: Weakly-Supervised Learning,3D Human Pose Estimation
## Review
利用2d skeleton map 來當 supervision
![](https://i.imgur.com/7tfNkRD.png)


# Self-supervised Visual Feature Learning with Deep Neural Networks: A Survey

# Unsupervised Person Re-identification by Soft Multilabel Learning
Authors: Hong-Xing Yu, Wei-Shi Zheng, Ancong Wu, Xiaowei Guo, Shaogang Gong, Jian-Huang Lai
arxiv: [https://arxiv.org/abs/1903.06325](https://arxiv.org/abs/1903.06325)
comment: CVPR 2019 oral
tag: Self-supervised Learning


# Self-Attention Generative Adversarial Networks
Authors: Han Zhang, Ian Goodfellow, Dimitris Metaxas, Augustus Odena
arxiv: [https://arxiv.org/abs/1805.08318](https://arxiv.org/abs/1805.08318)
tag: Generative Adversarial Network
code: [SAGAN](https://github.com/heykeetae/Self-Attention-GAN)

# Spectral Normalization for Generative Adversarial Networks
Authors: Takeru Miyato, Toshiki Kataoka, Masanori Koyama, Yuichi Yoshida
arxiv: [https://arxiv.org/abs/1802.05957](https://arxiv.org/abs/1802.05957)
comment: ICLR 2018 oral
tag: Generative Adversarial Network
code: [SNGAN](https://github.com/pfnet-research/sngan_projection)


# Large Scale GAN Training for High Fidelity Natural Image Synthesis
Authors: Andrew Brock, Jeff Donahue, Karen Simonyan
arxiv: [https://arxiv.org/abs/1809.11096](https://arxiv.org/abs/1809.11096)
comment: ICLR 2019 oral
tag: Generative Adversarial Network
code: [BigGAN-Pytorch](https://github.com/ajbrock/BigGAN-PyTorch)

# Deep Reinforcement Learning
Authors: Yuxi Li
arxiv: [https://arxiv.org/abs/1810.06339](https://arxiv.org/abs/1810.06339)
comment: DRL overview
tag: Deep Reinforcement Learning

# Invariant Information Clustering for Unsupervised Image Classification and Segmentation
Authors: Xu Ji, João F. Henriques, Andrea Vedaldi
arxiv: [https://arxiv.org/abs/1807.06653](https://arxiv.org/abs/1807.06653)
tag: Unsupervised Learning

# Learning deep representations by mutual information estimation and maximization
Authors: R Devon Hjelm, Alex Fedorov, Samuel Lavoie-Marchildon, Karan Grewal, Phil Bachman, Adam Trischler, Yoshua Bengio
arxiv: [https://arxiv.org/abs/1808.06670](https://arxiv.org/abs/1808.06670)
comment: ICLR 2019 oral
tag: Representation Learning, mutual information, Unsupervised Learning

# Model Based Reinforcement Learning for Atari
Authors: Lukasz Kaiser, Mohammad Babaeizadeh, Piotr Milos, Blazej Osinski, Roy H Campbell, Konrad Czechowski, Dumitru Erhan, Chelsea Finn, Piotr Kozakowski, Sergey Levine, Ryan Sepassi, George Tucker, Henryk Michalewski
arxiv: [https://arxiv.org/abs/1903.00374](https://arxiv.org/abs/1903.00374)
tag: Model-based deep RL

# Lipschitz Generative Adversarial Nets
Authors: Zhiming Zhou, Jiadong Liang, Yuxuan Song, Lantao Yu, Hongwei Wang, Weinan Zhang, Yong Yu, Zhihua Zhang
arxiv: [https://arxiv.org/abs/1902.05687](https://arxiv.org/abs/1902.05687)
tag: Generative Adversarial Network

