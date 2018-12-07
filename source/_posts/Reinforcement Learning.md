---
title: Reinforcement Learning 
tags:
  - Reinforcement Learning
categories:
  - Reinforcement Learning
mathjax: true
date: 2018-11-07 13:12:38
---

Reinforcement Learning: An Introduction
Richard S. Sutton and Andrew G. Barto

-- Reading Notes
<!--more-->

[【专知荟萃23】深度强化学习RL知识资料全集（入门/进阶/论文/综述/代码/专家，附查看）](https://cloud.tencent.com/developer/article/1090566)


# Multi-armed Bandits
## k-armed bandit problem
you re faced repeatedly with k choices, after each choice you receive a numerical reward .
Your objective is to maximize the totol expected score
you can find that the optimal choose if you knew the value of each action.
we say you are exploting your current knowledge if you choose the optimal choice, otherwise we call exploring
exploring may get the greater total reward in the long run
The need to balance exploration and exploitation is a distinctive challenge that arises in RL

## Incremental Implementation
Let \\(R_i\\) denote the reward received after the ith selection of this action,and let \\(Q_n\\) denote the estimate of its action
value after it has been selected \\(n-1\\) times
$$ Q_n\doteq \frac{R_1+R_2+...+R_{n-1}}{n-1}$$

Given \\(Q_n\\) and the nth reward,\\(R_n\\)
$$
\begin{split}
Q_{n+1}&=\frac{1}{n}\sum_{i=1}^{n}R_i\\
&=\frac{1}{n}(R_n+\sum_{i=1}^{n-1}R_i)\\
&=\frac{1}{n}(R_n+(n-1)\frac{1}{n-1}\sum_{i=1}^{n-1}R_i)\\
&=\frac{1}{n}(R_n+(n-1)Q_n)\\
&=\frac{1}{n}(R_n+nQ_n-Q_n)\\
&=Q_n+\frac{1}{n}[R_n-Q_n]

\end{split}
$$
![](https://i.imgur.com/rWBnfzw.png)

## Tracking a Nonstationary Problem
For nonstationary, it makes sense to give more weight to recent reward than to long-past rewards.
we modified the incremental update rule to 
$$Q_{n+1}\doteq Q_n+\alpha[R_n-Q_n]$$
where the step-size parameter \\(\alpha \in (0,1]\\) is constant

$$
\begin{split}
Q_{n+1}&=Q_n+\alpha[R_n-Q_n]\\
&=\alpha R_n+(1-\alpha)Q_n\\
&=\alpha R_n+(1-\alpha)[\alpha R_{n-1}+(1-\alpha)Q_{n-1}]\\

&=\alpha R_n+(1-\alpha)[\alpha R_{n-1}+(1-\alpha)^2\alpha R_{n-2}+...+(1-\alpha)^{n-1}\alpha R_{1}+(1-\alpha)^{n}Q_1\\

&=(1-\alpha)^nQ_1+\sum_{i=1}^n\alpha(1-\alpha)^{n-i}R_i

\end{split}
$$


## Optimistic Initial Values
