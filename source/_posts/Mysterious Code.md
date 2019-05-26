---
title: Codeforces Round 558 (Div. 2) D - Mysterious Code
tags:
  - Difficulty 2200
  - DP
  - Strings
categories:
  - Codeforces
mathjax: true
date: 2019-05-25 22:02:06
---

解題心得(未完)
<!--more-->

# Code
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6+10;
const int inf = 2147483646;


char s[1050], a[60], b[60];
int n, m, h;
int dp[1050][60][60];
int nxt[2][60][26]; // i,j,k ??{0~j j+1= k ??fail
int kmp[60];
void calc(char* s, int p){
	
	int n = strlen(s+1);
	kmp[1]=0;
	for(int i=2;i<=n;i++){
		int cur=kmp[i-1];
		while(cur>0&&s[cur+1]!=s[i])
			cur = kmp[cur];
		if(s[cur+1]==s[i])
			cur++;
		kmp[i]=cur;
	}
	for(int i=0; i<=n; i++){
		for(char c='a';c<='z';c++){
			int cur = i;
			while(cur>0&&s[cur+1]!=c){
				cur=kmp[cur];
			}
			if(s[cur+1]==c)
				cur++;

			nxt[p][i][c-'a']=cur;
		}
	}
}
int main(){
	scanf("%s%s%s", s+1, a+1, b+1);
	
	h = strlen(s+1);
	n = strlen(a+1);
	m = strlen(b+1);
	for(int i=0; i<=h; i++)
		for(int j=0; j<=n; j++)
			for(int k=0; k<=m; k++)
				dp[i][j][k] = -inf;
	dp[0][0][0] = 0;
	calc(a, 0);
	calc(b, 1);
	for(int i=0;i<h;i++)
		for(int j=0;j<=n;j++)
			for(int k=0;k<=m;k++)
				for(char c='a';c<='z';c++){
					if(s[i+1]=='*'||s[i+1]==c){
						int na=nxt[0][j][c-'a'];
						int nb=nxt[1][k][c-'a'];
						int z=dp[i][j][k];
						if(na==n)z++;
						if(nb==m)z--;
						dp[i+1][na][nb] = max(dp[i+1][na][nb], z);
					}
				}
	int ans =-inf;
	for(int i=0;i<=n;i++)
		for(int j=0;j<=m;j++)
			ans=max(ans,dp[h][i][j]);
	cout << ans<<endl;
}

```
# Notes
卡了很久的bug 因為 `scanf("%s", s+1)`, 但`int n = strlen(s)` ...