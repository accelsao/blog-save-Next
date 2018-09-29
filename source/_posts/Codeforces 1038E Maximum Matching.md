---
title: Codeforces 1038E Maximum Matching
tags:
  - Graphs
  - DP
  - Codeforces
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-25 17:10:42
---

待補

<!--more-->


# Solution I
N=100 Color=4
Floyd-Warshall O(\\(n^2\\))

f[i][j][u][v]=第i和j邊之中u,v開頭尾的路
初始:f[i][i][u][v]=f[i][i][v][u]=val 沒啥特別的
更新:
若只要其中一段的話
f[i][j][u][v]=max(f[i][j][u][v],max(f[i][k][u][v],f[k+1][j][u][v]))
若i~k取 u,h段
f[i][j][u][v]=max(f[i][j][u][v],f[i][k][u][h]+f[k+1][j][h][v])
若k+1~j取 u,h段 //u,h h,u都可
f[i][j][u][v]=max(f[i][j][u][v],f[i][k][v][h]+f[k+1][j][h][u]);


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
const int N=105;
const int M=1e9+7;
const double eps=1e-6;
//size() use int
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){return x?BIT[x]+sum(x-(x&-x)):0;}
//int find(int x){return x==p[x]?x:p[x]=find(p[x]);}
//LL pw(LL a, LL b,LL M) { return b ? b & 1 ? a*pw(a, b - 1,M) % M : pw(a*a%M, b>>1,M) : 1; }

int n,u,v,val;
int f[N][N][5][5];

int main(){
	cin>>n;
	Re(f,-0x3f);
	int ans=0;
	FOR(i,1,n){
		cin>>u>>val>>v;
		f[i][i][u][v]=f[i][i][v][u]=val;
		ans=max(ans,val);
	}
	for(int i=n-1;i>=1;i--)
		for(int j=i+1;j<=n;j++)
			FOR(u,1,4)
				FOR(v,1,4){
					for(int k=i;k<j;k++){
						f[i][j][u][v]=max(f[i][j][u][v],max(f[i][k][u][v],f[k+1][j][u][v]));
						FOR(h,1,4){
							f[i][j][u][v]=max(f[i][j][u][v],f[i][k][u][h]+f[k+1][j][h][v]);
							f[i][j][u][v]=max(f[i][j][u][v],f[i][k][v][h]+f[k+1][j][h][u]);
						}
						
					}
					ans=max(ans,f[i][j][u][v]);
				}
		
	cout<<ans<<endl;
}
```