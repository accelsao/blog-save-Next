---
title: Codeforces Global Round 3 E - Earth Wind and Fire
tags:
  - math
  - constructive algorithms
categories:
  - Codeforces
mathjax: true
date: 2019-06-06 15:08:21
---

解題心得
<!--more-->

# Problem
給你a,b array
你每次可以選擇 \\(a_i\\) 和 \\(a_j\\) 滿足 \\(a_i <= a_j\\) 和 d 滿足 \\(0 \leq d*2 \leq a_j-a_i\\)
操作: \\(a_i + d\\) 且 \\(a_j - d\\)
做完所有操作以後可以組成 array b (不計順序)

# Solution
首先我們把a, b sorting
原因1: 我們保證\\(a_i <= a_j\\) 每次從當前位置右邊的數搬過來d
原因2: 當我們先求出ab的差(\\(delta_i = b_i-a_i\\)) 我們可以發現若我們選擇i,j d1, d2是一正一負 也就是說我們可以從j 搬到 i
\\(d1=a_j-b_j \geq 0\\), \\(d2=b_i-a_i\geq 0\\)
\\(d = min(d1, d2), a_j-a_i\geq 2*d\\) 因為 \\(b_j \geq b_i\\)

接下來還要保證
所有 prefix of diff 不能<0 , 否則代表他需要搬走d, 但所有右邊的石頭又比他大, 所以無法搬
sum of diff =0

用stack維護數字

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N=2e5+10;
const int inf=1e9+10;
const int M=998244353;

typedef long long ll;

int n;
struct set_comp{
	bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs)const{
		return lhs.first > rhs.first;
	}
};

struct Op{
	int l, r, c;
	Op(int l,int r,int c):l(l),r(r),c(c){}
};
int main(){
	cin>>n;
	vector<pair<int, int>> a(n);
	vector<int> b(n);
	
	for(int i=0;i<n;i++){
		cin>>a[i].first;	
		a[i].second = i+1;
	}

	for(int i=0;i<n;i++){
		cin>>b[i];
	}

	sort(a.begin(), a.end());
	sort(b.begin(), b.end());
	
	ll sum=0;
	for(int i=0;i<n;i++){
		a[i].first = b[i] - a[i].first;
		sum += a[i].first;
		if(sum < 0){
			cout<<"NO\n";
			return 0;
		}
	}
	if(sum != 0){
		cout<<"NO\n";
		return 0;
	}
	
	
	stack<pair<int, int>> st;
	vector<Op> ans;
	
	for(int i=0;i<n;i++){
		if(a[i].first > 0){
			st.push(make_pair(a[i].first, a[i].second));
		}
		else if(a[i].first < 0){
			int x = -a[i].first;
			while(x > 0){
				int val, idx;
				tie(val, idx) = st.top();st.pop();
				if(val >= x){
					val -= x; 
					ans.push_back(Op(idx, a[i].second, x));
					x = 0;
					if(val > 0){
						st.push(make_pair(val, idx));
					}
				}
				else{
					x -= val;
					ans.push_back(Op(idx, a[i].second, val));
					val = 0;
				}
			}
		}
	}
	

	cout<<"YES\n";
	cout<<ans.size()<<endl;
	for(const auto& x:ans){
		printf("%d %d %d\n", x.l, x.r, x.c);
	}
		
}
/*
5
1000000000 1000000000 300000000 500000000 100000000
400000000 300000000 9000000000 100000000 300000000
*/
```