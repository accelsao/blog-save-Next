---
title: Algorithms Review 1903
tags:
  - Competitive Programming
categories:
  - Learning Notes
mathjax: true
date: 2019-03-25 11:47:38
---

紀錄各種題目的思路,解法,困難,tricks

待補詳細:
CF 1105E
CF 1110D

<!--more-->

# Codeforces
## [1105E Helping Hiasat](http://codeforces.com/contest/1105/problem/E)
有一堆人要操作一個帳號 他們希望每次操作的時候是自己的名子, 
操作 1 -> 可以改名 2 -> 代表他們上線

### Tutorial
因為每次在操作1之前名子都是固定的 所以我們在兩個1之間的一堆2 只能讓某個人happy
ex. 1 2 2 2 1 2 2 1 2
考慮每個人出現在那些group中 用bit狀態表示
1 -> 1100
2 -> 0110
3 -> 0011
4 -> 0001
最佳解就是選擇盡量多的人 (兩兩and是0)
這裡用bitset來處理01 然後對於兩兩有交集的a[i][j]=false 來處理他們不能同時出現的情況
然後考慮選擇的順序 用suffle順序 很多次來窮舉(我不會算這邊的失敗機率... 這問題留著 但幾千次幾乎就可以AC 40人的情況下)

{% fold Code %}
```cpp
#include <bits/stdc++.h>
#define ld long double
#define LL long long
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
#define endl "\n"
//#pragma GCC optimize(2)
using namespace std;
//typedef complex<double>C;
//const double PI(acos(-1.0));
//const double eps(1e-8);
//for(int mask=i;mask>0;mask=(mask-1)&i)
//int a[25]= { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97 };
//__gcd, atan2(y,x)=y/x , __int128
//c(n,k)=c(n-1,k-1)+c(n-1,k)
//c(i,r),i for r to n =c(n+1,r+1)
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){int s=0;while(x){s+=BIT[x];x-=x&-x;}return s;}
//int find(int x){return x==p[x]?x:p[x]=find(p[x]);}
//LL pw(LL a,LL b){LL t=1;for(;b;b>>=1,a=a*a%M)b&1?t=t*a%M:0;return t;}
//log() = ln() , log(x)/log(y)=log(y)-base-x
//INT_MAX 127,INT_MIN 128
//int dw[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
//int dw[8][2]={{0,1},{1,0},{0,-1},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
//cout<<fixed<<setprecision(12)<<ans<<endl;
//__builtin_popcount(mask)

int n,m;
map<string,int>name;
int get_id(string s){
	return name[s]?name[s]:name[s]=name.size();
}
const int N=45;
bitset<100100>bs[N];
bool cmp(int a,int b){
	return bs[a].count()<bs[b].count();
}
int main(){
	name.clear();
	cin>>n>>m;
	int op;
	int gp=0;
	for(int i=0;i<n;i++){
		cin>>op;
		if(op==1){
			gp++;
		}
		else{
			string s;
			cin>>s;
			int k=get_id(s);
			bs[k][gp]=1;		
		}
	}
	vector<vector<int>>g(m+1,vector<int>(m+1,1));
	
	for(int i=1;i<=m;i++)
		for(int j=i+1;j<=m;j++)
			if((bs[i]&bs[j]).count()){
				g[i][j]=0;
				g[j][i]=0;
			}
	
	
	int df=m*m;
	int ans=0;
	while(df--){
		vector<int>p(m+1);
		for(int i=1;i<=m;i++)p[i]=i;
		random_shuffle(p.begin()+1,p.end());
		vector<int>vis(m+1,0);
		int z=0;
		for(int i=1;i<=m;i++){
			int gg=0;
			int x=p[i];
	//		cout<<x<<" --\n";
			for(int j=1;j<=m;j++)
				if(vis[j]&&!g[x][j])gg=1;
			if(!gg){
				vis[x]=1;
				z++;
			}
		}
		ans=max(ans,z);
	}
	printf("%d\n",ans);
	
	
	
}

/*
11 4
1
2 a
1
2 a
2 b
1
2 b
2 c
1
2 c
2 d

*/
```
{% endfold %}


## [1110D Jongmah](http://codeforces.com/contest/1110/problem/D)
給你一堆數字 [x,x+1,x+2]or[x,x,x]都可以加分 用過不用的情況下最多可以有幾分

### Tutorial
3個[x,x+1,x+2]都可以轉換成3個[x,x,x,]
所以我們需要考慮的是0,1,2個 3個相鄰數的情況
基於x這張牌只有3種組合會用到它[x-2,x-1,x],[x-1,x,x+1],[x,x+1,x+2]
DP[i][j][k]代表考慮到第i張牌 且當前牌拿了k張湊成[x-2,x-1,x]的情況和j張[x-1,x,x+1]的情況和(所有牌-j-k)[x,x+1,x+2]的情況
DP[i][j][k]=DP[i-1][l][j]+k+(all the car-j-k-l)/3 (for l 0~2)
all the car 是計算同樣x 3張的情況

每次只要考慮i-1 所以只需要2維

