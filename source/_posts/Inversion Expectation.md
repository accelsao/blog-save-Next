---
title: Educational Codeforces Round 57 F - Inversion Expectation
tags:
  - null
categories:
  - Codeforces
mathjax: true
date: 2019-06-13 17:33:00
---

解題心得
<!--more-->

又是期望值問題

# Problem

給你array a, 其中有-1, 可以隨機選擇1~n 但最終arr需是permuation的一種 保證非-1的數字只會出現一次
inversion = i > j and a[i] < a[j] (逆序數對)
[3, 1 ,2] = 2
問逆序數對的期望值是多少個

# Solution
首先分3種情況
1. 都是-1的期望貢獻
我們可以兩兩考慮 \\(n*(n-1)\\)種組合裡面剛好會有一半是逆序 因為是permutaion
共有C(n,2)個pair 各是\frac{1}{2}的期望值
\\(ans = \frac{cnt(-1) \times (cnt(-1) - 1)}{2} \times \frac{1}{2}\\)
2. 一個-1, 一個已知數
很間單就是這樣(-1的數量) * (其中比x大的數 / cnt(-1))
3. 兩個都是常數
用BIT維護可以得到\\(<=x\\)的數量

>Q. 為什麼可以倆倆獨立計算呢 A. 假設 a1=n, a2=n-1, 雖然a3只有n-2個可能但是我們計算pair(a1,a2)根本不需要考慮其他n-2的數是什麼
**我們只在乎這兩個pair的數字** 然後這兩個pair又會有\\(n \* (n-1)\\)種可能 其中又剛好1/2是逆序 所以整體就是\\(\frac{1}{2} \times\\) (有多少pair)



# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5 + 10;
const int M=998244353;
typedef long long ll;


ll pw(ll a, ll b){
	ll t = 1;
	while(b){
		if(b&1){
			t = t * a % M;
		}
		a = a * a % M;
		b >>= 1;
	}
	return t;
}

int a[N];
int b[N];
ll gt[N];

int f[N], n;

void add(int x){
	while(x <= n){
		f[x]++;
		x += x&-x;
	}
}
int get(int x){
	int s=0;
	while(x){
		s+=f[x];
		x -= x&-x;
	}
	return s;
}

int main(){
	cin>>n;

	for(int i=1;i<=n;i++){
		cin>>a[i];
		if(a[i] != -1){
			b[a[i]] = 1;
		}
	}
	
	ll num = 0;
	for(int i=n; i>=1; i--){
		gt[i] = num;
		num += !b[i];
	}
	
	ll ans = num * (num - 1) % M * pw(4, M - 2) % M;
	ll z;
	ll inv = pw(num, M-2);
	int lf = 0;
	for(int i=1;i<=n;i++){
		if(a[i] == -1){
			lf++;
		}
		else{
			z = inv * gt[a[i]] % M;
			ans = (ans + z * lf % M) % M;
		}
	}
	
	int rt = 0;
	for(int i=n; i>=1; i--){
		if(a[i] == -1){
			rt++;
		}
		else{
			z = inv * (num - gt[a[i]]) % M;
			ans = (ans + z * rt % M) % M;
		}
	}
	int cur=0;
	for(int i=1;i<=n;i++){
		if(a[i] != -1){
			ans = (ans + (cur - get(a[i]))) % M;
			cur++;
			add(a[i]);
		}
	}
	cout<<ans<<endl;
}
```