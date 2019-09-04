---
title: Helvetic Coding Contest 2019 online mirror C2 - Heidi and the Turing Test (Medium)
tags:
  - data structures
categories:
  - Codeforces
mathjax: false
date: 2019-08-17 15:07:09
---

解題心得
<!--more-->

# Solution 
問題轉換
1. (x,y)->(x+y,x-y)
問題就會變成2r的正方形能框住最多點的數量
然後再考慮每個點轉換成正方形
就變成給你一堆正方形 找出某一點被最多正方形框住的數量
又轉換成2D區間計數 所以用Segmentree實作即可
忘了怎麼用lazyupdate...

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
 
#define F first
#define S second
typedef long long ll;
const int N = 2e6 +10;
const int inf = 1e9+7;
const ll INF = 1e18;
 
int mx[N];
int lz[N];
void lz_upd(int u){
	if(lz[u]) {
		mx[u<<1]+=lz[u] ;
		mx[u<<1|1]+=lz[u];
		lz[u<<1]+=lz[u];
		lz[u<<1|1]+=lz[u];
		lz[u]=0;
	} 
}
void add(int u,int l,int r,int L,int R,int v){
	if(l>R||r<L)return ;	
	if(l>=L&&r<=R){
		lz[u]+=v;
		mx[u]+=v;
		return ;
	}
	lz_upd(u);
	int m=(l+r)>>1;
	add(u<<1,l,m,L,R,v) ;
	add(u<<1|1,m+1,r,L,R,v);

	mx[u]=max(mx[u<<1],mx[u<<1|1]);
}

 
int main(){
 
 
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	
 
	
	int n, R;
	cin>>n>>R;
	vector<pair<int,int>> pts(n); 
	vector<int>X;
	for(int i=0;i<n;i++){
		cin>>pts[i].F>>pts[i].S; 
		int x=pts[i].F+pts[i].S;
		int y=pts[i].F-pts[i].S;
		X.push_back(x-R);
		X.push_back(x+R+1);
	}
	sort(X.begin(),X.end());
	X.resize(unique(X.begin(),X.end())-X.begin());
	
	
	map<int,vector<pair<pair<int,int>,int>>> mp;
	
	for(int i=0;i<n;i++) {
		int x=pts[i].F+pts[i].S;
		int y=pts[i].F-pts[i].S;
		int l=lower_bound(X.begin(),X.end(),x-R)-X.begin()+1;
		int r=lower_bound(X.begin(),X.end(),x+R+1)-X.begin();
		mp[y-R].push_back({{l,r},1}) ; 
		mp[y+R+1].push_back({{l,r},-1}) ; 
	} 
	 
	int ans=0;
	for(auto x:mp){
		for(auto y:x.S){
			add(1, 1, X.size(), y.F.F, y.F.S, y.S);
		}
		ans=max(ans,mx[1]);
	}
	cout<<ans<<endl;
	
}
```
