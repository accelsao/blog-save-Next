---
title: Generative Adversarial Nets
tags:
  - GAN
categories:
  - Machine Learning
mathjax: true
date: 2018-11-04 10:47:15
---


論文筆記
<!--more-->

# Abstract
A framework for estimating generative models via adversarial process
Train 2 models: Generative G that capture the data distribution, and a discriminative model D that estimates the probability that a sample came from the training data rather than G
The procedure for G is to maximize the probability of D making a mistake

# Adversarial nets