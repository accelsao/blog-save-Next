---
title: Codeforces Round 549 (Div. 1) B - Lynyrd Skynyrd
tags:
  - RMQ
categories:
  - Codeforces
mathjax: false
date: 2019-06-03 15:04:08
---

解題心得
<!--more-->

# Problem
m個數組成p
給你n個數
q個詢問 l, r
問a_l, a_r 之間的數的subsequence 有沒有Permutaion(p)
# Solution

首先b[i]=smallest j, a[j]是a[i]的下一個permuation的數 因為每個數的next number是固定的
b[b[b[]]]... n-1次就可以得到組成permutaion的subsequence的最右邊的位址
問題轉變成區間求極值 用RMQ預處理
如果get(l, r) <= r 有解

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N=200000+10;
const int inf=1e9+10;

typedef long long ll;

int n,m,Q;
int ls[N];
int p[N], a[N];
int b[N][20];
int nxt[N];
int st[N][20];
int lg2[N];
int get(int l,int r){
	int k=lg2[r-l];
	return min(st[l][k], st[r-(1<<k)][k]);
}
int main(){
	cin>>n>>m>>Q;
	int x;
	for(int i=0;i<n;i++){
		cin>>x;
		p[x] = i;
	}
	for(int i=0;i<m;i++){
		cin>>x;
		a[i] = p[x];
	}
	
	
	for(int i=0;i<n;i++){
		nxt[i]=m;
	}
	for(int i=0;i<=m;i++)
		for(int k=0;k<20;k++)
			b[i][k]=m;
	
	for(int i=m-1;i>=0;i--){
		int x=a[i];
		int y=a[i]+1;
		if(y==n)y=0;
		//printf("%d %d %d\n", x, y, nxt[y]);
		b[i][0]=nxt[y];
		nxt[x]=i;
	}
		
//for(int i=0;i<m;i++)
//		cout<<b[i][0]<<endl;	
	for(int k=1;k<20;k++)
		for(int i=0;i<m;i++){
			b[i][k]=b[b[i][k-1]][k-1];
			//printf("i:%d, k:%d\n", i,k);
		}
	
//for(int i=0;i<m;i++)
//		cout<<b[i][0]<<endl;
	
	for(int i=0;i<m;i++){
		int x=i;
		for(int k=0;k<20;k++)
			if(((n-1)>>k)&1){
			//	printf("x:%d, k:%d\n", x,k);
				x=b[x][k];
			}
		st[i][0]=x;
	}
//	
//	for(int i=0;i<m;i++)
//		cout<<st[i][0]<<endl;
	
	for(int k=0;k<20;k++)
		for(int i=0;i<m;i++){
			if(i+(1<<(k+1))<=m)
				st[i][k+1]=min(st[i][k], st[i+(1<<k)][k]);
		}
	
	for(int i=2;i<=m;i++){
		lg2[i]=lg2[i/2]+1;
	}
	
	while(Q--){
		int l,r;
		cin>>l>>r;
		l--;
		if(get(l, r)<r){
			printf("1");
		}
		else{
			printf("0");
		}
	}
	cout<<"\n";
	
}
```