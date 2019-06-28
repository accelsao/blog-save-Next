---
title: Codeforces Round 552 G - Minimum Possible LCM
tags:
  - brute force
  - greedy
  - math
  - number theory
categories:
  - Codeforces
mathjax: true
date: 2019-06-27 16:25:35
---

解題心得
<!--more-->

# Problem
給你array a, 求pair(i,j) \\(lcm(a[i],a[j])\\)最小
n 是數字數量
a 是 \\(10^7\\) 數字的大小範圍

# Solution
第一個解法
首先用O(n)求出質數 並同時可求出SPF
把每個數加到他的所有因數裡面
由於從小到大處理 所以只要用pair維護這個最小的數就行
窮舉所有公因數 兩個相乘/公因數 直接紀錄最小的(i,j) 
O(n)求SPF, recursive求出所有質數 估計複雜度就是因數數量 然後這個數的上限是O(n^{1/3})
所以複雜度為\\(O(na^{1/3})\\)

第二個解法 窮舉因數 找到兩個最小的 一樣直接計算
每個因數只要計算1/n
複雜度為a(1+1/2+1/3...1/a) 約等於 \\(alog(a)\\)
# Code

\\(O(a+nlogn+na^{1/3})\\)
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e7+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;



int isprime[N];
int spf[N]; //shortest prime factor
vector<pair<int,int>>a;
vector<ll> prime;
vector<pair<int,int>>ans;// lhs<rhs
vector<pair<int,int>> divs;
int b[N];
void get(int i, int num, int idx){
	if(i==divs.size()){
		if(ans[num].first==-1){
			ans[num].first=idx;
		}
		else if(ans[num].second==-1){
			ans[num].second=idx;
		}
		return ;
	}
	int cur=1;
	for(int j=0;j<=divs[i].second;j++){
		get(i+1, cur*num ,idx);
		cur *= divs[i].first;
	}
}
void add(int x, int idx){	
	divs.clear();
	int cur=x;
	while(cur>1){
		int d=spf[cur];
		if(divs.size()&&divs.back().first==d){
			divs.back().second++;
		}
		else{
			divs.push_back(make_pair(d, 1));
		}
		cur /= d;
	}
//	printf("%d: \n", x);
//	for(const auto& j:divs){
//		printf("(%d %d), ", j.first, j.second);
////		cout<<j.first<<" ";
//	}
//	printf("\n");
//	cout<<"\n";
	get(0, 1, idx);
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	
	ans.resize(N, make_pair(-1,-1));
	
	int n, x;
	for(int i=2;i<N;i++){
		if(!isprime[i]){
			prime.push_back(i);
			spf[i]=i;
		}
		for(const auto& j:prime){
			if(j>spf[i])break;
			ll num=j*i;
			if(num >= N)break;
			isprime[num]=1;
			spf[num]=j;
//			z[num]++;
//			printf("%d: %lld\n", i, num);
		}
	}
	cin>>n;
	a.resize(n);
	for(int i=0;i<n;i++){
		cin>>a[i].first;
		a[i].second=i+1;
		b[i+1]=a[i].first;
	}
	sort(a.begin(),a.end());
	for(int i=0;i<n;i++){
//		cout<<a[i].first<<endl;
		if(i>1 && a[i].first==a[i-2].first)continue;
//		;
		add(a[i].first, a[i].second);
	}
	
//	for(int i=1;i<=10;i++){
//		cout<<ans[i].first<<" "<<ans[i].second<<endl;
//	}
	
	ll res=INF;
	int lx,rx;
	
	
	
	for(int i=1;i<N;i++){
		if(ans[i].second!=-1){
			ll lhs=b[ans[i].first] / i;
			ll rhs=lhs * b[ans[i].second];
			if(res>rhs){
//				cout<<i<<endl;
				res=rhs;
				lx=ans[i].first;
				rx=ans[i].second;
			}
//			res=min(res,rhs);
		}
	}
	printf("%d %d\n", min(lx,rx), max(rx, lx));
//	cout<<res<<"\n";
}
```

O(aloga)
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e7+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;


int a[N];
int n;
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	int x;
	ll ans=INF;
	int lx,rx;
	for(int i=1;i<=n;i++){
		cin>>x;
		if(a[x] && ans>x){
			ans=x;
			lx=a[x];
			rx=i;
		}
		a[x]=i;
	}

	
	for(int i=1;i<N;i++){
		if(i>=ans)break;
		int l=-1,r;
		for(int j=i;j<N;j+=i){
			if(!a[j])continue;
			if(l==-1){
				l=j;
			}
			else{
				r=j;
				ll tmp=l/i;
				tmp*=r;
				
				if(tmp<ans){
					ans=tmp;
//					cout<<tmp<<endl;
//					cout<<l<<" "<< r<<endl;
//					cout<<a[l]<<" "<< a[r]<<endl;
					lx=a[l];
					rx=a[r];
				}
				
			}
				
		}
	}
	printf("%d %d\n",min(lx,rx),max(lx,rx));
}


```