---
title: Codeforces Round 560 F2 - Microtransactions
tags:
  - binary search
  - greedy
categories:
  - Codeforces
mathjax: false
date: 2019-06-18 17:19:00
---

解題心得
<!--more-->

# Problem
給你要買的商品種類和個數
還有一些促銷資訊 特定某天特定商品半價
正常都2元 促銷1元
你每天得到一元

# Solution
答案範圍[1, 2*sum] sum是所有商品個數總和
二分搜 然後先考慮購買促銷
第k天如果A促銷 就盡量買完A 錢夠的話
注意第K天最多只有K元 所以反過來跑的時候 如果都沒促銷(沒購買)要調整價錢到K
跑完所有天數以後 剩下的商品就是需要2元買的
所以剩下數量*2 如果不夠當前 False 需要更多天
反之 看能不能更少天

# Notes
沒想到可以把錢留到最後促銷那天在買商品 因為一天可以買**無限個無限種**
這條件很重要 不然怎麼想都很困難... 

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5+10;
const int M = 998244353;
typedef long long ll;



int n,m;
int a[N];
vector<int> disc[N<<1];
int sum=0;
// log(sum) * (m+n+sum)

bool check(int k){
	vector<int> b(n+1);
	
	int buy=0,cur = k;
	for(int i=k;i>=1;i--){
		cur=min(cur,i);
		for(const auto& j:disc[i]){
			int h = min(a[j]-b[j], cur);
			cur-=h;
			b[j]+=h;
			buy+=h;
		}
	}
	return ((sum-buy)<<1) <= k-buy;
}

int main(){
	ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
	cin>>n>>m;
	sum=0;
	for(int i=1;i<=n;i++){
		cin>>a[i];
		sum+=a[i];
	}
	
	int d,t;
	while(m--){
		cin>>d>>t;
		disc[d].push_back(t);
	}
	
	int l=1,r=sum<<1,m,ans=-1;
	while(l<=r){
		m=l+r>>1;
		if(check(m)){
			r=m-1;
			ans = m;
		}
		else{
			l=m+1;
		}
	}
	cout<<ans<<endl;	
}
```
