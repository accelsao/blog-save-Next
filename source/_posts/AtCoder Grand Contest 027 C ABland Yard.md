---
title: AtCoder Grand Contest 027 C - ABland Yard
tags:
  - Graphs
  - AtCoder
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-18 09:02:42
---

# Problem

給你一張圖(The given graph may NOT be simple or connected)
每個點代表'A' or 'B'
希望選一些點當作出發點
然後經過的點所代表的label 能造出任意AB字串
每個點可以經過任意次(包括0)

輸出Yes/No

<!--more-->

# Solution

因為要造出所有AB字串
對於任一點 必須要可以到AorB
這樣就可以造出任意字串了

每次把不合的點拔掉
不合的條件:孤點or只連接一種Type的點
最後圖如果empty代表不合


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

int n,m;
string a;
vector<int>g[N];
int num[N][2];
int del[N];
int main(){Accel
	cin>>n>>m>>a;
	int u,v;
	a='0'+a;
	REP(i,m){
		cin>>u>>v;
		g[u].PB(v);
		g[v].PB(u);
		num[u][a[v]=='B']++;
		num[v][a[u]=='B']++;
	}
	queue<int>q;
	FOR(i,1,n)
		if(!num[i][0]||!num[i][1])q.push(i),del[i]=1;
	while(SZ(q)){
		int u=q.front();q.pop();
		for(auto v:g[u])
			if(!del[v]&&!--num[v][a[u]=='B']){
				q.push(v);
				del[v]=1;
			}
	}
	FOR(i,1,n)
		if(!del[i]){
			cout<<"Yes\n";
			return 0;
		}
	cout<<"No\n";
			
}
```