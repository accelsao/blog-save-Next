---
title: Learning to Segment Every Thing
tags:
  - null
categories:
  - Computer Vision
mathjax: false
date: 2018-11-26 08:51:09
---

一般 instance segmentation 需要所有數據被instance mask標註 但這樣的資料很難取得
這篇的目的是用部分監督學習和weight transfer learning來處理large scale instance segmentation

<!--more-->

instance segmentation通常需要strong supervision 但它既少而且難以取得
相對的 bounding-box 就很豐富且易取得

解決問題:
*Is it possible to train highquality instance segmentation models without complete instance segmentation annotations for all categories?*
解決方法:
1. 部分監督學習
2. 新的weight transfer learning

建構 partially supervised instance segmentation task
(1)拿一小部分的instance mask 剩餘都用bounding box
這樣的好處是我們不需要太多instance mask annotations 就可以提高instance segmentation的規模

為了處理 partially supervised instance segmentation 
我們用Mask RCNN裡頭的*transfer learning*
用Mask RCNN的原因是它將instance segmentation 問題分解成 bounding-box object prediction和 mask prediction
這兩個子問題用 "heads" 網路來訓練,bounding-box head的參數



## Weight prediction and task transfer learning

