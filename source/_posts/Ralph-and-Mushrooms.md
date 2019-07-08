---
title: Codeforces Round 447 E - Ralph and Mushrooms
tags:
  - dp
  - graphs
categories:
  - Codeforces
mathjax: true
date: 2019-07-05 15:34:40
---

解題心得
以前的題目再做一次 還是沒想到怎麼處理
<!--more-->

# Notes
n個點, m條edge
很明顯有環的話 我們可以**全拿**
所以我們要對DAG縮環 從新編號建圖 O(n)
所以環上的邊 可以拿到 \\(k+(k-1)+(k-1-2) ... (k-1-2,...,-m)\\) and  \\(k-1-2,...,-m) \geq 0\\)
用二分找到邊界點 然後預處理 只要處理到\\(2\times 10^4\\)
二分 \\(O(log(k))\\), \\(k=2\times 10^4\\)
全部 \\(O((n+m)log(k))\\), 不用二分也可以用數學解\\(O(1)\\), 最快只要\\(O(n+m)\\)

# Code
放後來寫的用二分比較久 但不需要計算數學解
```cpp
#include <bits/stdc++.h>
using namespace std;

#define F first
#define S second
typedef long long ll;
const int N = 1e6 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;

int n, m;
ll dis[N];
stack<int> st;
int vis[N], lw[N], df=0, gp[N], scc=0;
vector<pair<int ,int>> g[N];
vector<pair<ll, int>> new_g[N];
int cyc[23456];
ll sum_cyc[23456];
ll f[N];// sum of scc
void dfs(int u, int p=-1){
	vis[u]=lw[u]=++df;
	st.push(u);
	
	for(const auto& e:g[u]){
		int v=e.F;
		if(!vis[v]){
			dfs(v, u);
			lw[u]=min(lw[u], lw[v]);
		}
		else if(!gp[v]){
			lw[u]=min(lw[u], vis[v]);
		}
	}
	if(vis[u]==lw[u]){
		gp[u]=++scc;
		int cur=st.top();
		while(cur!=u){
			gp[cur]=scc;
			st.pop();
			cur=st.top();
		}
		st.pop();
	}
}

ll get(ll k){
	int l=0, r=20000, m, z;
	while(l<=r){
		m=(l+r)>>1;
		if(k>=cyc[m]){
			l=m+1;
			z=m;
		}
		else{
			r=m-1;
		}
	}
	ll sum=k*(z+1)-sum_cyc[z];
	return sum;
}
ll dag(int u){
	if(dis[u])return dis[u];
	ll _d=0;
	for(const auto& e:new_g[u]){
		_d=max(_d, dag(e.S) + e.F);
	}
	return dis[u]=_d+f[u];
}
int main(){
	
	for(int i=1;i<=20000;i++){
		cyc[i]=cyc[i-1]+i;
		sum_cyc[i]=sum_cyc[i-1]+cyc[i];
	}
//	printf("%lld\n",get(4));

	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m;
	int u,v,d;
	for(int i=0;i<m;i++){
		cin>>u>>v>>d;
		g[u].push_back({v, d});		
	}
	int s;
	cin>>s;
	dfs(s);
	for(int u=1;u<=n;u++){
		for(const auto& e:g[u]){
//			printf("%d %d\n", u, e.F);
			int _u=gp[u], _v=gp[e.F];
//			printf("%d %d\n", _u, _v);
			if(_u != _v){
				new_g[_u].push_back({e.S, _v});
			}
			else{
				f[_u]+=get(e.S);
			}
		}
	}
	
//	for(int i=1;i<=scc;i++)
//		cout<<f[i]<<endl;
	
	ll ans=dag(gp[s]);
	printf("%lld\n", ans);
}

/*
5 5
1 2 4
2 3 4
3 4 5
4 2 1
4 5 20
1

*/

```