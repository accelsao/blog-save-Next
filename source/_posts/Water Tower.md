---
title: CS Academy Round 86 - Water Tower
tags:
  - Implementation
categories:
  - CS Academy
mathjax: false
date: 2019-06-10 15:16:43
---

解題心得
<!--more-->

# Problem
給你一座水塔 一開始裝滿水高H
給你Q個小洞 有時間t和位置x
水高如果高於小洞的位置就會流
每個洞流速為 1 volume/s
問多久後水都會流光, 流不完輸出-1

# Solution
流的過程中要注意的是 如果流到一半的水位低於某個小洞 就要減少流的量
一開始把這些洞按照時間排序, 然後只要把洞高放入priority_queue就可以O(1)得知當前最高小洞的位置
如此一來只要水位一低於當前最高小洞 那就要減少流的量
每次只要考慮到下一個洞**之前** 會流到哪個高度同時洞口都比當前水位低
如果 cur_H(目前水高) - delta(到下一洞發生的時間-當前時間) x number(會流水的小洞數量) < h(目前最高的小洞位置)
就要先減少最高小洞的流水
可以先加入時間很大的小洞 (inf, 任意數)來保證讀完Q個小洞後可以繼續處理水流 delta會相當大

# Notes
1. 一開始沒處理好cur_time的部分
2. 還有while迴圈只要處理水位低於最高小洞要減少的情況就好 可以流到下一個小洞的話就拉出去處理

# Code
```cpp
#include <bits/stdc++.h>
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
//#pragma GCC optimize(2)
using namespace std;
const int N=1e6+1;
const int M=1e9+7;
const double eps=1e-8;
typedef complex<double>C;
const double PI(acos(-1.0));
//for(int mask=i;mask>0;mask=(mask-1)&i)
//int a[25]= { 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97 };
//__gcd, atan2(y,x)=y/x , __int128
//c(n,k)=c(n-1,k-1)+c(n-1,k)
//c(i,r),i for r to n =c(n+1,r+1)
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//void add(int x){while(x<N)BIT[x]++,x+=x&-x;}
//int sum(int x){int t=0;while(x){t+=BIT[x];x-=x&-x;}return t;}
//LL pw(LL a,LL b){LL t=1;for(;b;b>>=1,a=a*a%M)b&1?t=t*a%M:0;return t;}
//log() = ln() , log(x)/log(y)=log(y)-base-x
//INT_MAX 127,INT_MIN 128
//int dw[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
//int dw[8][2]={{0,1},{1,0},{0,-1},{-1,0},{1,1},{1,-1},{-1,1},{-1,-1}};
//cout<<fixed<<setprecision(12)<<ans<<endl;
//__builtin_popcount(mask)

int main(){Accel
 	int Q;   
 	double H;
 	cin>>H>>Q;
 	vector<pair<int ,int>> a(Q);
	for(int i=0;i<Q;i++){
		cin>>a[i].first>>a[i].second;
	}
	a.push_back(make_pair(M, M));
	sort(a.begin(),a.end());
    Q = a.size();
	priority_queue<double> q;
	
	double cur_t=0;
	for(int i=0;i<Q;i++){
		
		while(q.size()){
		    double delta = a[i].first - cur_t;
		    double h = 1.0 * q.top();
		    if(h - H > eps){
			    q.pop();
			    continue;
			}
			int cnt = q.size();
            
            if(H - delta * cnt - h < eps){
                cur_t += (H - h) / cnt;
                q.pop();
                H = h;
            }
            else{
                break;
            }
		}
        double delta = a[i].first - cur_t;
        int cnt = q.size();
        if(H - delta * cnt <= 0){
            if(cnt > 0){
                cur_t += H / cnt;
            }
            printf("%lf\n", cur_t);
            return 0;
        }
        if(a[i].second > H){
            continue;
        }
		H -= delta * cnt;
		cur_t = a[i].first;
		q.push(a[i].second);
		
	}
    cout<<-1<<endl;
}
```