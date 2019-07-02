---
title: Vus the Cossack and a Graph
tags:
  - dfs and similar
  - graphs
  - greedy
  - implementation
categories:
  - Codeforces
mathjax: true
date: 2019-07-01 16:01:54
---

解題心得(未完)
TODO:
這個[Code](https://codeforces.com/contest/1186/submission/56222130) 好像更為簡潔
直接爆寫4hr 但還沒完全理解
問題1: 如何處理散落的component
用0和某一點建邊 但我不確定是否通用 雖然AC了
<!--more-->

# Notes
一開始不知道怎麼拔edge可以最優
於是乎 這題可以用Euler Tour的想法
先把所有odd degree的點連接到0 再把所有和0沒有連接的node也接到0
這樣一來所有點都可以從0開始遍歷 重點是**同時這張圖也是Euler Cycle**
因為是**Euler Cycle**代表我們可以從A點出發經過某些邊再回到A點並且所有經過的邊只會經過**恰好一次**
那我們又因為把所有點都和0連接 代表我們可以從0出發繞過某些點再回到0 且保證所有點都會走過 每條邊也會經過一次
走過的路徑會形成 `0ABC0EFD0ZYX0` 之類的樣子
為甚麼我們要做成這樣的edge呢
首先這張連接0點的圖最多有n+m的邊 (假設每個點都跟0建邊)
那我們只要取(n1+m)/2 就可以保證他符合條件 (n+m)/2 (n1為實際和0建邊數量)
那我們只要對路徑上index偶數的邊delete就剛好能留下ceil((n1+m)/2)
但問題來了
有些邊是虛邊(連接0) 這些邊本來就是**delete 邊**
不過我們有一個解法, 如果這條虛邊是需要**remain** 代表他的下一條邊是被**delete** 那我們交換就解決了
如果這條虛邊本來就要**delete**可忽略
又因為我們一定從0開始 因此我們只要把0前兩個點到0前一個點這條邊強制設為1就好了
0ABC0 -> XOXO
調整為 -> XOOO

本來還想不到怎麼traverse Euler Cycle 因為可能有點連通塊沒有和0連接
結果只要0和u, v 相連兩點連接即可
輸出結果會剛好形成 0-u連接兩個邊的情況

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
?
typedef long long ll;
const int N = 2e6+10;
const int M = 1e9 + 7;
const int inf = 1234567890;
const ll INF = 1e18;
?
int n,m;
vector<pair<int,int>> g[N];
int connect0[N];
vector<int> euler_tour;
int vis_edge[N];
void dfs(int u){
	while(g[u].size()){
		int v=g[u].back().first;
		int id=g[u].back().second;
		g[u].pop_back();
		if(vis_edge[id])continue;
		vis_edge[id]=1;
		dfs(v);
	}
	euler_tour.push_back(u);
}
void _union(int u){
	if(connect0[u])return ;
	connect0[u]=1;
	for(const auto& e:g[u]){
		_union(e.first)	;
	}
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m;
	int u,v;
	for(int i=0;i<m;i++){
		cin>>u>>v;
		g[u].push_back({v, i});
		g[v].push_back({u, i});
	}
	for(int i=1;i<=n;i++){
		if(g[i].size()%2){
//			printf("node=%d\n",i);
			g[i].push_back({0, i+m});
			g[0].push_back({i, i+m});
			_union(i);
		}
	}
	for(int i=1;i<=n;i++){
		if(!connect0[i]){
			if(g[i].size()==0)continue;
//			cout<<i<<endl;
			int u=i, v=g[i].back().first;
//			printf("u=%d,v=%d\n", u, v);
			g[u].push_back({0, u+m});
			g[0].push_back({u, u+m});
			g[v].push_back({0, v+m});
			g[0].push_back({v, v+m});
			_union(u);
		}
	}
//	for(int i=0;i<=n;i++)
//		for(const auto& e:g[i])
//		printf("%d=%d \n",i, e.first);
	
	dfs(0);
//	for(const auto& x:euler_tour){
//		printf("%d ",x);
//	}
//	cout<<endl;
	vector<pair<int,int>>remain_edge;
	vector<int> remain(euler_tour.size());
	int p=0;//remain
	for(int i=0;i<euler_tour.size();i++){
		if(euler_tour[i]==0){
			p=0;
			if(i>=2){
				remain[i-2]=1;
			}
		}
		else{
			p^=1;
			if(euler_tour[i+1])
				remain[i]=p;
			
		}
	}
	for(int i=0;i<euler_tour.size();i++){
		if(remain[i]){
			remain_edge.push_back({euler_tour[i], euler_tour[i+1]});	
		}
	}
	cout<<remain_edge.size()<<endl;
	for(const auto& e:remain_edge){
		printf("%d %d\n",e.first,e.second);
	}
	
}
```

