---
title: attention overview
tags:
  - null
categories:
  - null
mathjax: true
date: 2018-11-27 08:07:16
---

attention overview

<!--more-->


# [Recurrent Models of Visual Attention](https://arxiv.org/pdf/1406.6247.pdf)
In image classification and object detection tasks, usually comes at a high computational cost both at training and testing
It takes days even on multiple GPUs even though the inputs are downsampled to reduce computation.

Human doesnt tend to process a whole sceen in its entirely at once, instead , select interest of region

## RAM(Recurrent Attention Model)
![](https://i.imgur.com/QqnPCGS.png)
the model is RNN, \\(f_h(\theta _h)\\) takes the glimpse representation as inputs and combine the internal representation \\(h_{t-1}\\)
produc the new internal state \\(h_{t}\\)
the location network \\(f_l(\theta _l)\\)use \\(h_{t}\\) to produce \\(l_t\\) for the next steps
\\(f_a(\theta _a)\\) is action(classify) network

# [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/pdf/1409.0473.pdf)
The first works in NLP that use attentions
NMT is based on seq to seq model

