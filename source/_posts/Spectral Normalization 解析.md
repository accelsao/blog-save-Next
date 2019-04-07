---
title: Spectral Normalization 解析
tags:
  - Spectral Normalization
categories:
  - Reading Notes
mathjax: true
date: 2019-04-07 10:41:19
---

Spectral Normalization Notes

<!--more-->

# Main
The spectral norm $\sigma(W)$ that we use to regularize discriminator is the largest singular value of $W$
If we apply SVD, the computation is expensive, so we use the power iteration method to approximate $\sigma(W)$
Suppose A is $n\times n$ matrix, eigenvectors $\nu_1,\nu_2,...,\nu_n$, eigenvalues $\lambda_1,\lambda_2,...,\lambda_n$
we have

$$
\begin{align}
Ax &= A(x_1\cdot \nu_1 + x_2\cdot \nu_2 + ... + x_n\cdot \nu_n)\\
&= x_1(\lambda_1 \nu_1) + x_2(\lambda_2 \nu_2) + ... + x_n(\lambda_n \nu_n)
\end{align}
$$

after t times

$$
\begin{align}

A^{t}x &= x_1(\lambda_1^{t} \nu_1) + x_2(\lambda_2^{t} \nu_2) + ... + x_n(\lambda_n^{t} \nu_n)\\
&= \lambda_1^{t} [ \, x_1\nu_1 + x_2( \frac{\lambda_2}{\lambda_1}^{t} )\nu_2 + ... + x_n( \frac{\lambda_n}{\lambda_1}^{t} )\nu_n ] \,
\end{align}
$$

Because $ \lambda_1 > \lambda_1 > ... > \lambda_n $, if not we can add small eps. We get

$$
\lim\limits_{t \to \infty} (\frac{\lambda_i}{\lambda_1})^{t} = 0 , i\neq 1
$$

So we can get
$$
\lim\limits_{t \to \infty} A^{t}x = \lambda_1x_1\nu_1
$$


Note that we need to find **singular value** of $W$, and we know it is equal to eigenvalue of $W^{T}W = \sqrt{\lambda_1}$ ,

Apply power method on $W^{T}W$, we need the largest eigenvalue. Algorithm in [Paper](https://arxiv.org/pdf/1802.05957.pdf)
![](https://i.imgur.com/d6OYQrk.png)
we can find that $u^{t+1} = W^{T}Wu^{t}$ after step 1. The L2normalize wont change the direction. It is fine.

We need $\lambda_1$
$$
W^{T}Wu = \lambda_1u, ||u||_2 = 1 \\
uW^{T}Wu = \lambda_1u^{T}u = \lambda_1 \\
||Wu||_2 = \sqrt{\lambda_1}
$$
Times $u$ on either side of eq.21
$$
u^{T}u = \frac{uWv}{||Wv||_2} \\
1 = \frac{uWv}{\sqrt{\lambda_1}} \\
\sqrt{\lambda_1} = uWv
$$

# Code from pytorch
```py
def compute_weight(self, module, do_power_iteration):
	# NB: If `do_power_iteration` is set, the `u` and `v` vectors are
	#     updated in power iteration **in-place**. This is very important
	#     because in `DataParallel` forward, the vectors (being buffers) are
	#     broadcast from the parallelized module to each module replica,
	#     which is a new module object created on the fly. And each replica
	#     runs its own spectral norm power iteration. So simply assigning
	#     the updated vectors to the module this function runs on will cause
	#     the update to be lost forever. And the next time the parallelized
	#     module is replicated, the same randomly initialized vectors are
	#     broadcast and used!
	#
	#     Therefore, to make the change propagate back, we rely on two
	#     important bahaviors (also enforced via tests):
	#       1. `DataParallel` doesn't clone storage if the broadcast tensor
	#          is alreay on correct device; and it makes sure that the
	#          parallelized module is already on `device[0]`.
	#       2. If the out tensor in `out=` kwarg has correct shape, it will
	#          just fill in the values.
	#     Therefore, since the same power iteration is performed on all
	#     devices, simply updating the tensors in-place will make sure that
	#     the module replica on `device[0]` will update the _u vector on the
	#     parallized module (by shared storage).
	#
	#    However, after we update `u` and `v` in-place, we need to **clone**
	#    them before using them to normalize the weight. This is to support
	#    backproping through two forward passes, e.g., the common pattern in
	#    GAN training: loss = D(real) - D(fake). Otherwise, engine will
	#    complain that variables needed to do backward for the first forward
	#    (i.e., the `u` and `v` vectors) are changed in the second forward.
	weight = getattr(module, self.name + '_orig')
	u = getattr(module, self.name + '_u')
	v = getattr(module, self.name + '_v')
	weight_mat = self.reshape_weight_to_matrix(weight)

	if do_power_iteration:
		with torch.no_grad():
			for _ in range(self.n_power_iterations):
				# Spectral norm of weight equals to `u^T W v`, where `u` and `v`
				# are the first left and right singular vectors.
				# This power iteration produces approximations of `u` and `v`.
				v = normalize(torch.mv(weight_mat.t(), u), dim=0, eps=self.eps, out=v)
				u = normalize(torch.mv(weight_mat, v), dim=0, eps=self.eps, out=u)
			if self.n_power_iterations > 0:
				# See above on why we need to clone
				u = u.clone()
				v = v.clone()

	sigma = torch.dot(u, torch.mv(weight_mat, v))
	weight = weight / sigma
	return weight
```

# Reference
[Spectral Normalization 谱归一化](https://zhuanlan.zhihu.com/p/55393813)
[Spectral Normalization Explained](https://christiancosgrove.com/blog/2018/01/04/spectral-normalization-explained.html)
[Pytorch source code](https://github.com/pytorch/pytorch/blob/master/torch/nn/utils/spectral_norm.py)
