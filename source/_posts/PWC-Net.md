---
title: PWC-Net
tags:
  - Flow
categories:
  - Computer Vision
mathjax: false
date: 2019-01-12 12:52:04
---

Notes on PWC-Net paper

<!--more-->


# Approach

## Feature Pyramid Extractor
just extracte image feature by conv2d
as raw images are variant to shadows and lighting changes, we uses learnable feature pyramids
## Warping Layer
we take warping operation to estimate large motion
take the last flow and upsample 2x and warp it with the current feature for cost volume
## Cost Volume Layer
define matching cost as correlation between features of the first image and warped features of the second image
cost volume estimate flow
## Optical flow estiamte
context network to expoilt contextual information to refine optical flow
Input is cost volume, features of the first image and optical flow of the previous layer
use densenet connections to enhance
## Context layer
to effectively to enlarge receptive field
takes estimated flow and features of the second last layer from optical flow estimator and outputs a refined flow
designed based dilated conv




