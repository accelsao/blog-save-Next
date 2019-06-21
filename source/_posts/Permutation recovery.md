---
title: Codeforces Round 559 C - Permutation recovery
tags:
  - constructive algorithms
  - data structures
  - dfs and similar
  - graphs
  - greedy
  - math
  - sortings
categories:
  - Codeforces
mathjax: true
date: 2019-06-19 17:18:32
---

解題心得
<!--more-->

# Problem
給你一個陣列A
希望你組成一個n的permutaion可以滿足nxt[i]除了-1以外的條件
nxt[i] = 最小的j, i<j && a[i]<a[j]

# Solution
我們可以發現如果對於任意pair(i,j) i<j<nxt[i]<nxt[j] 則無解
因為 當i<nxt[i] 代表a[i+1]-a[nxt[i]-1] 這些數字必定都小於a[i] 否則nxt[i] 就會更小
上面的條件可以看到j<nxt[i] 所以a[i]>a[j] 但是nxt[j] 一定不會超過nxt[i] 因為我們知道a[j]<a[i]<a[nxt[i]]

以上條件可以用priority_queue維護當前最小的nxt[i]

如果nxt[i]=-1 就把她調整成nxt[i]=i+1 因為這是最小可行的且也不會發生上面那個條件 i<j=nxt[i]<nxt[j]

再來把建邊 nxt[i] -> i, 大小關係就成立了 
從大到小開始配值
再來注意的是 同樣的children index小的要先配值 這樣才能保證i>j 如果i<j 那nxt[i]就會變成j了

把個nxt[i]>i 且都有值 所以可以保證存在一個permutaion

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 5e5+10;
const int M = 998244353;
typedef long long ll;

int n;
vector<int> g[N];
int num;
int b[N];
void dfs(int u){
	b[u]=num--;
	for(auto v:g[u]){
		dfs(v);
	}
}
int main(){
	ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
	int t;
	cin>>t;
	while(t--){
		cin>>n;
		vector<int> a(n+2);
		for(int i=1;i<=n+1;i++){
			g[i].clear();
		}
		int min_nxt = n+1;
		bool fl=0;
		priority_queue<int> _q;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			if(fl)continue;
			if(a[i]==-1){
				a[i]=i+1;
			}
			while(_q.size()&&-_q.top()<=i)_q.pop();
			if(_q.size()){
				int u=-_q.top();
				if(u<a[i]){
					fl=1;
				}
			}
			_q.push(-a[i]);
			g[a[i]].push_back(i);
		}
		if(fl){
			cout<<"-1\n";continue;
		}
		
		num=n+1;
		dfs(n+1);

		for(int i=1;i<=n;i++){
			cout<<b[i]<<" ";
		}
		cout<<"\n";
		
	}
	
	
	
}

```

 