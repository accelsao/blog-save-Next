---
title: Attention Is All You Need
tags:
  - Attention
categories:
  - Computation and Language
mathjax: true
date: 2018-11-27 08:07:16
---

[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf) -- Learning Notes

<!--more-->


# Model Architecture
The Transformer
![](https://i.imgur.com/lwtePxn.png)

## Encoder and Decoder
Encoder:
N=6 ,Each layer has two sub-layers
The first is multi-head self-attention mechanism,second is simple,position-wise fully connected feed forward network
we uses skip connection for each sub-layer,followed by layer normalization
the output is \\(LayerNorm(x+Sublayer(x))\\),
all sublayer output of dimension \\(d_{model}=512\\)

Decoder:
Also,composed of stack of N=6 identical layer
The different is their is masked multi-head attention
prevent that the predict of position i can depend only on the known position that less than i

## Attention
![](https://i.imgur.com/YDB56S9.png)
### Scale dot product attention
the input consist of queries, keys of dimension d_k,and values of dimension d_v
$$ Attention(Q,K,V)=SoftMax(\frac{QV^T}{\sqrt{d_k}})V $$

two common attention additive and dot product
additive attention outperforms dot product attention without scaling for larger values of \\(d_k\\)
because the dot product grow large in magnitude ,so we scaled it by \\(\frac{1}{\sqrt(d_k)}\\)

### Multi head attention

input Q:queries,K:key,V:value

\\(MultiHead(Q,K,V)=Concat(head1,...headn)W^O\\),
where \\(head_i=Attention(QW_i^Q,KW_i^K,VW_i^V)\\)
we take 8 of scaled dot product attention , see fig

## Position-wise Feed-Forward Networks
$$ FFN(x)=max(0, xW1 + b1)W2 + b2 $$ 
we can look as two 1X1 convolutions
The network is like [FC+ReLu+FC]
size change:
\\(d_{model}=512\\) -> FC -> \\(d_{ff}=2048\\) -> ReLu -> FC -> \\(d_{model}=512\\)

## Positional Encoding
In order to make use of the order of seq, we must inject some position of the token in seq
the paper use sin and cos
$$ PE(pos,2i)=sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})$$
$$ PE(pos,2i+1)=cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}})$$


# Talks
[如何理解谷歌团队的机器翻译新作《Attention is all you need》？](https://www.zhihu.com/question/61077555)

# Reference
[基於Attention之NLP paper - Attention Is All You Need](https://xiaosean.github.io/deep%20learning/nlp/2018/07/13/Attention-is-all-u-need/)