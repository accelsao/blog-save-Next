---
title: Codeforces 346D Robot Control
tags:
- Shortest Paths
- Codeforces
categories:
- Competitive Programming
mathjax: true
date: 2018-09-20 18:18:04
---

# Problem
給你有向有環圖和s,t 
當有兩條以上的選擇時機器人會隨機走
不能走"已經走過的點"
沒路走的時候就爆炸
可以選擇通知機器人往指定路走
問抵達t點的前提下需要通知的最少次數


<!--more-->

# Input 
* \\(1\leq N,M \leq 10^{6}\\),N個點,M條邊

# Solution
範例測資圖
考慮從終點回到起點 相反的建邊 從t回到s
![](https://i.imgur.com/MSpgJuh.png)

對於邊u->v來說
v點可能有多條邊連接其他點
可能只有一條

有很多條邊的時候就需要通知他來指令走的邊
因此d[v]=d[u]+1

如果只有一條邊的時候 就不需要指定他走的邊
d[v]=min(d[u],d[v])

```
維護這個最短路的方法可以用deque來實作
for each edge from u to v
    --outdegree[v]
    if(outdegree[v]==0){//只剩一條路
        d[v]=min(d[v],d[u])
        q.push_front(v)//將他從頭入queue保持queue裡的單調
        因為d[u]保證最小 d[v]更新後也保證<=d[u] 放入queue頭仍單調 
    }
    else{//不只一條路

        if(d[v] is undefined)
            d[v]=d[u]+1 
            q.push_back(v)
        如果d[v]有值 那不會出現更優的更新 因為先前的d[u]都是從最小開始取
        
        
    }
```
當你的v點是已經走過的點 vis[v]=true
再次經過此點不會讓d[v]更優 不必要走此邊
每次只要對於沒走過的點考慮他的值 是否需要+1即可
走到s點即可輸出答案


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
const int N=1e6+1;
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

int n,m,s,t;
vector<int>g[N];
int in[N];
int d[N];
int vis[N];
int main(){Accel
	
	cin>>n>>m;
	int u,v;
	REP(i,m){
		cin>>u>>v;
		g[v].PB(u);
		in[u]++;
	}
	cin>>s>>t;
	for(int i=0;i<=n;i++)d[i]=-1;
	d[t]=0;
	deque<int>q;q.push_front(t);
	while(SZ(q)){
		int u=q.front();q.pop_front();
		if(u==s)break;
		if(vis[u])continue;
		vis[u]=1;
		for(auto v:g[u]){
			if(!--in[v]){
				if(d[v]>d[u]||d[v]==-1){
					d[v]=d[u];
					q.push_front(v);
				}
			}
			else if(d[v]==-1){
				d[v]=d[u]+1;
				q.push_back(v);
			}
		}
		
	}
	cout<<d[s]<<endl;
	
}
```
