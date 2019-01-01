---
title: Codeforces 1047C Enlarge GCD
date: 2018-09-17 09:54:04
tags:
- Prime number
- Codeforces
- Number theory
categories:
- Competitive Programming
mathjax: true
---

# Problem

給你一堆數字 設當前gcd=x
要求拿掉越少數字越好 剩下的數字的gcd>x
辦不到輸出-1
<!--more-->

# Solution 
N=1.5*1e7 所以預求質數到4000 4000^2>N
讓>4000的數字記錄到map裡

否則如果窮舉for(int i=2;i*i<=a[i];i++)就會慘吞TLE
因為若a[i]=1e7 i會經過4000左右 4000*n(3*1e5) 1e9左右會TLE
跑所有與處理的質數的話 約550個數 550*n 1.65 1e8 少10倍左右



# Code
```cpp
#include <bits/stdc++.h>
#define LL long long
#define Accel ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
#define Re(a,b) memset(a,b,sizeof a)
#define F first
#define S second
#define MP make_pair
#define PB push_back
#define ALL(X) (X).begin(), (X).end()
#define FOR(I, A, B) for (int I = (A); I <= (B); I++)
#define REP(I, N) for (int I = 0; I < (N); I++)
#define PII pair<int,int>
#define PLL pair<LL,LL>
#define SZ(a) ((int)a.size())
using namespace std;
const int M=998244353;
const int inf=1e9+7;
const LL INF=1e18;
const int N=15000010;


int a[N];
int cnt[N];
bool f[N];
vector<int>pr;
int main(){Accel
	FOR(i,2,4000){
		if(!f[i]){
			f[i]=1;
			pr.PB(i);
			for(int j=i+i;j<N;j+=i)
				f[j]=1;
		}
	}
	
	int n;
	cin>>n;
	int d=0;
	FOR(i,1,n){
		cin>>a[i];
		d=__gcd(d,a[i]);
		
	}

	FOR(i,1,n){
		a[i]/=d;
	}
	int ans=0;
	map<int,int>mp;
	for(auto j:pr){
		int cnt=0;
		FOR(i,1,n)
			if(a[i]%j==0){
				cnt++;
				while(a[i]%j==0)a[i]/=j;
				
			}
		ans=max(ans,cnt);
	}
	FOR(i,1,n){
		if(a[i]!=1)mp[a[i]]++;
	}
	for(auto x:mp)ans=max(ans,x.S);
	if(ans==0)
		cout<<-1<<endl;
	else cout<<n-ans<<endl;
	
	
	
	
}

```