---
title: Educational Codeforces Round 62 E - Palindrome-less Arrays
tags:
  - combinatorics
  - dp
  - difficulty 2200
categories:
  - Codeforces
mathjax: true
date: 2019-06-05 17:26:35
---

解題心得
<!--more-->

# Problem

給你一個array 可以把其中的-1改成1~k的任意數字
最後array 滿足不包含長度大於1的回文array

題目定義的 回文array: len is odd a[i]=a[n-1-i], \\(i \leq n-1-i\\)

# Soltuion
題目重點:
我們只要處理長度為3的subarray就好 因為如果長度>3的 最後也需要由長度=3的來決定 good or bad
更進一步只需要檢查 a[i], a[i+2] (a[i+1]在中間 所以不重要)
所以我們把index 奇偶分開來看 重新組成array
對於新的array, 我們不能讓他們出現相鄰相同數字的可能性
然後可能出現的數組會長這樣 (a, -1, -1, -1, b) or (a, -1, -1, -1, a)
如果a, b在邊邊所以是empty的話 計算 k*(a, -1, -1, ..., b) -1的數量多扣一

先預處理以上可能的情況
cntSame[i] = a, -1, -1, -1 ... a (-1 有 i 個)
cntDiff[i] = a, -1, -1, -1 ... b (-1 有 i 個)

cnstSame[0]=0;
cnstDiffe[0]=1;

將長度分成奇偶討論
奇數:
$$
cntSame[i] = \text{(放a) }cntSame[i/2]^2 + \text{(非a) }cntDiff[i/2]^2 \times (k-1) \\
cntDiff[i] = \text{(放a) } cntSame[i/2] \times cntDiff[i/2] + \text{(放b) } cntDiff[i/2] \times cntSame[i/2] + \text{(非a非b) } cntDiff[i/2]^2 \times (k-2)
$$
偶數:
$$
cntSame[i] = \text{(非a)} cntDiff[i-1] \times (k-1) \\
cntDiff[i] = \text{(放a)} cntSame[i-1]  + \text{(非a非b)} cntDiff[i-1] \times (k-2)
$$

再來記得處理a, b是邊界的情況 以及 Mod的處理

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N=2e5+10;
const int inf=1e9+10;
const int M=998244353;

typedef long long ll;

int n;
ll k;
ll cntSame[N];
ll cntDiff[N];

vector<int> a;
vector<int> b;

ll get(int l,int r){
//	cout<<l<<" "<<r<<endl;
	int len = r-l-1;
//	cout<<b.size()<<endl;
	if(r>=b.size()){
		ll h = 1;
		if(l<0){
			len--;
			h = k;
		}
		if(len==0)return h;
		len--;
		h = h * ((cntSame[len] + cntDiff[len] * (k-1) % M) % M) % M;
		return h;
	}
	if(l<0){
		if(len==0)return 1;
		len--;
		ll h = (cntSame[len] + cntDiff[len] * (k-1) % M) % M;
		return h;
	}
	if(b[r] == b[l])
		return cntSame[len];
	else{
		return cntDiff[len];
	}
}

void solve(){
	ll ans=1;	
	for(int k=0;k<2;k++){
		b.clear();
		for(int i=0;i*2+k<n;i++){
			b.push_back(a[2*i+k]);
		}
		int ls=-1;
		for(int i=0;i<b.size();i++){
			if(b[i]==-1)continue;
			//cout<<ls<<" "<<i<<endl;
			ans = ans * get(ls, i) % M;
			ls = i;
		}
		ans = ans * get(ls, b.size()) % M;
//		cout<<ans<<endl;
	}
	cout<<ans<<endl;
}


int main(){
	cin>>n>>k;
	a.resize(n);
	for(int i=0;i<n;i++){
		cin>>a[i];
	}
	
	cntSame[0]=0;
	cntDiff[0]=1;
	for(int i=1;i<=n;i++){
		if(i%2){
			cntSame[i] = ((cntSame[i/2] * cntSame[i/2] % M) + ((k-1) * cntDiff[i/2] % M * cntDiff[i/2] % M)) % M;
			cntDiff[i] = (2 * cntSame[i/2] * cntDiff[i/2] % M) + ((k-2) * cntDiff[i/2] % M * cntDiff[i/2] % M) % M;
		}
		else{
			cntSame[i] = cntDiff[i-1] * (k-1) % M;
			cntDiff[i] = (cntSame[i-1] + cntDiff[i-1] * (k-2) % M) % M;
		}
	}
	
	solve();
	
}
```