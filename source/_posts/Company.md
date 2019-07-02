---
title: Codeforces Round 520 E - Company
tags:
  - binary search
  - data structures
  - dfs and similar
  - greedy
  - trees
categories:
  - Codeforces
mathjax: false
date: 2019-06-29 14:55:49
---

解題心得
寫了2hr #7 TLE 下回續戰...
<!--more-->

題目描述略

# Notes
這題重點
**多個節點的LCA可以發現 針對dfs序的入點找出最大和最小的點 這兩個點的LCA就是所有點的LCA**
因為用Set求最小兩個和最大兩個所以TLE
因為他是離線的 詢問的時候也不會變動 所以用SparseTable處理RMQ即可
還有也可以針對要skip的點u 求lca(lca(l,u-1),lca(u+1,r)) 就可以只求最小最大的兩個點了

# 複雜度分析
N為點的總數Q為詢問數
建立node的dfs序和depth O(n)
建立區間SparseTable O(nlog(n))
詢問ST O(log(n))
LCA建立 O(nlogn) 查詢O(log(n))

複雜度最多的是建立ST LCA的預處理和 每次詢問有一堆log(n)的查詢
總複雜度 O(nlog(n) + qlog(n)) 

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
 
typedef long long ll;
const int N = 1e5+10;
const int M = 1e9 + 7;
const int inf = 1234567890;
const ll INF = 1e18;
 
int p[N];
int n, q;
int blocksz;
vector<int> g[N];
int dep[N];
int df[N], dt;
int f[N][20];
int st_l[N][20];
int st_r[N][20];
struct Qry{
	int l, r, id;
	Qry(int _l=0,int _r=0,int _id=0):l(_l),r(_r),id(_id){}
};
bool cmp(const Qry& a,const Qry& b){
	return (a.l/blocksz < b.l/blocksz) || (a.l/blocksz == b.l/blocksz) && (a.r < b.r);
}
void dfs(int u,int p){
	df[u]=++dt;
	dep[u]=dep[p]+1;
	for(const auto& v:g[u]){
		if(v!=p){
			dfs(v, u);
		}
	}
}
int lca(int u,int v){
	if(dep[u]<dep[v]){
		int dif=dep[v]-dep[u];
		for(int i=19;i>=0;i--){
			if(dif>=(1<<i)){
				v=f[v][i];
				dif-=1<<i;
			}
		}
	}
	else{
		int dif=dep[u]-dep[v];
		for(int i=19;i>=0;i--){
			if(dif>=(1<<i)){
				u=f[u][i];
				dif-=1<<i;
			}
		}
	}
	while(u!=v){
		u=f[u][0];
		v=f[v][0];
	}
	return u;
}
 
pair<int,int> get_st(int l,int r){
	int len=r-l+1;
	int i=0;
	while(1<<(i+1)<=len)i++;
	int _l,_r;
	if(df[st_l[l][i]] < df[st_l[r-(1<<i)+1][i]]){
		_l=st_l[l][i];
	}
	else{
		_l=st_l[r-(1<<i)+1][i];
	}
	if(df[st_r[l][i]] > df[st_r[r-(1<<i)+1][i]]){
		_r=st_r[l][i];
	}
	else{
		_r=st_r[r-(1<<i)+1][i];
	}
	return make_pair(_l,_r);
}
 
int main(){
	
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n>>q;
	p[1]=1;
	for(int i=2;i<=n;i++){
		cin>>p[i];
		g[i].push_back(p[i]);
		g[p[i]].push_back(i);
	}
	
	// dfs��
	dt=0;
	dep[0]=-1;
	dfs(1, 0);
	for(int i=1;i<=n;i++){
		f[i][0]=p[i];
	}
	for(int j=1;j<20;j++)
		for(int i=1;i<=n;i++){
			f[i][j]=f[f[i][j-1]][j-1];
		}
	
//	printf("lca(%d,%d)=%d\n", 7,8,lca(7,8));
//	cout<<lca(4,6)<<endl;
	
//	for(int i=1;i<=n;i++){
//		printf("tr_lv[%d]=%d\n",i,tree_level[i]);
//	}
//	
//	for(int i=1;i<=n;i++){
//		printf("df[%d]=%d\n",i,df[i]);
//	}
	
	// �϶��̤j�̤p
	// offline ����
	vector<Qry>query(q);
	
	int l,r;
//	for(int i=0;i<q;i++){
////		cin>>l>>r;
//		
//		cin>>query[i].l>>query[i].r;
//		query[i].id=i;
//	}
//	cout<<"finish here\n";
//	blocksz=sqrt(n);
//	sort(query.begin(),query.end(),cmp);
//	for(int i=0;i<q;i++){
//		printf("%d %d\n", query[i].first, query[i].second);
//	}
	// ST
	for(int i=1;i<=n;i++){
		st_l[i][0]=i;
		st_r[i][0]=i;
	}
	for(int j=1;j<20;j++){
		for(int i=1;i<=n;i++){
			int _i=i+(1<<(j-1));
			if(_i>n)break;
			if(df[st_l[i][j-1]] < df[st_l[_i][j-1]]){
				st_l[i][j] = st_l[i][j-1];
			}
			else{
				st_l[i][j] = st_l[_i][j-1];
			}
			if(df[st_r[i][j-1]] > df[st_r[_i][j-1]]){
				st_r[i][j] = st_r[i][j-1];
			}
			else{
				st_r[i][j] = st_r[_i][j-1];
			}
//			st_l[i][j]=min(st_l[i][j-1], st_l[_i][j-1]);
//			st_r[i][j]=max(st_r[i][j-1], st_r[_i][j-1]);
		}
	}
//	set<pair<int,int>>	s;
//	int cur_l=1,cur_r=0;
	for(int i=0;i<q;i++){
		cin>>l>>r;
		int u,v;
		tie(u,v)=get_st(l,r);
		//printf("u:%d v:%d\n",u,v);	
 
		int ban_u_lca,ban_v_lca;
		int _u,_v;
		
		if(u==l){
			tie(_u,_v)=get_st(u+1,r);
			ban_u_lca=lca(_u,_v);
		}
		else if(u==r){
			tie(_u,_v)=get_st(l,u-1);
			ban_u_lca=lca(_u,_v);
		}
		else{
			tie(_u,_v)=get_st(l, u-1);
			int lhs=lca(_u,_v);
			tie(_u,_v)=get_st(u+1,r);
			int rhs=lca(_u,_v);
			ban_u_lca=lca(lhs,rhs);
		}
		
		
		if(v==l){
			tie(_u,_v)=get_st(v+1,r);
			ban_v_lca=lca(_u,_v);
		}
		else if(v==r){
			tie(_u,_v)=get_st(l,v-1);
			ban_v_lca=lca(_u,_v);
		}
		else{
			tie(_u,_v)=get_st(l, v-1);
			int lhs=lca(_u,_v);
			tie(_u,_v)=get_st(v+1,r);
			int rhs=lca(_u,_v);
			ban_v_lca=lca(lhs,rhs);
		}
		if(dep[ban_u_lca] > dep[ban_v_lca]){
			printf("%d %d\n", u, dep[ban_u_lca]);
		}
		else{
			printf("%d %d\n", v, dep[ban_v_lca]);
		}
		
		
	}
		
}
```


