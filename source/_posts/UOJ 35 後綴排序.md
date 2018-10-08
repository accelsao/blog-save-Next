---
title: UOJ 35 後綴排序
tags:
  - Suffix array
  - Strings
  - UOJ
  - Doubling Algorithm
  - Difference Cover mod 3
  - DC3
categories:
  - Competitive Programming
mathjax: true
date: 2018-10-01 17:39:28
---

# Problem

給你一個字串 求他後綴排序
還有排序後相鄰兩個的LCP(Longest Common Prefix)

[題目](http://uoj.ac/problem/35)

<!--more-->


# 後綴數組
用來處理String的東西
比起後綴樹更好寫,且複雜度不輸,所占空間又小
其中用到兩種算法
倍增算法和DC3

## 倍增算法

## Code
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
const int N=123456;
const int M=1e9+7;

int c[N];//基數排序輔助 
int t1[N],t2[N];
char s[N];
int n;
int sa[N];//suffix array
int ra[N],h[N];//rank,height
void da(char *s,int *sa,int *ra,int *h,int m=128){//double algorithm
	n++;
	//用bucket sort排序suffix 
	int i,j,p,*x=t1,*y=t2;
	for(i=0;i<m;i++)c[i]=0;
	for(i=0;i<n;i++)c[x[i]=s[i]]++;
	for(i=1;i<m;i++)c[i]+=c[i-1];//前綴和  關鍵字i 至少是第幾名
	for(i=n-1;~i;i--)
		sa[--c[x[i]]]=i;
		
	/*cout<<"SA---\n";
    for(i=0;i<n;i++)
    	cout<<sa[i]<<" ";cout<<endl;
	*/
	
	for(j=1;;j<<=1){//一直排序到所有的名次都不同
		p=0;
		//y[i]是"第二關鍵字排名" i 所在第一關鍵字的位置 
		for(i=n-j;i<n;i++)//因為後面j個不會有關鍵字 所以他們的排序會在前面 相當於他們關鍵字是空字元 
			y[p++]=i;
		for(i=0;i<n;i++)//剩下的對第二關鍵字 排序 
			if(sa[i]>=j)//排名>=j的對"第二關鍵字" 才有貢獻 
				y[p++]=sa[i]-j;
				
		for(i=0;i<m;i++)c[i]=0;
		for(i=0;i<n;i++)c[x[y[i]]]++;
		for(i=1;i<m;i++)c[i]+=c[i-1];
		for(i=n-1;~i;i--)sa[--c[x[y[i]]]]=y[i];
		
		//接下來把x當作rank y是舊的x 
		swap(x,y);
		p=1;//排序指標
		x[sa[0]]=0;//第一個直接給值 
		
		//生成下一次的第一關鍵字 
		for(i=1;i<n;i++)//如果他跟前面的字元相同排名(前綴相等) 
			x[sa[i]]=(y[sa[i]]==y[sa[i-1]]&&y[sa[i]+j]==y[sa[i-1]+j])?p-1:p++;
			
			
		/*cout<<"x---\n";
    	for(i=0;i<n;i++)
	    	cout<<x[i]<<" ";cout<<endl;   
        */
		if(p>=n)break;
		m=p;
	}
	int k=0;
	n--;
	//RA(SA(i))=i
	for(i = 0; i <= n; i++) ra[sa[i]] = i;
	//求LCP
	
	for(i=0;i<n;i++){
		if(k)k--;
		j=sa[ra[i]-1];
		while(s[i+k]==s[j+k])k++;
		h[ra[i]]=k;
	}
	
}
int main(){
	
	while(~scanf("%s",s)){
		n=strlen(s);
		da(s,sa,ra,h);
		for(int i=1;i<=n;i++)
			cout<<sa[i]+1<<" ";cout<<endl;
		for(int i=2;i<=n;i++)
			cout<<h[i]<<" ";cout<<endl;
	}
}

```
## 參考
[模板](https://blog.csdn.net/HowardEmily/article/details/75913511)
[后缀数组 最详细讲解](https://www.cnblogs.com/victorique/p/8480093.html)
[后缀数组（详细）](https://blog.csdn.net/jokes000/article/details/7839686)

