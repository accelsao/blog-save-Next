---
title: Codeforces Round 546 E - Nastya Hasn't Written a Legend
tags:
  - binary search
  - data structures
categories:
  - Codeforces
mathjax: true
date: 2019-06-23 15:29:49
---

解題心得
<!--more-->

# Problem
給你array a, k 
if $$a_i < a_{i-1} + k_{i-1}, \;a_i = a_{i-1} + k_{i-1}$$
兩種操作
```
s l r: 輸出l,r 的sum
+ pos x: a[pos]+x
```

# Solution
兩種方法
1:
我們令b[i]=a[i]-k[1,i-1] 會發現b是遞增的 (如果a已經調整過)
那麼當a[i]+=x 那接下來b就需要增加x直到pos, b[pos]>=b[i]+x
b[i]~b[pos-1]都改成b[i]+x
用SegmentTree維護b, k預處理因為他不會變動
需要維護sum, tag
void push -> lazy update
pos 用二分搜找
2:
令b為a[i]-a[i-1]-k 同時將未調整確認的index放入set
等同於求upperbound就可以找到上述的pos
然後用BIT維護區間sum
最特別的是他用2個BIT處理 為了處理區間
ADD的時候記錄下標 FIND的時候才能計算val*len
add: f[0]+val, f[1]-val*(pos-1)
find: return f[0]*pos+f[1]
所以當你a[i]+x 我們會一直找到upperbound(pos) 然後調整他的值(-y)如果<0就移除掉並且調整y

# Notes
看來還是沒把SegmentTree熟練 也沒想到BIT仍能處理 用兩個BIT實現**區間改值**

# Code

## Segment Tree
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e5+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e17;


ll tag[N<<2],sum[N<<2];
ll a[N], b[N], s[N], k[N];
int n,q;

void build(int v,int l,int r){
	
	tag[v]=INF;
	if(l==r){
		sum[v]=b[l];
		return ;
	}
	int m=(l+r)>>1;
	build(v<<1, l, m);
	build(v<<1|1, m+1, r);
	sum[v]=sum[v<<1]+sum[v<<1|1];
}
void push(int v,int l,int r){
	int m=(l+r)>>1;
	if(tag[v]!=INF){
		sum[v<<1] = tag[v]*(m-l+1);
		sum[v<<1|1] = tag[v]*(r-m);
		tag[v<<1]=tag[v<<1|1]=tag[v];
		tag[v]=INF;
	}
}
void upd(int v,int l,int r,int L,int R,ll val){
	if(l>=L && r<=R){
		tag[v]=val;
		sum[v]=val*(r-l+1);
		return ;
	}
	push(v,l,r);
	int m=(l+r)>>1;
	if(R<=m){
		upd(v<<1,l,m,L,R,val);
	}
	else if(m<L){
		upd(v<<1|1, m+1,r,L,R,val);
	}
	else{
		upd(v<<1,l,m,L,m,val);
		upd(v<<1|1,m+1,r,m+1,R,val);
	}	
	sum[v]=sum[v<<1]+sum[v<<1|1];
}
ll find(int v,int l,int r,int L,int R){
//	cout<<v<<endl;
	if(l>=L && r<=R){
//		cout<<l<<" "<<r<<endl;
		return sum[v];
	}
	int m=(l+r)>>1;
	push(v,l,r);
	if(R<=m){
		return find(v<<1,l,m,L,R);
	}
	else if(m<L){
		return find(v<<1|1,m+1,r,L,R);
	}
	else{
		return find(v<<1,l,m,L,m)+find(v<<1|1,m+1,r,m+1,R);
	}
}

int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	
	
	for(int i=1;i<=n;i++){
		cin>>a[i];	
	}
	for(int i=1;i<n;i++){
		cin>>k[i];
		k[i]+=k[i-1];
		s[i]=s[i-1]+k[i];
		b[i]=a[i]-k[i-1];
	}
	b[n]=a[n]-k[n-1];
	build(1, 1, n);
	
	cin>>q;
	while(q--){
		char c;int l,r,i,x;
		cin>>c;
		if(c=='s'){
			cin>>l>>r;
			// b + t
//			cout<<s[r-1]<<endl;
//			cout<<find(1, 1, n, l ,r)<<endl;
			ll ans = find(1, 1, n, l, r);
			ans += s[r-1]-(l>=2?s[l-2]:0);
			cout<<ans<<endl;
//			cout<< find(1, 1, n, l, r) + s[r-1]-(l>=2?s[l-2]:0)<<"\n";
		}
		else{
			cin>>i>>x;
			l=i, r=n;
			int pos=l, m;
			ll p=find(1, 1, n, i, i) + x; // b_i + x
			while(l<=r){
				m=(l+r)>>1;
				if(find(1, 1, n, m, m) >= p){
					r=m-1;
				}
				else{
					pos = m;
					l=m+1;
				}
			}
			upd(1, 1, n, i, pos, p);
		}
		
	}
	
	
}
```
## BIT
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const int N = 1e5+10;
const int M = 1e9 + 7;
const int inf = 1e9+7;
const ll INF = 1e17;


ll tag[N<<2],sum[N<<2];
ll a[N], b[N];
ll f[N][2];
int n,q;


ll add(int x, ll val){
	int pos=x;
	while(x<=n){
		f[x][0]+=val;
		f[x][1]-=val*(pos-1);
		x+=x&-x;
	}
}
ll find(int x){
	ll res=0;
	int pos=x;
	while(x){
		res+=f[x][0]*pos;
		res+=f[x][1];
		x-=x&-x;
	}
	return res;
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin>>n;
	
	for(int i=1;i<=n;i++){
		cin>>a[i];	
		add(i, a[i]);
		add(i+1, -a[i]);
	}
	set<int> s;s.insert(n+1);
	for(int i=2;i<=n;i++){
		int k;
		cin>>k;
		s.insert(i);
		b[i]=a[i]-a[i-1]-k;
	}
	char op;
	int x,y;
	cin>>q;
	while(q--){
		cin>>op>>x>>y;
		
		if(op=='+'){
			b[x]+=y;
			s.insert(x);
			auto it=s.upper_bound(x);
			add(x, y);
			add(*it, -y);
//			cout<<x<<" "<<*it<<endl;
			ll _y=y;
			while(*it<=n){
				int pos=*it;
//				cout<<b[pos]<<endl;
				b[pos]-=_y;
//				cout<<_y<<endl;
//				cout<<pos<<endl;
				
				if(b[pos]<0){
//					cout<<"123"<<endl;
					_y=-b[pos];
					s.erase(it);
					it=s.upper_bound(x);
					add(pos, -b[pos]);
					add(*it, b[pos]);
					b[pos]=0;
				}
				else{
					break;
				}
			}
			
		}
		else{
			cout<<find(y)-find(x-1)<<endl;
		
		}
	}
}
```