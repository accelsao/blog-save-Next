---
title: Chain Matrix Multiplication
tags:
  - Math
  - Matrix Multiplication
  - Pytorch
categories:
  - Algorithms
mathjax: false
date: 2019-04-07 16:59:37
---

Chain Matrix Multiplication 演算法
多個matrix相乘的**順序**會大大的影響速度 尤其是DL這種data scale

<!--more-->

# Tensor chain_matmul
```cpp
Tensor chain_matmul(TensorList matrices) {
  checkAllSameDim(matrices, 2);

  if (matrices.size() == 1) { // 直接Return
    return matrices[0];
  } else if (matrices.size() == 2) { //直接相乘
    return at::mm(matrices[0], matrices[1]);
  } else if (matrices.size() == 3) { //看是AB先好還是BC先好
    return _chain_matmul_three_matrices(matrices);
  } else {

    // Following the algorithm in Chapter 15.2 : Introduction to Algorithms, Cormen et al.
    // Minor modifications have been made to accommodate zero-indexing
    auto n = matrices.size();

    // Dim vector - the length of which is n + 1. Note that for matrix multiplication, there
    // needs to a common dimension between the multiplicands, hence for n matrices, there are
    // n + 1 values. The values p_{i} and p_{i + 1} correspond to the dimensions of matrix i in
    // the chain (zero-indexed)
    std::vector<int64_t> p;
    p.push_back(matrices[0].size(0));
    for (int64_t i = 0; i < n; i++) {
      p.push_back(matrices[i].size(1));
    }

    // Cost matrix - an element m[i, j] of this matrix corresponds to the minimum cost of
    // parenthesizing matrices A_{i} to A_{j}. By this definition m[i, i] = 0 for all i
    // m[i, j] is filled using the substructure property of the algorithm, meaning:
    // m[i, j] = min_{i <= k < j} m[i, k] + m[k, j] + p_{i-1}p_{k}p_{j}
    std::vector<std::vector<int64_t>> m(n, std::vector<int64_t>(n, 0));

    // Auxiliary table for constructing the order
    // s[i, j] stores the index k at which the optimal split is obtained
    std::vector<std::vector<int64_t>> s(n, std::vector<int64_t>(n));

    // j and q are used repetitively in the algorithm below
    int64_t j, q;

	/*
		蠻暴力的計算區間的DP
		length 從1~n-1
		i~j 窮舉所有中間點k
	*/
	
    for (int64_t l = 1; l < n; l++) {
      for (int64_t i = 0; i < n - l; i++) {
        j = i + l;
        m[i][j] = std::numeric_limits<int64_t>::max();
        for (int64_t k = i; k < j; k++) {
          q = m[i][k] + m[k + 1][j] + p[i] * p[k + 1] * p[j + 1];
          if (q < m[i][j]) {
            m[i][j] = q;
            s[i][j] = k;
          }
        }
      }
    }

    // We use the result from the algorithm to compute the matrix chain product via recursion
    return _chain_matmul_general(matrices, s, 0, n - 1);
  }
}

```

# 三個matrix的情況
```cpp
static inline Tensor _chain_matmul_three_matrices(TensorList matrices) {
  int64_t a = matrices[0].size(0);  // This is the first dimension
  int64_t b = matrices[1].size(0);  // This is the common dimension between the first two matrices
  int64_t c = matrices[2].size(0);  // This is the common dimension between the last two matrices
  int64_t d = matrices[2].size(1);  // This is the last dimension

  // The matrices are of size (a x b), (b x c), (c x d)
  // cost_1 is the cost of parenthesizing (a x b) and (b x c) and then combining (c x d)
  // cost_2 is the cost of parenthesizing (b x c) and (c x d) and then combining (a x b)
  int64_t cost_1 = (a * c) * (b + d);  // abc + acd = ac(b+d)
  int64_t cost_2 = (b * d) * (a + c);  // bcd + abd = bd(c+a)	

  if (cost_1 > cost_2) {
    return at::mm(matrices[0], at::mm(matrices[1], matrices[2]));
  } else {
    return at::mm(at::mm(matrices[0], matrices[1]), matrices[2]);
  }
}
```

# 遞迴計算matrix的結果
```cpp
static inline Tensor _chain_matmul_general(TensorList matrices, std::vector<std::vector<int64_t>>& order, int64_t i, int64_t j) {
  if (i == j)
    return matrices[i];
  else
    return at::mm(_chain_matmul_general(matrices, order, i, order[i][j]), _chain_matmul_general(matrices, order, order[i][j] + 1, j));
}
```

# Reference
[pytorch/aten/src/ATen/native/LinearAlgebra.cpp](https://github.com/pytorch/pytorch/blob/master/aten/src/ATen/native/LinearAlgebra.cpp)
[Lecture12 Pdf](https://home.cse.ust.hk/~dekai/271/notes/L12/L12.pdf)
