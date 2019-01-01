---
title: Deep Learning - Reading Notes
tags:
- Deep Learning
- Linear Algebra
categories:
- Artificial Intelligence
- Machine Learning
- Deep Learning
mathjax: true
date: 2018-10-09 19:28:12
---

An MIT Press book
Ian Goodfellow and Yoshua Bengio and Aaron Courville
[website](https://www.deeplearningbook.org/)

<!--more-->

# Part I: Applied Math and Machine Learning Basics
## Linear Algebra

### Scalars, Vectors, Matrices and Tensors
* Scalars : 純量
* Vector : 向量
`'-' sign` 是補集
Let S={1,3,6}
\\(x_{-S}\\)is vector 包含所有elements 除了S {1,3,6}
* Matrices : 矩陣
* Tensors : 張量

### Transpose
$$A^\top_{i,j}=A_{j,i}$$

### Multiplying Matrices and Vectors
$$A(B+C)=AB+AC$$
$$A(BC)=(AB)C$$
$$(AB)^\top=B^\top A^\top$$

### Eigendecomposition

An eigenvector of a square matrix A is a non-zero vector v such that multiplication
by A alters only the scale of v:
$$ Av = λv $$
The scalar λ is known as the eigenvalue corresponding to this eigenvector.

Suppose that a matrix A has n linearly independent eigenvectors, {v(1) , . . . ,v(n)}, with corresponding eigenvalues {λ1, . . . , λn}
We may concatenate all of the eigenvectors to form a matrix V with one eigenvector per column: V = [v (1) , . . . ,v(n)]
Likewise, we can concatenate the eigenvalues to form a vector λ = [λ1 , . . . ,λn]
The eigendecomposition of A is then given by:
$$A = V diag(λ)V^{−1}$$

### Singular Value Decomposition
Every real matrix has a singular value decomposition

$$A = UDV^\top $$
Suppose that A is an \\(m\times n\\) matrix. Then U is defined to be an \\(m\times m\\) matrix,
D to be an \\(m\times n\\) matrix, and V to be an \\(n\times n\\) matrix.

The matrices U and V are both defined to be orthogonal matrices.
The matrix D is defined to be a diagonal matrix. Note that D is not necessarily square

The elements along the diagonal of D are known as the singular values of
the matrix A. The columns of U are known as the left-singular vectors. The
columns of V are known as as the right-singular vectors.

### The Moore-Penrose Pseudoinverse

The pseudoinverse of A is defined as a matrix

$$ A^+ =\lim_{a\to 	0}(A^\top A+\alpha I)^{-1}A^\top $$

Practical algorithms for computing the pseudoinverse are not based on this definition,
but rather the formula

$$ A^+ =VD^+U^\top $$
where U, D and V are the singular value decomposition of A, and the pseudoinverse
D+ of a diagonal matrix D is obtained by taking the reciprocal of its non-zero
elements then taking the transpose of the resulting matrix

When A has more columns than rows, then solving a linear equation using the
pseudoinverse provides one of the many possible solutions. Specifically, it provides
the solution \\(x = A^+ y\\) with minimal Euclidean norm \\(||x||_2\\) among all possible
solutions.
When A has more rows than columns, it is possible for there to be no solution.
In this case, using the pseudoinverse gives us the x for which Ax is as close as
possible to y in terms of Euclidean norm \\(||Ax − y||_2\\)



## Machine Learning Basics
### Challenges Motivating Deep Learning
一些簡單的機器學習算法像 speech recognition or object recognition 就沒辦法處理得很好
尤其是高維度的資料,新樣本上的泛化特別困難,計算Cost也是巨大的而deep learning就是設計來解決這類問題
#### The curse of dimensionality
一維需要\\(O(n)\\),2維需要\\(O(n^2)\\),d維需要\\(O(n^d)\\) 複雜度是指數級的
Because in high-dimensional spaces the number of configurations is huge, much larger than our number of examples, a typical grid cell has no training example associated with it. How could we possibly say something meaningful about these new configurations?
#### Local Constancy and Smoothness Regularization
為了良好的泛化,機器學習算法需要有prior來指引學習
先前,我們有看到prior直接影響函數本身,透過這個影響改變參數
最常見的有**smoothness prior** or **local constancy prior**
這裡介紹為甚麼**smoothness prior**不足以解決這些AI-level任務
有很多不同方法來表示學習函數應該要有平滑或局部不變的prior
所有不同的方法被設計來提升學習進度來學習\\(f^\ast\\),滿足\\(f^\ast \approx f^*(x+\epsilon)\\)
most configurations \\(x\\) and small change \\(\epsilon\\)
也就是說當我們知道一個good input x,就有另外一個好的鄰近x ,再多就有一堆好的input

# Part II Deep Networks: Modern Practices
## Deep Feedforward Networks
Deep feedforward networks, also often called feedforward neural networks,or multilayer perceptrons (MLPs),