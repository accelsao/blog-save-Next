---
title: Codeforces Round 530 C - Construct a tree
tags:
  - binary search
  - greedy
  - binary search
  - constructive algorithms
  - dfs and similar
  - graphs
  - trees
categories:
  - Codeforces
mathjax: false
date: 2019-06-22 15:30:52
---

解題心得
<!--more-->

# Problem
給你n,S
代表n個點組成Tree 且每個點的subtree size的sum = S
還要盡量讓所有點中最大的子樹數量 越少越好

# Solution
一開始要決定他的子樹max size k是多少
k決定了最小 最大的當然是k=1 Tree轉成鍊
最小的話就是每個點都盡量分出K的子樹點 因為基本上每加上一個 depth=d的點 貢獻上就是d (d-1個parent會受惠+自己)
也就是說depth壓 sum越低
二分k以後 我們就先假設當前數是鍊狀 然後開始從最下面的點考慮要不要移到上面減少SUM
將depth b移動到depth a sum會減少(b-a) 所以只要保證當前depth的數量不要超過上限就好 上限為[1, k, k^2, k^3 ...]
然後再來就控制把個depth的vertex index l,r 依序輸出即可

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e5+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


ll n,s;
int L[N],R[N];
bool check(ll k){
	ll _k=1, lf=n;
	ll sum=0;
	for(int i=1;;i++){
		if(lf>=_k){
			sum+=_k*i;
		}
		else{
			sum+=lf*i;
//			cout<<sum<<endl;
			return (sum<=s);
		}
		lf -= _k;
		_k *= k;
//		cout<<i<<endl;	
//		printf("i=%d, sum=%lld\n", i,sum);
	}
}
int main(){
	cin>>n>>s;
	if(s>(n+1)*n/2 || s<2*n-1){
		cout<<"No\n";
		return 0;
	}
	
	if(s == (n+1)*n/2){
		cout<<"Yes\n";
		for(int i=1;i<n;i++)
			cout<<i<<" ";
		cout<<"\n";
		return 0;
	}
	
//	cout<<check(2)<<endl;
	
	int l=2,r=n, m, k;
	while(l<=r){
		m=l+r>>1;
		if(check(m)){
			r=m-1;
			k=m;
		}
		else{
			l=m+1;
		}
	}
//	cout<<k<<endl;
	//k
	cout<<"Yes\n";
	for(int i=1;i<=n;i++)
		L[i]=1;
	R[1]=1;
	for(int i=2;i<=n;i++){
		R[i]=R[i-1]*k;
	}
	ll tot=(n+1)*n/2;
	for(int i=n;tot>s;i--){
		int j=1;
		while(L[j]==R[j])j++;
		while(tot-s<i-j)j++;
		L[i]--;
		L[j]++;
		tot-=i-j;
	}
	int num=0;
	for(int i=2;i<=n;i++){
		l=num+1;
		num+=L[i-1];
		r=num;
		int p=l;
		for(int j=0;j<L[i];j++){
			cout<<p++<<" ";
			if(p>r)p=l;
		}
	}
	cout<<"\n";

}
```
