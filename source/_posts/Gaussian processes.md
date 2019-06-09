---
title: Gaussian processes
tags:
  - Gaussian processes
categories:
  - Machine Learning
mathjax: true
date: 2019-05-01 09:06:13
---

MLAPP Ch.15 公式推導

<!--more-->


# 公式
## [Adjugate matrix](https://en.wikipedia.org/wiki/Adjugate_matrix)
$$
\begin{align}
adj(A)\;A = A\;adj(A) = (det\;A)(I) \\
adj(A) = det(A)A^{-1} \\
A^{-1} = det(A)^{-1}adj(A)
\end{align}
$$
## [Jacobi formula](https://en.wikipedia.org/wiki/Jacobi%27s_formula)
$$
\frac{d}{dt} det A(t) = tr( adj(A(t))\frac{dA(t)}{dt})
$$
## [Woodbury matrix identity](https://en.wikipedia.org/wiki/Woodbury_matrix_identity)
aka. matrix inversion lemma
$$
\left(A+UCV\right)^{-1}=A^{-1}-A^{-1}U\left(C^{-1}+VA^{-1}U\right)^{-1}VA^{-1}
$$

# 15.2.4 Estimating the kernel parameters
$$
\begin{align}
log\;p(y|X) = log\;\mathcal{N}(y|0,K_{y}) = -\frac{1}{2}yK_{y}^{-1}y - \frac{1}{2}log|K_{y}|-\frac{N}{2}log(2\pi)
\tag{15.22}\label{15.22}
\end{align}
$$

$$
\begin{align}
\frac{\partial}{\partial \theta_{j}}log\;p(y|X) = \frac{1}{2}y^{T}K_{y}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}}K_{y}^{-1}y -\frac{1}{2}tr(K_{y}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}})
\tag{15.23}\label{15.23}
\end{align}
$$
Since,
$$
\begin{align}
AA^{-1} = I \\
\nabla_{x}(AA^{-1}) = \nabla_{x}(I) = 0 \\
A\nabla_{x}(A^{-1}) + \nabla_{x}(A)A^{-1} = 0 \\
\nabla_{x}(A^{-1}) = -A^{-1}\nabla_{x}(A)A^{-1} \\
\frac{\partial K_{y}^{-1}}{\partial \theta_{j}} = -K_{y}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}}K_{y}^{-1}
\end{align}
$$
first term of 15.22
$$
\begin{align}
\frac{\partial}{\partial \theta_{j}}(-\frac{1}{2}yK_{y}^{-1}y) &= \frac{1}{2}y^{T}K_{y}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}}K_{y}^{-1}y
\end{align}
$$
and, second term of 15.22
$$
\begin{align}
\frac{\partial}{\partial \theta_{j}}(-\frac{1}{2}log|K_{y}|) &= -\frac{1}{2}\frac{1}{det(K_{y})}\frac{\partial det(K_{y})}{\partial \theta_{j}} \\
&= -\frac{1}{2}\frac{1}{det(K_{y})}tr(adj(K_{y})\frac{\partial K_{y}}{\partial \theta_{j}}) \tag{Jacobi formula} \\
&= -\frac{1}{2}\frac{1}{det(K_{y})}tr(det(K_{y})K_{j}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}}) \tag{Adjugate matrix} \\
&= -\frac{1}{2}tr(K_{j}^{-1}\frac{\partial K_{y}}{\partial \theta_{j}}) \tag{tr(cA) = ctr(A)}
\end{align}
$$


# 15.3.1.2 Computing the posterior predictive
$$
\begin{align}
var[f_{\star}] &\approx k_{\star\star} - k_{\star}^{T}K^{-1}k_{\star} + k_{\star}^{T}K^{-1}(K^{-1} + W)^{-1}K^{-1}k_{\star} \tag{15.46}\\
&= k_{\star\star} - (k_{\star}^{T}K^{-1}k_{\star} - k_{\star}^{T}K^{-1}(K^{-1} + W)^{-1}K^{-1}k_{\star}) \tag{調整括號}\\
&= k_{\star\star} - (k_{\star}^{-1}Kk_{\star}^{-T} + k_{\star}^{-1}W^{-1}k_{\star}^{-T})^{-1} \tag{Woodbury matrix identity} \\
&= k_{\star\star} - (k_{\star}^{-1}(K + W^{-1})k_{\star}^{-T})^{-1} \tag{矩陣結合律}\\
&= k_{\star\star} - k_{\star}^{T}(K + W^{-1})^{-1}k_{\star} \tag{矩陣乘法的Inverse}
\end{align}
$$
Since,
$$
A^{-1} = k_{\star}^{T}K^{-1}k_{\star} \\
U = k_{\star}^{-1} \\
V = k_{\star}^{-T} \\
VA^{-1}U = k_{\star}^{-T}k_{\star}^{T}K^{-1}k_{\star}k_{\star}^{-1} = K^{-1} \\
C^{-1} = W
$$

# 參考
[Gaussian process regression的导出——权重空间视角下的贝叶斯的方法](https://zhuanlan.zhihu.com/p/27554656)
