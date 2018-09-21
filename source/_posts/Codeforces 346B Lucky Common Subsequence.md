---
title: Codeforces 346B Lucky Common Subsequence
tags:
  - DP
  - Strings
  - Codeforces
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-20 20:41:26
---

# Probelm
給你三個字串s1,s2,virus
求s1,s2不包括virus字串的lcs

<!--more-->

# Input
* \\(1\leq |s1|,|s2|,|virus|\leq 100\\)

# Solution

定義next1[i][j]= s1[i]['A'+j]的下一個index
next2 s2 同理

定義next3[i][j]=lcs(string(virus[0,i-1]+'A'+j),virus)的長度
next3[i][j]=-1 if i==len(virus)and string(virus[0,i-1]+'A'+j)==virus

簡單dp
構造字串
end
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
const int N=2e5+1;
const int M=1e9+7;
const double eps=1e-6;
//size() use int
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){return x?BIT[x]+sum(x-(x&-x)):0;}
//int find(int x){return x==p[x]?x:p[x]=find(p[x]);}
//LL pw(LL a, LL b,LL M) { return b ? b & 1 ? a*pw(a, b - 1,M) % M : pw(a*a%M, b>>1,M) : 1; }
string a,b,c;
int next1[110][30];
int next2[110][30];
int next3[110][30];
int A,B,C;
int dp[110][110][110];
int search(string s){
	int len=SZ(s);
	for(int i=len;i>=1;i--){
		bool match=true;
		REP(j,i)if(s[len-i+j]!=c[j])match=false;
		if(match){
			if(i==C)return -1;
			return i;
		}
	
	}
	return 0;
}
int func(int a,int b,int c){
	int i;
	if(~dp[a][b][c])return dp[a][b][c];
	
	int ans=0,a2,b2,c2;
	REP(i,26){
		a2=next1[a][i];
		b2=next2[b][i];
		c2=next3[c][i];
		if(~a2&&~b2&&~c2)ans=max(ans,func(a2,b2,c2)+1);
	}
	return dp[a][b][c]=ans;
	
}
string p(int a,int b,int c){
	if(dp[a][b][c]==0)return "";
	REP(i,26){
        int a2 = next1[a][i];
        int b2 = next2[b][i];
        int c2 = next3[c][i];
        if(~a2&&~b2&&~c2)if(dp[a][b][c] == dp[a2][b2][c2] + 1)
			return (char)('A' + i) + p(a2, b2, c2);
    }
}
int main(){
	cin>>a>>b>>c;
	A=SZ(a),B=SZ(b);
	C=SZ(c);
	REP(i,26)next1[A][i]=-1;
	for(int i=A-1;i>=0;i--)
		REP(j,26)
			if(a[i]=='A'+j)next1[i][j]=i+1;
			else next1[i][j]=next1[i+1][j];
	REP(i,26)next2[B][i]=-1;
	for(int i=B-1;i>=0;i--)
		REP(j,26)
			if(b[i]=='A'+j)next2[i][j]=i+1;
			else next2[i][j]=next2[i+1][j];
	
	REP(i,C)REP(j,26)next3[i][j]=search(c.substr(0,i)+(char)('A'+j));
	for(int i=0;i<=A;i++)for(int j=0;j<=B;j++)for(int k=0;k<=C;k++)dp[i][j][k]=-1;
	
	int ans=func(0,0,0);
	if(ans==0){
		cout<<"0\n";
	}
	else{
		//cout<<"zzz\n";
		cout<<p(0,0,0)<<endl;
	}
}
```

