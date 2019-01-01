---
title: Tensorflow
tags:
  - Z Algorithm
  - DP
  - Strings
  - Codeforces
categories:
  - Competitive Programming
mathjax: true
date: 2018-10-15 17:47:27
---

[TensorFlows official documentation](https://www.tensorflow.org/api_docs/python/)
[CS 20: Tensorflow for Deep Learning Research](http://web.stanford.edu/class/cs20si/syllabus.html)
<!--more-->

# Assignment 1
決定直衝作業 記錄各種小東西
## Log設置
```py
import os
os.environ['TF_CPP_MIN_LOG_LEVEL']='2'

'1' default:顯示所有訊息
'2' : warning + error
'3' : error
```

## tf.InteractiveSession
This is convenient in interactive shells and IPython notebooks, as it avoids having to pass an explicit Session object to run ops.
簡單說用InteractiveSession 就不需要Session() 減少代碼量

## [01.q1](https://github.com/accelsao/CS-20-Tensorflow-for-Deep-Learning-Research/blob/master/01/q1_sol.ipynb)
## Reference
[02 Lecture Notes](https://docs.google.com/document/d/1FSPNZFQsnaUVeTo0OQ2RrEZ0f4el9bIGI5sQALbG_F0/edit#)
[03 Lecture Notes](https://docs.google.com/document/d/1kMGs68rIHWHifBiqlU3j_2ZkrNj9RquGTe8tJ7eR1sE/edit#)

## [03_logreg](https://github.com/chiphuyen/stanford-tensorflow-tutorials/blob/master/examples/03_logreg.py)

## MNIST 99% Tensorflow and logistic

## CBOW+Skip gram
