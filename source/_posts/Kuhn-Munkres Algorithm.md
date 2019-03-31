---
title: Kuhn-Munkres Algorithm
tags:
  - Kuhn-Munkres Algorithm
categories:
  - Algorithms
mathjax: true
date: 2018-12-13 08:56:09
---

Kuhn-Munkres算法 
二分圖最佳完美匹配
<!--more-->
# Problem
輸入n 接下來有n男n女要匹配 
n*n的矩陣代表 matrix[i][j] = i-j間好感度 
要求匹配的好感度和最大

in:
3
0 -7 10
0 1 9
10 0 -5
out:
Match {1:3} = 10
Match {2:2} = 1
Match {3:1} = 10
21


# \\(\mathcal{O}(n^4)\\)
```cpp
#include <bits/stdc++.h>
#define Accel ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
#define LL long long
using namespace std;
const int N=1e3+10;
int g[N][N];
int n,m;
int lx[N],ly[N];
int match[N];
int slack[N];
bool vis_x[N],vis_y[N];
const int inf=1e9+7;
bool dfs(int u){
	if(vis_x[u])return 0;
	vis_x[u]=1;
	for(int v=1;v<=n;v++){
		if(vis_y[v])continue;
		int tmp=lx[u]+ly[v]-g[u][v];
		if(tmp==0){
			vis_y[v]=1;
			if(match[v]==-1||dfs(match[v])){
				match[v]=u;
				return 1;
			}
		}
		else if(slack[v]>tmp){
			slack[v]=tmp;
		}
	}
	return 0;
}
int KM(){
	memset(match,-1,sizeof(match));
	//y初始期望0 
	memset(ly,0,sizeof(ly));
	//x初始期望是所有和她相連的點鐘最大的  
	for(int i=1;i<=n;i++){
		lx[i]=-inf;
		for(int j=1;j<=n;j++)
			lx[j]=max(lx[j],g[i][j]);
	}
	//嘗試 匹配
	for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++)slack[j]=inf;
		while(1){
			memset(vis_x,0,sizeof(vis_x));
			memset(vis_y,0,sizeof(vis_y));
			if(dfs(i))break;
			int cut=inf;
			for(int j=1;j<=n;j++)
				if(!vis_y[j]&&cut>slack[j])
					cut=slack[j];
			for(int j=1;j<=n;j++){
				if(vis_x[j])lx[j]-=cut;
				if(vis_y[j])ly[j]+=cut;
				else slack[j]-=cut;
			}
		}
	}
	int ans=0;
	for(int i=1;i<=n;i++){
		printf("Match {%d:%d} = %d\n",i,match[i],g[match[i]][i]);
		ans+=g[match[i]][i];
	}
	return ans;
}
int main(){Accel
	cin>>n;
	for(int i=1;i<=n;i++)	
		for(int j=1;j<=n;j++){
			cin>>g[i][j];
		}
	
	cout<<KM()<<endl;
}

```
# \\(\mathcal{O}(n^3)\\)

# 參考
[通俗解釋](https://www.cnblogs.com/wenruo/p/5264235.html)
[Kuhn-Munkres 算法详细解析](https://blog.sengxian.com/algorithms/km)
[日月卦長](http://sunmoon-template.blogspot.com/2016/05/kuhn-munkres-algorithm.html)