{% fold Code %}
```cpp
#include <bits/stdc++.h>
#define ld long double
#define LL long long
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
#define endl "\n"
//#pragma GCC optimize(2)
using namespace std;
//typedef complex<double>C;
//const double PI(acos(-1.0));
//const double eps(1e-8);
//for(int mask=i;mask>0;mask=(mask-1)&i)
//int a[25]= { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97 };
//__gcd, atan2(y,x)=y/x , __int128
//c(n,k)=c(n-1,k-1)+c(n-1,k)
//c(i,r),i for r to n =c(n+1,r+1)
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){int s=0;while(x){s+=BIT[x];x-=x&-x;}return s;}
//int find(int x){return x==p[x]?x:p[x]=find(p[x]);}
//LL pw(LL a,LL b){LL t=1;for(;b;b>>=1,a=a*a%M)b&1?t=t*a%M:0;return t;}
//log() = ln() , log(x)/log(y)=log(y)-base-x
//INT_MAX 127,INT_MIN 128
//int dw[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
//int dw[8][2]={{0,1},{1,0},{0,-1},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
//cout<<fixed<<setprecision(12)<<ans<<endl;
//__builtin_popcount(mask)L

const int N=1e6+10;
int n,m;
int a[N],c[N];
int main(){
	cin>>n>>m;
	int x;
	for(int i=0;i<n;i++){
		cin>>x;
		a[x]++;
	}
	vector<vector<int>>dp(3,vector<int>(3,0));
	for(int i=1;i<=m;i++){
		vector<vector<int>>new_dp(3,vector<int>(3,0));
		for(int x=0;x<3;x++)
			for(int y=0;y<3;y++)
				for(int z=0;z<3;z++){
					if(a[i]>=x+y+z){
						new_dp[y][z]=max(new_dp[y][z],dp[x][y]+z+(a[i]-x-y-z)/3);
					}
				}
		swap(dp,new_dp);
	}
	cout<<dp[0][0]<<endl;
}
```
{% endfold %}

## [1083B The Fair Nut and Strings](http://codeforces.com/contest/1083/problem/B)
給你字串a,b(只有a or b)
你可以選至多k個len=n的字串在[a,b]區間內
這k個字串的所有prefixes最大 重複不計
Example:
2 4
aa
bb
你可以選 aa,ab,ba,bb 共4個都在[aa,bb]區間內
所有prefixes有 a,aa,ab,b,ba,bb 共6個
### Tutorial
想法居然超級簡單 ... 我真嫩
對於len-1的所有可能的prefixes, 先假定後面接`a or b` 所以\\(num=num*2\\)
```
xxxa
xxxb
```
但是如果他a字串是`b` 我們就不會有a的可能 num-1,同理b字串`a`的情況 num-1
其中要以防num溢位 num=min(num,1<<61) //用1<<61 *2以後就爆了
{%fold Code%}
```cpp
#include <bits/stdc++.h>
using namespace std;
int n,k;
const int N=57890;
string a,b;
int main(){
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin>>n>>k;
	cin>>a>>b;
	long long num=1,ans=0;
	for(int i=0;i<n;i++){
		num=num*2;
		if(a[i]=='b')num--;
		if(b[i]=='a')num--;
		num=min(num,1LL<<61);
		ans+=min(num,1LL*k);
	}
	cout<<ans<<endl;
	
	
}
```
{%endfold%}

# Atcoder
## [AtCoder Grand Contest 032](https://atcoder.jp/contests/agc032/tasks)
### A
給你一個empty sequence a, 和N個操作
第\\( i \\)次操作 選擇一個\\(j\\) \\(1\leq j \leq i\\), 從第\\( j \\)的position插入 \\(j\\)
目標是變成b
{%fold Example1%}
3
1 2 1
依序丟入 1 1 2
1: 1
1: 1 1
2: 1 2 1
{%endfold%}

{%fold Example2%}
9
1 1 1 2 2 1 2 3 2
依序丟入 1 2 2 3 1 2 2 1 1
1: 1
2: 1 2
2: 1 2 2
3: 1 2 3 2
1: 1 1 2 3 2
2: 1 2 1 2 3 2
2: 1 2 2 1 2 3 2
1: 1 1 2 2 1 2 3 2
1: 1 1 1 2 2 1 2 3 2
{%endfold%}
#### Tutorial
這裡可以想到一個特性 當我插入j的時候 我會把原本j~end的數字都往後推
換言之 我把推完的seq拿掉j就可以還原了
所以我們每次找rightmost的pos[j]=j的數字
這個order就和插入的相反
{%fold Code%}
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N=1e6+10;

int main(){
	int n;
	cin>>n;
	vector<int>a(n);
	for(int i=0;i<n;i++)cin>>a[i];
	vector<int>b(n);
	for(int i=0;i<n;i++){
		int k=1,rm=-1;
		for(int j=0;j<n;j++){
			if(a[j]==k)
				rm=j;
			if(a[j])k++;
		}
		if(rm==-1){
			cout<<rm<<endl;return 0;
		}
		b[i]=a[rm];
		a[rm]=0;
		
	}
	reverse(b.begin(),b.end());
	
	for(auto x:b)
		cout<<x<<endl;		
}

```
{%endfold%}