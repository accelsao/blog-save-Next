---
title: Deep Learning (IYA) Chapter 9 - Convolutional Networks
date: 2018-09-17 09:54:04
tags:
- Convolutional Neural Network
categories:
- Artificial Intelligence
- Machine Learning
- Deep Learning
mathjax: true
---



Convolution is a specialized kind of linear operation.
Convolutional networks are simply neural networks that use convolution in place of general matrix
multiplication in at least one of their layers


<!--more-->

# The Convolution Operation

Suppose we are tracking the location of a spaceship with a laser sensor. Our
laser sensor provides a single output x(t), the position of the spaceship at time
t. Both x and t are real-valued, we can get a different reading from the laser
sensor at any instant in time.

Now suppose that our laser sensor is somewhat noisy. To obtain a less noisy estimate of the spaceship’s position, we would like to average together several measurements

If we apply such a weighted average operation at every moment, we obtain a new
function providing **s** a smoothed estimate of the position of the spaceship:
$$s(t)=\int x(a)w(t-a)da$$
This operation is called convolution.
The convolution operation is typically denoted with an asterisk:
$$s(t)=(x*w)(t)$$


In convolutional network terminology, the first argument (in this example, the function x) to the convolution is often referred to as the input and the second argument (in this example, the function w) as the kernel. The output is sometimes referred to as the feature map.


If we use a two-dimensional image I as our input, we probably also want
to use a two-dimensional kernel K
$$S(i,j)=(I*K)(i,j)=\sum_m\sum_nI(m,n)K(i-m,j-n)$$
or
$$ S(i,j)=(K*I)(i,j)=\sum_m\sum_nI(i-m,j-n)K(m,n) $$

Usually the latter formula is more straightforward to implement in a machine
learning library, because there is less variation in the range of valid values of m
and n.


Instead, many neural network libraries implement a
related function called the cross-correlation, which is the same as convolution
but without flipping the kernel:
$$ S(i,j)=(I*K)(i,j)=\sum_m\sum_nI(i+m,j+n)K(m,n) $$

# Motivation
# Pooling

A typical layer of a convolutional network consists of three stages.
In the first stage, the layer performs several convolutions in parallel to produce a
set of linear activations. In the second stage, each linear activation is run through
a nonlinear activation function, such as the rectified linear activation function.
This stage is sometimes called the detector stage. In the third stage, we use a
pooling function to modify the output of the layer further.