---
title: CS Academy - Cut the Trees
tags:
  - data structures
categories:
  - CS Academy
mathjax: false
date: 2019-07-03 13:28:08
---

解題心得
難度估計2300up 未來可再回來寫一次
<!--more-->

# Notes
題目需要找到任意K天變化中 不超過D的樹的最大可能數量
首先我們每天砍一棵樹, 那基本不可能砍同一棵樹2次 因為與其砍兩次不如第一次砍別顆放任樹長高第二次再砍這個,這樣也不會更差
所以重點在於砍樹的順序 爾且K在1~N天的範圍

我們需要維護到目前為止 超過D被砍的數量(bad) 以及 被砍後還沒超過D的數量(good)
ans = max(ans, n-bad+min(good, i+1)) 因為到第i天為止最多砍i+1棵樹 (活動都從第0天開始)
bad很簡單 但good需要考慮到每棵樹d/grow_rate
因為每天砍至多一棵 可以用Set一開始insert 0~n-1,
之後每次可以取0~k天 並將他從set移除表示砍了樹且並未超過D
set 如果選擇了0,2,3,5 這時候good是4
數字代表離當天開始算j天前我砍了這棵樹 所以你如果最多活k天 那你從set裡面只能抓0~k 你抓k+1天的話 過了k+1天樹又會超過D所以沒用
就相當於a[0]=k(5), a[2]=k(3), a[3]=k(2), a[5]=k(0) 

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

#define F first
#define S second
typedef long long ll;
const int N = 2e5+10;
const int M = 1e9 + 7;
const int inf = 1234567890;
const ll INF = 1e18;

int n, d;
int h[N];
int bad, good;
vector<int> g[N];
set<int> s;
void cut(int k){
	auto it=s.upper_bound(k);
	bad++;
	if(it!=s.begin()){
		it--;
		s.erase(it);
		good++;
	}
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	bad=good=0;
	cin>>n>>d;
	for(int i=0;i<n;i++)s.insert(i);s.insert(inf);
	
	for(int i=1;i<=n;i++){
		cin>>h[i];	
	}
	int k;
	for(int i=1;i<=n;i++){
		cin>>k;
		if(h[i]>d){
			g[0].push_back(d/k);
		}
		else{
			int t = (d-h[i])/k+1;
			if(t<n){
				g[t].push_back(d/k);
			}
		}
	}
	int ans=0;
	for(int i=0;i<n;i++){
		for(const auto& k:g[i])cut(k);
		ans=max(ans, n-bad+min(i+1, good));
	}
	cout<<ans<<endl;
}
```
