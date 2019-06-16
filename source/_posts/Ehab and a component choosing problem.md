---
title: Codeforces Round 525 E - Ehab and a component choosing problem
tags:
  - dp
  - trees
  - greedy
  - math
categories:
  - Codeforces
mathjax: true
date: 2019-06-15 17:27:50
---

解題心得
<!--more-->

# Problem
給你一個tree 選擇k個component
每個component的 sum of weight / k 要最大
每個node只能屬於一個component
數字範圍 \\(-10^9 \leq w_i \leq 10^9\\)

# Solution
搬出魔王的不等式
**MAX(a1 , a2 , ... , an) >= AVG((a1 , a2 , ... , an)**
等號成立於a1=a2=...=an
換言之 我們只要找出component最大值就好了
假設有多個 我們就記錄數量k
因為是tree 我們可以轉成有根樹root 1然後跑遍所有點 紀錄dp[i] = 以i為根的整個子樹的最大權重
很明顯的如果孩子點的權重dp為負數那不要add最好
跑兩遍dfs 第一遍可以求到最大值
第二遍計算最大值的數量有多少
Q1. 為什麼不能跑一次?
A1. 因為計算數量的時候我們要讓每個點只屬於一個component 所以不能將子樹權重轉移上去根 然後計算權重的時候又需要子樹的權重 所以只能分開處理

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 3e5+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


int n;
vector<int> g[N];
ll a[N];
//int vis[N];
ll dp[N];
ll ans;
int k;
void dfs(int u, int p, int f){
	dp[u] = a[u];
	
	for(const auto& v:g[u]){
		if(v != p){
			dfs(v, u, f);
			dp[u] += max(0LL, dp[v]);
		}
	}

	if(f){
		ans = max(ans, dp[u]);
	}
	else{
		if(ans == dp[u]){
			k++;
			dp[u]=0;
		}
	}

}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	ans = -inf;
	k=0;
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i];
	}
	int u,v;
	for(int i=0;i<n-1;i++){
		cin>>u>>v;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	dfs(1, 0, 1);
	dfs(1, 0, 0);

	cout<<ans*k<<" "<<k<<endl;
}
```
