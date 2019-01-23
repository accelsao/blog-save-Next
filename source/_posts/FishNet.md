---
title: FishNet
tags:
  - null
categories:
  - null
mathjax: true
date: 2019-01-16 13:41:09
---


<!--more-->
Driven by the success of using high resolution features for region-level and pixel-level,
which enables the feature of high resolution to contain high-level semantic information
There are 3 advantages for FishNet
1. First backbone unifies the advantages of networks designed for pixel,region,image level tasks
2. It enables the gradient directly propogated from deep to shallow layers
Resnet utilize a conv layer with stride to deal with the inconsistency with input, output channels, which make the identity inapllicable and degrades the gradient from the
output to shallow layers
3. Features from different depth are preserved and used for refining each other

