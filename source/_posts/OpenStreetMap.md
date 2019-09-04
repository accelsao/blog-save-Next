---
title: Codeforces Round 574 E - OpenStreetMap
tags:
  - data structures
  - two pointers
categories:
  - Codeforces
mathjax: false
date: 2019-07-20 16:47:28
---

解題心得
<!--more-->

# Notes
一個簡單的deque問題寫的有夠糟 當兵太久真的遲鈍

# Code 1
```cpp
#include <bits/stdc++.h>
using namespace std;

#define F first
#define S second
typedef long long ll;
const int N = 3e3 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;

ll d[N][N];
int x, y, z;
int n, m, a, b;
int main(){


	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m>>a>>b;
	vector<ll> g(n*m);
	cin>>g[0]>>x>>y>>z;
	int k=n*m;
	for(int i=1;i<k;i++){
		g[i]=(g[i-1]*x+y)%z;
	}
	k=0;
	for(int i=1;i<=n;i++)
		for(int j=1;j<=m;j++){
			d[i][j]=g[k++];
		}
		
	vector<deque<ll>> dq(m+1);
	for(int j=1;j<=m;j++){
		for(int i=1;i<a;i++){
			while(dq[j].size()&&dq[j].back()>d[i][j]){
				dq[j].pop_back();
			}
			dq[j].push_back(d[i][j]);
		}
	}


	ll ans=0;
	
	for(int i=a;i<=n;i++){
		for(int j=1;j<=m;j++){
			while(dq[j].size()&&dq[j].back()>d[i][j]){
				dq[j].pop_back();
			}
			if(i>a&&dq[j].size()&&(dq[j].front()==d[i-a][j])){
				dq[j].pop_front();
			}

			dq[j].push_back(d[i][j]);
		}
		deque<ll>q;
		
		for(int j=1;j<=b;j++){
			while(q.size()&&q.back()>dq[j].front()){
				q.pop_back();
			}
			q.push_back(dq[j].front());
		}
		ans+=q.front();
		for(int j=b+1;j<=m;j++){
			
			while(q.size()&&q.back()>dq[j].front()){
				q.pop_back();
			}
			
			if(q.size()&&q.front()==dq[j-b].front())q.pop_front();
			q.push_back(dq[j].front());
			ans+=q.front();
		}

	}
	cout<<ans<<endl;
}

```

# Code 2
```cpp
#include <bits/stdc++.h>
using namespace std;

#define F first
#define S second
typedef long long ll;
const int N = 3e3 +10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e18;

vector<int>d[N];
int x, y, z;
int n, m, a, b;
vector<int> sqz(const vector<int>& a, int len){
	deque<int> q;
	vector<int> b;
	for(int i=0;i<a.size();i++){
		while(q.size()&&q.back()>a[i]){
			q.pop_back();
		}
		if(i>=len&&q.size()&&q.front()==a[i-len])q.pop_front();
		q.push_back(a[i]);
		if(i>=len-1){
			b.push_back(q.front());
		}
		
		
	}
	return b;
}
int main(){


	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>m>>a>>b;
	vector<ll> g(n*m);
	cin>>g[0]>>x>>y>>z;
	for(int i=1;i<n*m;i++)
		g[i]=(g[i-1]*x+y)%z;
	
	
	int k=0;
	for(int i=0;i<n;i++){
		vector<int> bv(m);
		for(int j=0;j<m;j++){
			bv[j]=g[k++];
		}
		d[i]=sqz(bv, b);		
	}
	
	ll ans=0;
	for(int j=0;j<m-b+1;j++){
		vector<int> bv(n);
		for(int i=0;i<n;i++){
			bv[i]=d[i][j];
		}
		vector<int> f=sqz(bv, a);
		ans=accumulate(f.begin(), f.end(), ans);
	}
	cout<<ans<<endl;


}

```