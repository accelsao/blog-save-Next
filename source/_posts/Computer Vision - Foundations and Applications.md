---
title: Computer Vision - Foundations and Applications
tags:
  - Stanford Vision and Learning Lab
categories:
  - Computer Vision
  
mathjax: false
date: 2018-10-08 21:07:22
---

CV相關Paper,Notes,Links,Course,Book

CS131的課程Notes 包含最新的[2018](http://vision.stanford.edu/teaching/cs131_fall1819/index.html) 和[舊的2016-2017](http://vision.stanford.edu/teaching/cs131_fall1617/index.html)

[My Hw](https://github.com/accelsao/CS131-Computer-Vision-Foundations-and-Applications)


[计算机视觉，图像处理 经典代码paper整理](https://blog.csdn.net/qq_26499769/article/details/79219839)

<!--more-->
# Feature Extraction

## [Distinctive Image Features from Scale-Invariant Keypoints](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf)
### 4步分解

1. Scale-space extrema detection
利用difference-of-Gaussian function(DoG) 識別潛在對於尺度旋轉不變的interest point
2. Keypoint localization
根據穩定性來決定key point的位置和尺度


