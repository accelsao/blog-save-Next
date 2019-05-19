---
title: Codeforces Round #561 (Div. 2) D - Cute Sequences
tags:
  - Math
categories:
  - Codeforces
mathjax: true
date: 2019-05-19 20:43:47
---

解題心得 (未完)
<!--more-->


# TO DO
用tutorial的解法

# Problem
給你a, b, m
構建一個數列符合以下條件


1. \\(n <= 50\\)
2. \\(A_1 = a\\) and \\(A_n = b\\)
3. Let
$$
S_i = \sum_{1}^{i}A_i
$$
, $$ 1 \leq S_i - S_{i-1} \leq m $$


# Code
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N=1e6+10;

int main(){
	
	vector<long long>pw2(55);
	// 1, 1, 2, 4
	pw2[0]=1;
	for(int i=1;i<55;i++)
		pw2[i] = 1LL<<(i-1);
	
	int T;
	cin>>T;
	while(T--){
		long long a,b,m;
		cin>>a>>b>>m;
		if(a==b){
			//cout<<a<<endl;
			printf("1 %lld\n", a);
			continue;
		}
		
		
		vector<long long>ans(55);
		vector<long long>lw(55);
		int len=-1;
		ans[0] = a;
		lw[0]=a;
		long long s = a;
		long long s_lw = a;
		for(int i=1;i<50;i++){
			ans[i] = s + m;
			lw[i] = s_lw + 1;
			if(ans[i] >= b){
				if(lw[i] <= b)
					len = i+1;
				else
					len = -1;
				break;
			}
			s_lw += lw[i];
			s += ans[i];
		}
		
		if(len==-1){
			cout<<-1<<"\n";
			continue;
		}
		
//		for(int i=0;i<len;i++)
//			cout<<ans[i]<<" ";cout<<"\n";
		
		for(int i=1;i<len;i++){
			if(ans[len-1]==b)break;
			long long k=min((ans[len-1]-b) / pw2[len-1-i], m-1);
			for(int j=i;j<len;j++){
				ans[j] -= k * pw2[j-i];
			}
			
		}
		assert(ans[len-1]==b);
		cout<<len<<" ";
		for(int i=0;i<len;i++)
			cout<<ans[i]<<" ";
		cout<<"\n";
	}

	
}

/*
1
3 50 46

1
2 100000000000000 100000000000


*/
```