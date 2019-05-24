---
title: Codeforces Round 561 (Div. 2) D - Cute Sequences
tags:
  - Math
categories:
  - Codeforces
mathjax: true
date: 2019-05-19 20:43:47
---

解題心得
<!--more-->
# Problem
給你a, b, m
構建一個數列符合以下條件


1. \\(n <= 50\\)
2. \\(A_1 = a\\) and \\(A_n = b\\)
3. Let
$$
S_i = \sum_{j=1}^{i}A_j
$$
, $$ 1 \leq S_i - S_{i-1} \leq m $$

若無解輸出 -1

# 解法
數列會長這樣: [a, a+\\(r_1\\), 2a+\\(r_1\\)+\\(r_2\\) ...]
若我們 r 都選擇 1 \\(X_n = 2^{n-2}(a+1)\\)
若選擇 m 則為 \\(X_n = 2^{n-2}(a+m)\\)
所以要找到一個 n 可以讓 b 介於兩者之間
窮舉可能長度, 若發現某 len 的最大值超過 b 但最小值也超過 b 則保證無解, 因為加長len只會更大

對於可以 len 我們可以構建出一個可行解
我們發現
$$
X_n = 2^{n-2}a + 2^{n-3}r_1 + 2^{n-4}r_2 + ... +  r_{n-1} + r_n
$$
目標是求出合理的r1 ~ rn
我們嘗試把它改成以下形式
$$
X_n = 2^{n-2}(a + C) + r, r < 2^{n-2}
$$
r 就相當於 \\(b \mod 2^{n-2}\\) 一堆1010
那我們怎麼計算 C ?
將 \\(b / 2^{n-2}\\) - a 就是 C
得到 C 以後
在計算r1~rn
let r = \\(d_{n-3}d_{n-4}...d_0\\) 每個都 1 or 0
$$
\begin{align}
X_n &= 2^{n-2}a + 2^{n-2}C + r \\
&= 2^{n-2}a + 2^{n-3}C + 2^{n-4}C + ... + 2C + C + C + 2^{n-3}r_{n-3} + 2^{n-4}r_{n-4} + ... + 2r_1 + r_0 \tag{把 r 的 bit 拆開} \\ 
&= 2^{n-2}a + 2^{n-3}(C + r_{n-3}) + 2^{n-4}(C + r_{n-4}) + ... + 2(C + r_1) + (C+r_0) + C \tag{合併 對應 ri} \\
\end{align}
$$

所以算出 C 之後 每次只要加上 r 的對應 bit, (1 or 0) 就可以算出ri

# Code
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 10;

int main(){
	long long a, b, m;
	int T;
	cin>>T;
	while(T--){
		cin>>a>>b>>m;
		if(a==b){
			printf("1 %lld\n", a);
			continue;
		}
		int len = -1;
		for(int i=2;i<=50;i++){
			long long t = 1LL<<(i-2);
			if(b <= t * (a+m)){
				if(t * (a+1) <= b){
					len = i;
				}
				break;
			}
		}
		if(len == -1){
			puts("-1");
			continue;
		}
		int n=len;
		long long k = (b>>(n-2)) - a;
		//printf("k: %lld\n", k);
		printf("%d %lld", n, a);
		long long s=a;
		for(int i=n-3; ~i; i--){
			long long r = k + ((b>>i)&1);
			printf(" %lld", s + r);
			s <<= 1;
			s += r;
		}
		printf(" %lld\n", b);
	}
}
```
