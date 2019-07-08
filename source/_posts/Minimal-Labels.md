---
title:  Educational Codeforces Round 25 E - Minimal Labels
tags:
  - data structures
  - dfs and similar
  - graphs
  - greedy  
categories:
  - Codeforces
mathjax: true
date: 2019-07-06 16:19:20
---

# Problem
給你graph 要求label
給邊\\(u -> v, label[u] < label[v]\\)
label sequence 是最小字典序
<!--more-->
因為要按照字典序
我們假設目前沒有邊
那我們會得到 `1, 2, 3, 4, 5, ... , n`
現在我們考慮正邊(\\(u<v\\))
add `2 -> 5` and `3 -> 7`
很明顯沒有任何變化
如果加入反邊 (正邊全部清掉)
add `5 -> 2`
ans = `1, 3, 4, 5, 2, 6, ... , n`
如果我們從label1可以處理遇到反邊會遇到困難

不如我們從labeln開始處理
當遇到正邊也可以忽略
當遇到反邊我們先處理out degree=0的點 因為如果outdegree!=0 \\(v < u, label[v] > label[u]\\) 我們勢必需要先給v, label
所以用topological sorting的想法持續拔點即可

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
 
#define F first
#define S second
typedef long long ll;
const int N = 1e5 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;
 
 
int n, m;
vector<int> g[N];
int out[N];
int ID[N];
int main(){
	
 
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m;
	
 
	int u, v;
	for(int i=0;i<m;i++){
		cin>>u>>v;
		g[v].push_back(u);
		out[u]++;
	}
	
		
	priority_queue<int> q;
	for(int i=n;i>=1;i--){
		if(!out[i]){
			q.push(i);
		}
	}
	int id=n;
	while(q.size()){
		int u=q.top();q.pop();
		ID[u]=id--;
		for(const auto& v:g[u]){
			if(!--out[v]){
				q.push(v);
			}
		}
	}
	for(int i=1;i<=n;i++){
		printf("%d ", ID[i]);
	}
	printf("\n");
}
 
/*
100 1
15 12
 
*/
```

