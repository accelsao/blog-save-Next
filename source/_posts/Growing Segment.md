---
title: CS Academy Round 86 - Growing Segment
tags:
  - data structures
categories:
  - CS Academy
mathjax: false
date: 2019-06-10 20:38:14
---

解題心得(未完) Test 20~25 穩定WA 
沒有頭緒 快忘掉後再重寫一遍
<!--more-->

# Problem

# Solution

# Notes
測試set insert(n)放for(i=1 to n-1)前面和後面速度上的差異

# Code
```cpp
#include <bits/stdc++.h>
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
//#pragma GCC optimize(2)
using namespace std;
const int N=1e6+1;
const int M=1e9+7;
const double eps=1e-8;
typedef complex<double>C;
const double PI(acos(-1.0));
typedef long long ll;

set<int> s;
priority_queue<pair<int ,int> > Q;
ll cur_v;
vector<int> a;
void Erase(set<int>::iterator it){
	set <int>::iterator pre = it;
	set <int>::iterator nxt = it; nxt++;
	if(pre != s.begin()){
		pre--;
		cur_v -= abs(a[*it] - a[*pre]);
		if(nxt != s.end()){
			cur_v -= abs(a[*it] - a[*nxt]);
			cur_v += abs(a[*pre] - a[*nxt]);
		}
	}
	else if(nxt != s.end()){
		cur_v -= abs(a[*it] - a[*nxt]);
	}
	s.erase(it);
}
void Update(set<int>::iterator it){
	set <int>::iterator pre = it;
	set <int>::iterator nxt = it; nxt++;
	if(pre != s.begin()){
		pre--;
		int len = -abs(a[*it] - a[*pre]);
//		int rhs = a[*it] >= a[*pre] ? *it:*pre;
		int rhs = *it;
		Q.push(make_pair(len, rhs));
	}
	else if(nxt !=s.end()){
		int len = -abs(a[*it] - a[*nxt]);
//		int rhs = a[*it] >= a[*nxt] ? *it:*nxt;
		int rhs = *nxt;
		Q.push(make_pair(len, rhs));
	}
}


int n, q;
int ans[N];
int main(){Accel
	cin>>n>>q;
	a.resize(n+1);	
	for(int i=1;i<=n;i++)
		cin>>a[i];
	s.insert(n);
	for(int i=1;i<n;i++){
		if(a[i] <= min(a[i-1], a[i+1]) ||
		a[i] >= max(a[i-1], a[i+1])){
			s.insert(i);
		}
	}
	
	for(auto& x:s)
		cout<<x <<" ";cout<<"\n";
	
	vector<pair<int ,int> > b(q);
	for(int i=0;i<q;i++){
		cin>>b[i].first;
		b[i].second=i;
	}
	sort(b.begin(), b.end());
	
	
	
	cur_v = 0;
	auto cur = s.begin();
	auto nxt = cur; nxt++;
	while(nxt != s.end()){
		int u=*cur, v=*nxt;
		int len = abs(a[u]-a[v]);
//		int rhs = a[u] >= a[v] ? u:v; // right index
		cur_v += len;
		Q.push(make_pair(-len, v));
		cur++;
		nxt++;
	}
	cout<<cur_v<<endl;
	for(int i=0;i<q;i++){
		int len = b[i].first;
		int idx = b[i].second;
		while(Q.size()){
			int seg = -Q.top().first;
			if(seg > len) break;
			int rhs = Q.top().second;
			Q.pop();
			int l, r; // pre, nxt
			auto cur = s.find(rhs); 
			if(cur == s.end()) continue;
			auto pre = cur;
			auto nxt = cur;nxt++;
			if(pre == s.begin()){
				l = -1;
			}
			else{
				pre--;
				l = *pre;
			}
			if(nxt == s.end()){
				r = -1;
			}
			else{
				r = *nxt;
			}
			
			if(l != -1 && r != -1){
				Erase(cur);
				Update(nxt);
				Update(pre);
			}
			else if(r == -1){
				Erase(cur);
				Update(pre);
			}

		}
		ans[idx] = cur_v - (ll)len * (s.size() - 1);

		int u=*s.begin();
		if(a[u] < 0){
			ans[idx] += -a[u];
		}
		else{
			ans[idx] += max(a[u] - len, 0);
		}
	}
	for(int i=0;i<q;i++){
		cout<<ans[i]<<endl;
	}
}
```