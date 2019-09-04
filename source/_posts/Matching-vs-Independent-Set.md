---
title: Codeforces Round 576 C - Matching vs Independent Set
tags:
  - constructive algorithms
  - graphs
  - greedy
  - sortings
categories:
  - Codeforces
mathjax: false
date: 2019-08-17 11:09:47
---

解題心得
<!--more-->
# Solution
要求沒有共點的n個邊 or 沒有共邊的n個點
當形成Matching Edge的時候 剩下的點必是Independent Set 因為如果不是則可以再增加Matching Edge
所以當有n個Independent Vertices時有解 沒有的話有必有n個Matching Edges 3n-k>=2n (k<n)

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define F first
#define S second
typedef long long ll;
const int N = 3e5 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


vector<int>g[N];

int main(){


	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int T;
	cin>>T;
	while(T--){
		int n,m;
		cin>>n>>m;
		vector<bool>vis(3*n+1);
		vector<bool>edge(m);
		vector<pair<int,int>>E(m);
		for(int i=1;i<=n+n+n;i++)vis[i]=0;
		int u,v;
		for(int i=0;i<m;i++){
			cin>>E[i].F>>E[i].S;
			edge[i]=0;
		}
		int cnt=0;
		for(int i=0;i<m;i++){
			if(!vis[E[i].F]&&!vis[E[i].S]){
				vis[E[i].F]=vis[E[i].S]=1;
				edge[i]=1;
				cnt++;
			}
		}
		if(cnt>=n){
			printf("Matching\n");
			cnt=n;

			for(int i=0;i<m&&cnt>0;i++)
				if(edge[i]){
					printf(" %d",i+1);
					cnt--;
				}
			printf("\n");
		}
		else{
			printf("IndSet\n");
			cnt=n; 
			for(int i=1;i<=3*n&&cnt>0;i++)
				if(!vis[i]){
					printf(" %d",i);
					cnt--;
				}
			printf("\n");
		}
		
	}

	
}

```