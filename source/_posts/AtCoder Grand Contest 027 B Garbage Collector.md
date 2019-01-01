---
title: AtCoder Grand Contest 027 B Garbage Collector
tags:
  - Math 
  - Greedy
  - AtCoder
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-17 21:41:04
---


# Problem

一維線段上有N(\\(1\leq N\leq 2\times 10^{5}\\))個垃圾 位置保證\\(0<x_1<x_2<...<x_N\leq 10^{9}\\)
你初始在0 要蒐集所有垃圾 可以把任意數量的垃圾丟在原點
撿垃圾要X(\\(1\leq X\leq 10^{9}\\))能量,丟垃圾也要X能量(跟數量無關)
每次走距離1需消耗\\((k+1)^2\\)
求最小能量消耗

<!--more-->


# Solution

先假設一趟來回 我們要拿subset中所有的垃圾
那我們先到最遠的地方 在一起拿回來才會最優
因為如果在這之前我們先拿了垃圾 之後經過的路會給\\((k+1)^2\\)增加權重 跟回來時再拿比較消耗很多
所以我們直接走到最遠在沿路拿回到原點

現在討論一趟所需要的cost
最遠的需要(1+1)*(1+1)*x =4x
次遠的需要(2+1)*(2+1)*x =9x
以此類推
對於每次增加的垃圾袋
第一次要5x(包含到最遠的x)+5(9-4)x+7(16-9)x 以此類推
5,5,7,9,...,x,x+2

要讓答案最優 最遠的要配5x 次遠的7x...
所以我們窮舉需要走的趟次k
假設k=3 就會長這樣 ...9977555555
最遠的2k個配上5 次k遠的 7  以此類推

距離用perfix sum預處理
然後每次要再加上拿n個垃圾的能量\\(N\times X\\)和 丟K趟垃圾的能量\\(K\times X\\)
共為\\((K+N)\times X\\)

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
//#pragma GCC optimize(2)
using namespace std;
const int N=2e5+10;
const int M=1LL<<30;
const int inf=1e9+7;
const LL INF=1e18;
const double eps=1e-6;
typedef complex<double>C;
const double PI(acos(-1.0));
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
LL n,x;
LL a[N];
LL d[N];
int main(){Accel
	cin>>n>>x;
	FOR(i,1,n){
		cin>>a[i];
		d[i]=d[i-1]+a[i];
	}
	LL ans=9e18;
	for(int k=1;k<=n;k++){
		LL cur=3;
		LL z=0;
		for(int i=n;i>=1;i-=k){
			z+=(d[i]-d[max(i-k,0)])*max(5LL,cur);
			cur+=2;
			if(z>=ans){
				break;
			}
		}
		z+=(k+n)*x;
		ans=min(ans,z);
	}
	cout<<ans<<endl;
}

```