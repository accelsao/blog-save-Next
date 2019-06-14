---
title: Codeforces Round 562 C - And Reachability
tags:
  - bitmasks
  - dp
categories:
  - Codeforces
mathjax: true
date: 2019-06-14 14:46:38
---

解題心得
<!--more-->

# Problem
給arr a 給你詢問l,r
是否存在arr p, \\(p_1 = l < p_2 < p_3 < ... < p_k = r\\) 且相鄰兩個&不為0
Here & denotes the bitwise AND operation.

# Solution
要形成相鄰&不為0的p 代表他們兩個之間會有至少1個共有的bit
```
如 [2, 10, 14, 6] 
0010
1010
1100
0110
```
每次對於a[i]只要找到同樣包含bit k的a[j], i<j 就可以一直&
nxt[i][j] = smallest j that \\(a[j]&(1<<j) == 1\\)
nxt[i][k] = min { nxt[last[j]][k] }
last[j] 會找到同樣包含bit j的右邊最小的index

所以只要找到所有a[r]有bit j 且 nxt[l][j] <= r 就代表l可以一直&到r

# Notes
精神就是DP紀錄上一次可行解的位置 一直更新當前位置右邊中的最小可行解
從右邊掃到左邊就做完了

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 3e5 + 10;
const int M = 998244353;
typedef long long ll;



int n, q;
int a[N];
int ls[N];
int nxt[N][25];
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>q;
	
	for(int i=0;i<n;i++){
		cin>>a[i];
	}
	for(int i=0;i<20;i++){
		ls[i]=n;
		nxt[n][i]=n;
	}

	for(int i=n-1; i>=0; i--){
		for(int j=0;j<20;j++){
			nxt[i][j]=n;
		}
		for(int j=0;j<20;j++){
			if((a[i]>>j)&1){
				for(int k=0;k<20;k++){
					nxt[i][k] = min(nxt[i][k], nxt[ls[j]][k]);
				}
				ls[j] = i;
				nxt[i][j] = i;
			}
		}
	}
	
	int l,r;
	while(q--){
		cin>>l>>r;
		l--,r--;
		bool f = 0;
		for(int j=0;j<20;j++){
			f |= ((a[r]>>j) & 1) && (nxt[l][j]<=r);
		}
		if(f){
			puts("Shi");
		}
		else{
			puts("Fou");
		}
	}
	
}
```