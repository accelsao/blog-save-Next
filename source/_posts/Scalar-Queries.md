---
title: Educational Codeforces Round 65 F - Scalar Queries
tags:
  - null
categories:
  - null
mathjax: true
date: 2019-06-25 16:02:42
---

解題心得
<!--more-->

# Problem
	定義f(l,r)= sum of i from l to r a[i] * (a[i]在a[l]~a[r]中第K大的K)
	a=[9,2,3,5]
	f(1,4) = 9 * 4 + 2 * 1 + 3 * 2 + 5 * 3
	求sum of f(l,r) 1<=l<=r<=n

# Solution
因為算式複雜 直接引用[tutorial](https://codeforces.com/blog/entry/67058)

![](https://i.imgur.com/BWRlAW8.png)

最後結果為計算以下
$$
\sum^{n}_{i=1}a_{i} [\; (n-i+1)\sum_{1\leq l\leq i} lower(l,i,a_{i}) + i\sum_{i\leq r\leq n} lower(i,r,a_{i})\; ] + \sum^{n}_{i}a_{i}\cdot i\cdot (n+1-i)
$$

最後一個sum簡單
前面的部分 我們由小數字開始處理 用BIT來計算比當前index小的數字有多少 並且index j的總和有多少 同理也計算比當前index大的部分

# Notes
想法上是當你是第K大的時候 代表所以K-1個比你小的數都貢獻1, 算上自己總和就是剛好K次weight 還有當aj貢獻給ai, 如果aj在左邊則l=(1,aj)都繪算上貢獻 如果aj在右邊則r=(aj,n)都繪算上貢獻


# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 5e5+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e17;


int n;
ll f[N][2];//0:number 1:weight
ll mul(ll a, ll b){
	return a*b%M;
}
ll add(ll a, ll b){
	ll t=a+b+M;
	while(t>=M){
		t-=M;
	}
	return t;
}
ll BITnum(int x){
	int cnt=0;
	while(x){
		cnt += f[x][0];
		x -= x&-x;
	}
	return cnt;
}
ll BITget(int x){
	ll res=0;
	while(x){
		res = add(res, f[x][1]);
		x -= x&-x;
	}
	return res;
}
void BITadd(int x, int v){
	while(x<=n){
		f[x][0] ++;
		f[x][1] = add(f[x][1], v);
		x += x&-x;
	}
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	vector<pair<ll,int>> a(n);
	for(int i=0;i<n;i++){
		cin>>a[i].first;
		a[i].second=i+1;
	}
	sort(a.begin(),a.end());;
	ll res=0;
	for(int i=0;i<n;i++){
		ll lhs = BITget(a[i].second);
		ll tmp=mul(lhs,(n+1-a[i].second));
		ll rhs = add(BITget(n), -lhs);
		int num = BITnum(a[i].second);
		num = i - num;
		rhs = add(mul(n+1, num), -rhs);
		rhs = mul(rhs, a[i].second);
		tmp = add(tmp, rhs);
		res = add(res, mul(tmp, a[i].first));
		BITadd(a[i].second, a[i].second);
		res = add(res, mul(a[i].first, mul(a[i].second, n+1-a[i].second)));

	}
	cout<<res<<"\n";
}
```