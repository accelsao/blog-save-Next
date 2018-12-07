---
title: Deep Convolutional GAN
tags:
  - null
categories:
  - null
mathjax: true
date: 2018-12-04 12:32:03
---
[DC-GAN](https://arxiv.org/pdf/1511.06434.pdf)
[DCGAN-Tensorflow](https://github.com/carpedm20/DCGAN-tensorflow/blob/master/download.py)
<!--more-->


# Approach
1.
Replace pooling with strided convolutions
2.
![](https://i.imgur.com/MXlvzQ9.png)
3.
Batchnorm everywhere except input of D and output of G
4.
Relu in generator except tje output with tanh
LeakyRelu for discriminator
```
* Replace any pooling layers with strided convolutions (discriminator) and fractional-strided convolutions (generator)
* Use batchnorm in both the generator and the discriminator. Except the output of G and input of D
* Remove fully connected hidden layers for deeper architectures.
* Use ReLU activation in generator for all layers except for the output, which uses Tanh.
* Use LeakyReLU activation in the discriminator for all layers.
```


# [Pytorch](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html)
