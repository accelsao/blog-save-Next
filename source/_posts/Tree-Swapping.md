---
title: Tree Swapping
tags:
  - tree
categories:
  - CS Academy
mathjax: false
date: 2019-07-14 13:14:29
---


解題心得(未完)
# Problem
給你兩色tree 每次可以交換edge上兩點的顏色使得所有相鄰點異色 問最少交換次數 or 無解(-1)

<!--more-->
# Notes

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


int n;
string s;
int a[N];
vector<int> g[N];
int d[N];//delta +son +wrong node
int f[N];//cost +son +|delta|
void dfs(int u,int p,int clr){
//	printf("a[%d]=%d clr:%d\n",u,a[u],clr);
	if(a[u]!=clr){
		d[u]+=clr?1:-1;
	}
	for(const auto& v:g[u])
		if(v!=p){
			dfs(v, u, clr^1);
			d[u]+=d[v];
			f[u]+=f[v];
		}
	f[u]+=abs(d[u]);
}
int main(){


	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	cin>>s;
	int cnt=0;
	for(int i=0;i<n;i++){
		a[i+1]=(s[i]=='B');
	}
	int u, v;
	for(int i=1;i<n;i++){
		cin>>u>>v;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	for(int i=1;i<=n;i++){
		f[i]=d[i]=0;
	}
	int ans=inf;
	dfs(/*u=*/1, /*p=*/0, /*clr=*/0);
	
	
	if(!d[1]){
		ans=min(ans,f[1]);
	}
	for(int i=1;i<=n;i++){
		f[i]=d[i]=0;
	}
	dfs(/*u=*/1, /*p=*/0, /*clr=*/1);
	if(!d[1]){
		ans=min(ans,f[1]);
	}
	if(ans==inf)ans=-1;
	cout<<ans<<endl;
	
	
}

```