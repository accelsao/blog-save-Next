---
title: UVa 719 Glass Beads
tags:
  - 最小表示法
  - Strings
  - UVa
  - Two Pointer
  - 後綴自動機
categories:
  - Competitive Programming
mathjax: true
date: 2018-10-01 17:17:22
---


# Problem
Give you a string
求最小表示法

<!--more-->


# 解法

## 最小表示法

定義i為最小的字串 <-維護他
j指針指向當前檢查的
時間複雜度O(n)
有點Two Pointer思想

## Code
```cpp
#include <bits/stdc++.h>
#define LL long long
#define Accel ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
#define ALL(X) (X).begin(), (X).end()
#define FOR(I, A, B) for (int I = (A); I <= (B); I++)
#define REP(I, N) for (int I = 0; I < (N); I++)
#define PII pair<int,int>
#define PLL pair<LL,LL>
#define SZ(a) ((int)a.size())
using namespace std;

int T;
string s;
int main(){Accel
	cin>>T;
	while(T--){
		cin>>s;
		int i=0,j=0,n=SZ(s),k;
		while(i<n&&j<n){
			if(i==j)j++;
			k=0;
			while(k<=n&&s[(i+k)%n]==s[(j+k)%n])k++;
			if(s[(i+k)%n]>s[(j+k)%n])i+=k+1;
			else j+=k+1;
		}
		cout<<i+1<<endl;
	}
}
```

## 後綴自動機

聽說可以用SAM? 

待填坑

[日月卦長的模板庫](http://sunmoon-template.blogspot.com/2015/05/suffix-automaton.html)
[[教學][String][Data Structure] 後綴自動機(Suffix Automaton)](http://codingbeans.blogspot.com/2016/01/data-structure-suffix-automaton.html)