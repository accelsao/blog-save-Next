---
title: Computer Vision Paper Reading List 1901
tags:
  - Object Detection
  - 6DoF Pose Estimation
  - 3D Shape Reconstructtion
  - Deep Generative Models
  - Deep Learning
  - Computer Vision
categories:
  - Learning Notes
mathjax: false
date: 2019-01-17 15:13:36
---

Computer Vision Paper Reading List

<!--more-->

# Challenge/Dataset
**[[ CoCo ](http://cocodataset.org/#home)]** | **[[ KITTI ](http://www.cvlibs.net/datasets/kitti/index.php)]** | **[[ WIDER FACE ](http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/WiderFace_Results.html)]**
**[[ QMUL-SurvFace ](https://qmul-survface.github.io/)]**

# Face Detection

# NMS

- **[ Softer-NMS ]** | ** [ Softer-NMS: Rethinking Bounding Box Regression for Accurate Object Detection ]** | [`[pdf]`](https://arxiv.org/pdf/1809.08545.pdf)

# Attention in Vision

- **[ FA ]** | ** [ Factorized Attention: Self-Attention with Linear Complexities ]** | [`[pdf]`](https://arxiv.org/pdf/1812.01243v2.pdf)

# GAN

- **[ WGAN-GP ]** | ** [ Improved Training of Wasserstein GANs ]** | **[ NeurIPS 2017 ]** | [`[pdf]`](https://arxiv.org/pdf/1704.00028.pdf)
- **[ GANimation ]** | ** [ GANimation: Anatomically-aware Facial Animation from a Single Image ]** | [`[pdf]`](https://arxiv.org/pdf/1807.09251.pdf)

# Object Detection

- **[ CornerNet ]** | ** [ CornerNet: Detecting Objects as Paired Keypoints ]** | [`[pdf]`](https://arxiv.org/abs/1808.01244) | [`[code]`](https://github.com/princeton-vl/CornerNet)
- **[ FishNet ]** | ** [ FishNet: A Versatile Backbone for Image, Region, and Pixel Level Prediction ]** | **[ NeurIPS 2018 ]** | [`[pdf]`](https://arxiv.org/pdf/1901.03495.pdf) | [`[code]`](https://github.com/kevin-ssy/FishNet)
- **[ HTC ]** | ** [ Hybrid Task Cascade for Instance Segmentation ]** | [`[pdf]`](https://arxiv.org/pdf/1901.07518.pdf)

# DensePose

- **[ DensePose ]** | ** [ DensePose: Dense Human Pose Estimation In The Wild ]** | [`[pdf]`](https://arxiv.org/pdf/1802.00434.pdf) | [`[code]`](https://github.com/facebookresearch/DensePose)
- **[ Parsing R-CNN ]** | ** [ Parsing R-CNN for Instance-Level Human Analysis ]** | [`[pdf]`](https://arxiv.org/pdf/1811.12596.pdf)

# Instance Segmentation

- **[ Mask R-CNN ]** | [`[pdf]`](https://arxiv.org/pdf/1703.06870.pdf) | [`[code]`](https://github.com/facebookresearch/maskrcnn-benchmark)

# Panoptic Segmentation

- **[ AUNet ]** | ** [ Attention-guided Unified Network for Panoptic Segmentation ]** | [`[pdf]`](https://arxiv.org/pdf/1812.03904.pdf)
- **[ Panoptic FPN ]** | ** [ Panoptic Feature Pyramid Networks ]** | [`[pdf]`](https://arxiv.org/pdf/1901.02446.pdf)
- **[ UPSNet ]** | ** [ UPSNet: A Unified Panoptic Segmentation Network ]** | [`[pdf]`](https://arxiv.org/pdf/1901.03784.pdf)

# 3D Classification and Segmentation

- **[ PointNet ]** | ** [ PointNet: Deep Learning on Point Sets for 3D Classification and Segmentation ]** | **[ CVPR 2017 ]** | [`[pdf]`](https://arxiv.org/pdf/1612.00593.pdf) | [`[code]`](https://github.com/charlesq34/pointnet)
- **[ PointNet++ ]** | ** [ PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space ]** | **[ NIPS 2017 ]** | [`[pdf]`](https://arxiv.org/abs/1706.02413)

# 6DoF Pose Estimation

- **[ PoseCNN ]** | ** [ PoseCNN: A Convolutional Neural Network for 6D Object Pose Estimation in Cluttered Scenes ]** | [`[pdf]`](https://arxiv.org/pdf/1711.00199.pdf)
- **[ PointFusion ]** | ** [ PointFusion: Deep Sensor Fusion for 3D Bounding Box Estimation ]** | [`[pdf]`](https://arxiv.org/pdf/1711.10871.pdf)
- **[ DenseFusion ]** | ** [ DenseFusion: 6D Object Pose Estimation by Iterative Dense Fusion ]** | [`[pdf]`](https://arxiv.org/pdf/1901.04780.pdf) | [`[code&video]`](https://sites.google.com/view/densefusion/)

# 3D Shape Reconstruction

- **[ MarrNet ]** | ** [ MarrNet: 3D Shape Reconstruction via 2.5D Sketches ]** | **[ NeurIPS 2017 ]** | [`[pdf]`](https://arxiv.org/pdf/1711.03129.pdf)
- **[ AtlasNet ]** | ** [ AtlasNet: A Papier-Mache Approach to Learning 3D Surface Generation ]** | **[ CVPR 2018 ]** | [`[pdf]`](https://arxiv.org/pdf/1802.05384.pdf) | [`[code]`](https://github.com/ThibaultGROUEIX/AtlasNet)
- **[ ShapeHD ]** | ** [ Learning Shape Priors for Single-View 3D Completion and Reconstruction ]** | **[ ECCV 2018 ]** | [`[pdf]`](https://arxiv.org/pdf/1809.05068.pdf)
- **[ GenRe ]** | ** [ Learning to Reconstruct Shapes from Unseen Classes ]** | **[ NeurIPS 2018 ]** | [`[pdf]`](https://arxiv.org/pdf/1812.11166.pdf) | [`[code]`](https://github.com/xiumingzhang/GenRe-ShapeHD)

# Deep Generative Models

- **[ Augmented CycleGAN ]** | ** [ Augmented CycleGAN: Learning Many-to-Many Mappings from Unpaired Data ]** | [`[pdf]`](https://arxiv.org/pdf/1802.10151.pdf)
- **[ VON ]** | ** [ Visual Object Networks: Image Generation with Disentangled 3D Representation ]** | **[ NeurIPS 2018 ]** | [`[pdf]`](https://arxiv.org/pdf/1812.02725.pdf) | [`[code]`](https://github.com/junyanz/VON)