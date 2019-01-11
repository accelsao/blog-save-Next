---
title: RetinaNet
tags:
  - null
categories:
  - null
mathjax: true
date: 2019-01-07 14:34:42
---

RetinaNet Notes
<!--more-->

# Focal Loss
## Cross Entropy Loss
It is designed to address the one-stage objection detection scenario in which there is an extreme imbalenace beteween foreground and background classes during training

cross entropy(CE) loss:
$$
CE(p,y)=
\begin{cases}
-log(p) & \text{if }y = 1 \\
-log(1-p) & \text{otherwise}
\end{cases}
$$
\\(y\in {\pm 1}\\) specifies the ground-truth class and \\(p\in [0,1]\\) is the probability for the class with label \\(y=1\\)

$$
p_t=
\begin{cases}
p & \text{if }y = 1 \\
1 - p & \text{otherwise}
\end{cases}
$$
rewrite \\(CE(p,y) = CE(p_t) = -log(p_t)\\)
add hyperparameter \\(\alpha \in [0,1]\\) for class 1 and \\(1 - \alpha\\) for class -1
$$
CE(p_t) = -\alpha_t log(p_t)
$$

## Focal Loss

we add \\((1-p_t)^{\gamma }\\) to CE, \\(\gamma \geq 0\\)
$$
FL(p_t) = -(1-p_t)^{\gamma}log(p_t)
$$
when \\(p_t\\) is near 0, the loss is unaffected, while the factor goes to 0 and the loss is down-weighted.
They find \\(\gamma = 2\\) work best in experiments

Use \\(\alpha\\)-balanced variant for FL in practice
$$
FL(p_t) = -\alpha_t(1-p_t)^{\gamma}log(p_t)
$$