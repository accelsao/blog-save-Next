---
title: Tarjan Algorithm to find Strongly Connected Components
tags:
  - Tarjan
  - Strongly Components Components
categories:
  - Algorithm
mathjax: false
date: 2019-03-22 14:25:30
---

[GeeksforGeeks](https://www.geeksforgeeks.org/tarjan-algorithm-find-strongly-connected-components/)

<!--more-->

# Input
n nodes,m edges DAG
output numbers of SCC

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+5;
int n,m;
vector<int>g[N];
int low[N],vis[N];
int dfn;
stack<int>s;
int belong[N],scc;
int in[N];
void tarjan(int u){
	vis[u]=low[u]=++dfn;
	s.push(u);
	for(auto v:g[u]){
		if(!vis[v]){
			tarjan(v);
			low[u]=min(low[u],low[v]);
		}
		else if(!belong[v]){
			low[u]=min(low[u],vis[v]);
		}
	}
	if(low[u]==vis[u]){
		belong[u]=++scc;
		while(s.size()&&s.top()!=u){
			belong[s.top()]=scc;
			s.pop();
		}
		s.pop();//pop u
		
	}
}
int main(){
	cin>>n>>m;
	int u,v;
	for(int i=0;i<m;i++){
		cin>>u>>v;
		g[u].push_back(v);
	}
	dfn=scc=0;
	for(int i=1;i<=n;i++)vis[i]=in[i]=belong[i]=0;
	for(int i=1;i<=n;i++)
		if(!vis[i])
			tarjan(i);
		
	for(int i=1;i<=scc;i++){
		printf("scc %d\n",i);
		for(int j=1;j<=n;j++){
			if(belong[j]==i)
				printf("%d ",j);
		}
		puts("");
	}
	printf("Total: %d's scc\n",scc);
}


/*
5 5
2 1
1 3
3 2
1 4
4 5

*/
```