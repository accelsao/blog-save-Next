---
title: AtCoder Grand Contest 027 D - Modulo Matrix
tags:
  - Math 
  - Construct
  - AtCoder
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-18 11:11:11
---

# Problem
要求建構\\(N\times N\\)矩陣滿足以下條件
* \\(1\leq a_{i,j}\leq 10^{15}\\)
* \\(a_{i,j}\\) pairwise distinct intergers
* There exists a positive integer m such that the following holds: Let x and y be two elements of the matrix that are vertically or horizontally adjacent. Then, max(x,y) mod min(x,y) is always m
<!--more-->

# Soltion

![](https://i.imgur.com/eGkj0Yy.png)

以棋盤為例 先把黑色的cell處理掉 白色部分就是相鄰所有cell的lcm+1
如此m固定為1
因為我們要造出相異的val
我們可以用質數來處理
兩個prime相乘 這樣保證不會有出現同數字的可能
要出現同數字a=b,則要a%b==0(a>=b) a是b的倍數

我們還希望值盡量小以免超過上限
因此可以先將前n個質數按照右上-左下方向向右下放2,3,5...
在按照左上右下從n*2個開始放

基本上值不會超過上限
最多一格子也就4個質數相乘
這樣交錯放的時候質數第N+1大會在右下,第1大會在左下 這只是其中一個辦法 讓他們盡量大配上小的

剩下的取周圍lcm+1即可


**以上方法n=2的時候會有相同val 因此須特別處理**


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
const int N=550;
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
int p[8000];//prime
LL g[N][N];
int dw[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
LL gcd(LL a,LL b){
	return b?gcd(b,a%b):a;
}
LL lcm(LL a,LL b){
	if(!a)return b;
	if(!b)return a;
	LL g=gcd(a,b);
	return a/g*b;
}
int main(){Accel
	cin>>n;m=n;
	if(n==2){
		cout<<"4 7\n23 10\n";
		return 0;	
	}
	vector<LL>P;P.PB(0);
	for(int i=2;i<=7919;i++)
		if(!p[i]){
			P.PB(i);
			for(int j=i+i;j<=7919;j+=i)p[j]=1;
		}
	//for(auto x:P)
		//cout<<x<<" ";
	FOR(i,1,n)FOR(j,1,m)g[i][j]=1;
	
	FOR(i,1,n)FOR(j,1,m){
		if(i%2==j%2){
			g[i][j]*=P[(i+j)/2];
			
			g[i][j]*=P[(i-j+n)/2+n+1];
		}
	}
	//FOR(i,1,n){FOR(j,1,m)cout<<g[i][j]<<" ";cout<<endl;}
	FOR(i,1,n)FOR(j,1,m)if(i%2!=j%2){
		g[i][j]+=lcm(lcm(g[i-1][j],g[i][j-1]),lcm(g[i+1][j],g[i][j+1]));
	}
	
	FOR(i,1,n){FOR(j,1,m)cout<<g[i][j]<<" ";cout<<endl;}
}
```