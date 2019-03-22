---
title: Codeforces 1110E - Magic Stones
tags:
  - Math
categories:
  - Competitive Programming
mathjax: false
date: 2019-03-19 07:38:54
---

# Problem
給你array a和b
每次可以a的某一個數操作魔術
有以下效果 : a'[i]=a[i+1]+a[i-1]-a[i]
希望可以把它變成b 問你可不可行

<!--more-->

# 解法

考慮最後要變成一樣
第一項因為無法操作所以要一樣
a[0]==b[0]
再來考慮操作一數的變化
如果考慮差分 d[i]=a[i+1]-a[i]

a'[i] = a[i+1]+a[i-1]-a[i]
d'[i-1] = a'[i]-a[i-1] = a[i+1]-a[i] = d[i]
d'[i] = a[i+1]-a'[i] = a[i]-a[i-1] = d[i-1]

可發現d[i],d[i-1] 交換了
換言之 只要這n-1個差分在排序後相同 就有解 因為任意相鄰交換可以交換成有序
只要差分是相等的對於相同初值 就有相同數列

難就難在發現一次操作 差分居然有交換的性質

# Code

```cpp

const int N=1e5+100;
int n;
int a[N],b[N];
int main(){
	cin>>n;
	for(int i=0;i<n;i++)cin>>a[i];
	for(int i=0;i<n;i++)cin>>b[i];
	if(a[0]!=b[0]){
		cout<<"No\n";return 0;
	}
	for(int i=0;i<n-1;i++)
		a[i]=a[i+1]-a[i];
	for(int i=0;i<n-1;i++)
		b[i]=b[i+1]-b[i];
	sort(a,a+n-1);
	sort(b,b+n-1);
	for(int i=0;i<n-1;i++)
		if(a[i]!=b[i]){
			cout<<"No\n";return 0;
		}
	cout<<"Yes\n";
	return 0;
}

```
	

