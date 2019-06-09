---
title: Codeforces Round 548 D - Steps to One
tags:
  - math
  - number theory
  - probabilities
categories:
  - Codeforces
mathjax: true
date: 2019-06-07 13:56:02
---

解題心得
<!--more-->

# Problem
給你一個N, empty array a
每次隨機選擇1~N的數字加入a
若gcd(a) = 1停止
計算期望長度a是多少

# Solution

1. 暴力法

$$
ans = \sum^{\infty }_{i=2} \sum^{n}_{k=2} (\frac{1}{n})^{i}(\lfloor\frac{n}{k}\rfloor)^{i-1}(n-\lfloor\frac{n}{k}\rfloor)
$$

無限等比級數整理之後剩下
$$
\frac{\frac{n}{k}}{n} + \frac{\frac{n}{k}}{n-\frac{n}{k}}
$$
這是對於某k而言

因為 \\(\lfloor\frac{n}{k}\rfloor\\) 會有重複的比如說 6 會被2,3重複計算
所以我們k從n~2開始計算
每次再扣掉k的倍數

最後在加上長度1的Expected Value \\(\frac{1}{n}\\)

詳細如Code

2.
整個E 相當於 1+E(#randomly generated numbers in a row: gcd>1) (+1的原因是最後要在加上一個數導致gcd=1)
考慮到 inclusion exclusion principle 每個k要乘上莫比烏斯

轉換成 \\(1+ \sum_{i>1} - \mu(i) \text{E(#randomly generated numbers in a row: i∣gcd)}\\)
E(#randomly generated numbers in a row: i∣gcd) 相當於 負二項分布
r次失敗k次成功
negative binomial的期望值=rp/(1-p)
考慮到第一次失敗(導致gcd=1的那個數)就好
ans = 1 + p/(1-p)
\\(p = \frac{\lfloor\frac{n}{k}\rfloor}{n}\\)

# Code
1.
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e6 + 10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


ll pw(ll a, ll b){
	ll t=1;
//	cout<<b<<endl;
	while(b){
		if(b%2==1){
			t = t * a % M;
		}
		a = a * a % M;
		b >>= 1;
//		cout<<b<<endl;
//		cout<<t<<endl;
	}
	return t;
}

int main(){
	
//	cout<<pw(1, M-2)*3<<endl;
//	cout<<pw(2, M-2)*3%M<<endl;

	int n;
	cin>>n;
	
	ll ans = 0;
	
	vector<ll> f(n+1);
	for(int i=n;i>1;i--){
		ll m_k = n/i;
		ll m_k_m = pw(n, M-2) * m_k % M;
		f[i] = (f[i] + m_k_m)%M;
		f[i] = (f[i] + pw(n-m_k, M-2) * m_k % M) % M;
		for(int j=i+i;j<=n;j+=i){
			f[i] = (M + f[i] - f[j]) % M;
		}
//		cout<<f[i]<<endl;
		ans = (ans+f[i])%M;
		
	}
	
	
	ans = (ans+pw(n, M-2))%M;
	cout<< ans <<endl;
}
```
2.
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e6 + 10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


ll pw(ll a, ll b){
	ll t=1;
//	cout<<b<<endl;
	while(b){
		if(b%2==1){
			t = t * a % M;
		}
		a = a * a % M;
		b >>= 1;
//		cout<<b<<endl;
//		cout<<t<<endl;
	}
	return t;
}




int main(){

	int n;
	cin>>n;
	
	ll ans = 1;
	
	
	vector<ll> mu(n+1, 1);
	vector<int> prm(n+1);
	for(ll i=2;i<=n;i++){
		if(prm[i]==0){
			for(ll j=i;j<=n;j+=i){
				mu[j]*=-1;
				prm[j]=1;
			}
			ll k=i*i;
			for(ll j=k;j<=n;j+=k){
				mu[j]=0;
			}
		}
	}
	
//	
//	for(int i=2;i<=n;i++){
//		cout<<mu[i]<<endl;
//	}
	
	for(int i=2;i<=n;i++){
	
		ll p = (n/i) * pw(n, M-2) % M;


		ll a = (-mu[i] * p + M) % M; // -mu p
		ll b = pw((M+1-p)%M, M-2); // 1-p
		ll c = a * b % M; // a*b%M

//		cout<<c<<endl;
		ans = (ans + c % M)%M;

		
//		cout<<ans<<endl;
	}
	
	cout<< ans <<endl;
}
```