---
title: Educational Codeforces Round 67 D - Subarray Sorting
tags:
  - binary search
  - data structures
  - sortings
  - two pointers
categories:
  - Codeforces
mathjax: true
date: 2019-07-02 16:04:04
---

解題心得
<!--more-->

# Notes
題目要求a->b 可選任意a[l,r] sorting
那我們需要探討甚麼情況下會FAIL
1. 當sort(a)!=sort(b) 元素沒有map的時候
2. a[i]<a[j]但a[j]需要移動到a[i]的左邊 因為我們移動元素是靠sorting 因此大的swap到小的左邊是不可能的

一開始先建立a到b的位置 同個數字的相對位置不變
然後用BIT維護 1~a[i] 所移動到b的最大位置 (因為我們需要所有比a[i]小的數 移動到b的位置k 我們要保證k在a[i]到b的左邊 避免狀況2發生)

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
?
#define F first
#define S second
typedef long long ll;
const int N = 3e5+10;
const int M = 1e9 + 7;
const int inf = 1234567890;
const ll INF = 1e18;
?
int n,m;
int t;
int a[N];
int b[N];
vector<int>index[N];
int _idx[N];
int f[N];
int get(int x){
	int idx=0;
	while(x){
		idx=max(idx, f[x]);
		x-=x&-x;
	}
	return idx;
}
int _max(int x,int k){
	while(x<=n){
		f[x]=max(f[x], k);
		x+=x&-x;
	}
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n;
		bool flag=0;
		for(int i=1;i<=n;i++){
			cin>>a[i];	
			index[i].clear();
			f[i]=0;
		}
		for(int i=1;i<=n;i++){
			cin>>b[i];
			index[b[i]].push_back(i);
		}
		for(int i=n;i>=1;i--){
			if(index[a[i]].size()==0){
				flag=1;
				break;
			}
			int idx=index[a[i]].back();
			index[a[i]].pop_back();
			_idx[i]=idx;
		}
		for(int i=1;i<=n;i++){
			if(get(a[i]-1)>_idx[i]){
				flag=1;
				break;
			}
			_max(a[i], _idx[i]);//update a[i]~n to max{_idx[i]}
		}
		puts(flag?"NO":"YES");
		
	}
	
}
```
