---
title: Codeforces 1051D Bicolorings
tags:
  - DP	
  - Codeforces
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-21 08:20:01
---

# Problem
給你\\(2\times N\\)的grid
要求你上色 黑白兩色
然後他們的group剛好是K
只要共邊就視同同個group

<!--more-->

# Solution
從1~n dp

定義dp[i][j][k]=第 i column,有j個group ,i column 兩色相同 if k=0 else 兩色不同
初始
dp[1][1][0]=2; 
dp[1][2][1]=2;

轉移 
$$dp[i][j][0]=dp[i-1][j][1]\times 2+dp[i-1][j][0]+dp[i-1][j-1][0]$$
$$dp[i][j][1]=dp[i-1][j-1][0]\times 2+dp[i-1][j][1]+dp[i-1][j-2][1]$$

其實蠻easy的
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
const int M=998244353;
LL dp[1500][1500*2][2];
int main(){Accel
	int n,k;
	cin>>n>>k;
	dp[1][1][0]=2;//0 最右邊 兩個相等
	dp[1][2][1]=2;//1 不相等
	for(int i=2;i<=n;i++) 
		for(int j=1;j<=2*i;j++){
			dp[i][j][0]=dp[i-1][j][1]*2+dp[i-1][j][0]+dp[i-1][j-1][0];
			dp[i][j][0]%=M;
			dp[i][j][1]=dp[i-1][j-1][0]*2+dp[i-1][j][1];
			if(j-2>=1)dp[i][j][1]+=dp[i-1][j-2][1];
			dp[i][j][1]%=M;
			
		}
	LL ans=dp[n][k][0]+dp[n][k][1];
	ans%=M;
	cout<<ans<<endl;
}

```