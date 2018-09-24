---
title: Codeforces 1051E Vasya and Big Integers
tags:
- Z Algorithm
- DP
- Strings
- Codeforces
categories:
- Competitive Programming
mathjax: true
date: 2018-09-21 23:52:19
---

# Problem

給你大數a,l,r
要求你把a分成多段string 所有分段的string沒有前導0 且轉成數字的大小在l,r之間(包含)
ans mod 998244353
<!--more-->


# Input
* \\(1\leq a\leq 10^{10^6}\\)
* \\(0\leq l\leq r\leq 10^{10^6}\\)
* 輸入 a,l,r保證無leading zero

# Solution

我們要知道這個數字是否在L,R之間
首先len要<R且>L 或者 一樣位數時 逐位比較符合大小關係
比較的時候只在乎第一個不一樣的大小關係 所以可以維護公共前綴預處理後 只要比較一位即可
公共前綴用Z算法實作

z算法定義 z[i]指 s[i]開始的字串和s[0]開始的字串匹配的長度
$$s[0:z[i]]=s[i:i+z[i]]$$

z[0]就是整個string的長度 不需計算

當i<=r時
if 因為s[L,R]是string前綴 所以如果z[i-l]<r-i+1 那\\(s[i-l:i-l+z[k]]=s[i,R]的前綴\\)

```cpp
# Z_Algorithm
for(int i=1,l=0,r=0;i<n;i++){
		if(i<=r)z[i]=min(z[i-l],r-i+1);
		while(i+z[i]<n&&s[i+z[i]]==s[z[i]])z[i]++;
		if(i+z[i]-1>r){ //需要更新L,R
			l=i,r=i+z[i]-1;
		}	
	}
	return z;
```

然後DP處理檢查i位數
如果前綴>R則需要減少一位數
如果前綴<L則需要增加一位數

然後ans=dp(0)-dp(1)
dp(i)=包含i~n位開頭的方法數


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
const int M=998244353;
const int inf=1e9+7;
const LL INF=1e18;
const int N=1e6+10;
string s,l,r;
LL f[N];
int L,R,n;
int inc[N],decr[N];//increase decrease
vector<int>z_f(const string &s){
	int n=SZ(s);
	vector<int>z(n);
	for(int i=1,l=0,r=0;i<n;i++){
		if(i<=r)z[i]=min(z[i-l],r-i+1);
		while(i+z[i]<n&&s[i+z[i]]==s[z[i]])z[i]++;
		if(i+z[i]-1>r){
			l=i,r=i+z[i]-1;
		}	
	}
	return z;
}
LL dp(int idx){
	if(idx>n)return 0;
	if(idx==n)return 1;
	LL &ans=f[idx];
	if(~ans)return ans;
	ans=0;
	if(s[idx]=='0'){
		ans=dp(idx+1);
		if(l=="0"){
			ans+=dp(idx+1)-dp(idx+2);;//0單獨一個時 之後的必從idx+1為頭 	
		}
		ans+=M;
		ans%=M;
		return ans;		
	}
	
	int lidx=idx+L+inc[idx]; //考慮長度>=L的串 長度==L但小於L 就需要再多加一位數 inc 預處理
	int ridx=min(n,idx+R-decr[idx]); //考慮長度<=R的串 同理
	if(lidx<=n){
		ans+=dp(lidx)-dp(ridx+1); //符合L,R之間的部分
	}
	ans+=M;
	ans%=M;
	ans+=dp(idx+1); 
	ans%=M;
	return ans;
}
int main(){Accel
	cin>>s>>l>>r;
	Re(f,-1);
	n=SZ(s);
	L=SZ(l);
	R=SZ(r);
	string tmp=l+'#'+s;//解決單模式字串匹配的方法 S為文本 T為模式串 P=T+'#'+S 考慮S和P的關係
	vector<int>zl,zr;
	zl=z_f(tmp);
	REP(i,n){
		int idx=zl[i+L+1];
		if(idx<L&&i+idx<n&&s[i+idx]<l[idx])
			inc[i]++;
	}
	tmp=r+'#'+s;
	zr=z_f(tmp);
	REP(i,n){
		int idx=zr[i+R+1];
		if(idx<R&&i+idx<n&&s[i+idx]>r[idx])
			decr[i]++;
	}
	int ans=dp(0)-dp(1);
	ans+=M;
	ans%=M;
	cout<<ans<<endl;
	
}
```
