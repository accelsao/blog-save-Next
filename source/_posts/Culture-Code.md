---
title: Educational Codeforces Round 69 E - Culture Code
tags:
  - binary search
  - combinatorics
  - data structures
  - dp
  - shortest paths
  - sortings
categories:
  - Codeforces
mathjax: false
date: 2019-07-28 09:22:54
---

解題心得
<!--more-->

# Notes
把俄羅斯娃娃轉換成區間問題
找出不重疊的區間數要求1.無法插入更多區間2.空隙盡量小
問最小空隙的選法數
有不同方法可以解決
1. 直接將區間左右端點分開排序
Seg={左or右, 位置, 編號}
如果是右點則計算當前空隙(當前**最左位置**-此區間長度)
如果是左點則更新當前**最左位置**
2. 直接處理區間左點排序or右點排序 二分搜最小的可行區間 然後DP更新
3. 直接用lower_bound處理

看似很複雜但原來很簡單 只是區間問題 對於區間i lower_bound找到符合的區間j f[i],f[j] dp處理而已

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
 
#define F first
#define S second
typedef long long ll;
const int N = 2e5 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;
 
int n;
pair<ll,int>f[N];
int main(){
 
 
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
 
	cin>>n;
	vector<pair<int,int>>a(n);
	for(int i=0;i<n;i++){
		cin>>a[i].S>>a[i].F;
	}
	sort(a.begin(),a.end());
	f[n]={inf, 0};
	for(int i=n-1;i>=0;i--){
		int j=lower_bound(a.begin()+i, a.end(), make_pair(a[i].S, -1))-a.begin();
		if(j==n)f[i]={a[i].F, 1};
		else{
			f[i]={f[j].F-(a[i].S-a[i].F), f[j].S};
		}
		if(f[i].F>f[i+1].F){
			f[i]=f[i+1];
		}
		else if(f[i].F==f[i+1].F){
			f[i].S+=f[i+1].S;
			f[i].S%=M;
		}
	}
	cout<<f[0].S<<endl;
}
```