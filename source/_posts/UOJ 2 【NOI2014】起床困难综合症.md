---
title: UOJ 2 【NOI2014】起床困难综合症
tags:
  - UOJ
  - Greedy
categories:
  - Competitive Programming
mathjax: true
date: 2018-10-02 17:18:28
---

# Problem

給你n,m
你要找一個數字\\(x\\),\\(0\leq x\leq m\\)
經過n個位元操作後的value最大
位元操作包含AND,XOR,OR

<!--more-->

# 解法

觀察這一連串的變化會發現 每個位元不互相影響
所以把這30個位元分別處理 因為value要求大 所以從高位元開始處理

我們考慮這個位置給0 or 1 看他的結果是否是1
構建a1=0,a2=\\(2^{31}\\)-1
然後n個詢問欲處理結果
最後從高位開始 如果這個位置放0的結果比放1的結果差 代表我們一定要在這個位置放1
同時維護我們要加總的值 保持\\(\leq m\\)

如果放0的結果跟放1一樣 我們就不用放1 減少\\(> m\\)的可能

# Code
```cpp
#include <bits/stdc++.h>
#define ld long double
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
#define endl "\n"
//#pragma GCC optimize(2)
using namespace std;
//typedef complex<double>C;
//const double PI(acos(-1.0));
//const double eps(1e-8);
//for(int mask=i;mask>0;mask=(mask-1)&i)
//int a[25]= { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97 };
//__gcd, atan2(y,x)=y/x , __int128
//c(n,k)=c(n-1,k-1)+c(n-1,k)
//c(i,r),i for r to n =c(n+1,r+1)
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){int s=0;while(x){s+=BIT[x];x-=x&-x;}return s;}
//int find(int x){return x==p[x]?x:p[x]=find(p[x]);}
//LL pw(LL a,LL b){LL t=1;for(;b;b>>=1,a=a*a%M)b&1?t=t*a%M:0;return t;}
//log() = ln() , log(x)/log(y)=log(y)-base-x
//INT_MAX 127,INT_MIN 128
//int dw[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
//int dw[8][2]={{0,1},{1,0},{0,-1},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
//cout<<fixed<<setprecision(12)<<ans<<endl;
//__builtin_popcount(mask)
const int N=1234567;
const int M=1e9+7;
int n,m;
int main(){//Accel
	cin>>n>>m;
	LL a1=0,a2=1LL<<31;a2--;
	REP(i,n){
		string s;
		cin>>s;
		int x;cin>>x;
		if(s=="AND")a1&=x,a2&=x;
		else if(s=="OR")a1|=x,a2|=x;
		else a1^=x,a2^=x;
	}
	int cur=0,ans=0;
	for(int i=1<<30;i;i>>=1){
		if((a1&i)<(a2&i)&&cur+i<=m){
			cur+=i;
			ans+=(a2&i);
		}
		else ans+=(a1&i);
	}
	cout<<ans<<endl;
	
}
```