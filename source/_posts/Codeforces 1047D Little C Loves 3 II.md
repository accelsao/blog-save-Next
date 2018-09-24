---
title: Codeforces 1047D Little C Loves 3 II
date: 2018-09-17 09:54:04
tags:
- Math
- Codeforces
- Combinatorics
categories:
- Competitive Programming
mathjax: true
---

# Problem

給你N*M方格
每次可以放兩個棋子但他們的曼哈頓距離(Manhattan distance)必須是3
一直放到沒得放為止
問最佳的放法最終可以放幾個

<!--more-->




# Solution 
情況討論題

## \\(1\times M\\)
首先討論\\(1\times M\\)
因為只能放3,0這種直的 所有4以上才有答案 M+1 總數+2 可以和之前的一起放

XOOX
XXOXX
XXXXXX


**如果M=2的情況即可構出滿的 任意偶數M 都有\\(N\times M\\)**
**否則 就要考慮出4和6 若是滿的 對於\\(\geq 8\\)的偶數都可以由4和6組合**


## \\(Even \times Even \\)
先考慮兩個都偶數
除了\\(2\times 2\\)無解外
其餘都是\\(N\times M\\)

\\(2\times 4\\)
1234
3412

\\(2\times 6\\)=\\(1\times 6\\)+\\(1\times 6\\)

\\(M\geq 8\\)以上的偶數得解

接下來對於任意\\(偶數N\times 2\\) 就是\\(2\times 偶數N\\)

所以對於任意\\(偶數N\times 偶數M\\)
都能用\\(偶數N\times 2\\)解

**任意偶數N,M 得解**



## \\(Odd \times Even \\)

考慮奇數N 如果N=3
只有在M=2的時候不會滿

\\(3\times 2\\)
12
XX
21

\\(3\times 4\\)
1234
5615
2346

\\(3\times 6\\)
123453
678279
816945

接下來都可以分成4和6
\\(3\times 8\\)=\\(3\times 4\\)+\\(3\times 4\\)
\\(3\times 10\\)=\\(3\times 4\\)+\\(3\times 6\\)
if n>=8 n=4+4+x
if x%4=2 change 1 4 to 6

考慮N=5
\\(5\times 2\\)
12
34
51
42
35

\\(5\times 4\\)=\\(5\times 2\\)+\\(5\times 2\\)


考慮N=7
\\(7\times 2\\)
12
34
51
42
36
5X
6X

\\(7\times 4\\)=\\(3\times 4\\)+\\(4\times 4\\)
\\(7\times 6\\)=\\(3\times 6\\)+\\(4\times 6\\)
\\(7\times 偶數\geq 8\\)得解


\\(奇數\geq 9\\)可用 5+偶數解 因為N=5都是滿的 偶數也都是滿的 所以剩餘都是\\(N\times M\\)


## \\(Odd \times Odd \\)
\\(3\times 3\\)
123
3X4
412

\\(5\times 3\\)=\\(3\times 3\\)+\\(2\times 3\\)
123
374
412
5X6
675

\\(N\geq 7\\)
N=3+x x is evne and x>=4

\\(3\times 5\\)=\\(5\times 3\\)
\\(5\times 5\\)=\\(3\times 5\\)+\\(2\times 5\\)
\\(N\geq 7\\) 同理

剩餘同理交換N,M而已

# Code
```cpp
#include <bits/stdc++.h>
#define LL long long
using namespace std;
LL n,m;
void ans(LL x){
	cout<<x<<endl;exit(0);
}
int main(){
	cin>>n>>m;
	if(n>m)swap(n,m);
	if(n==1){
		ans(m-m%6+max((m%6-3)*2,0LL));
	}
	if(n%2!=m%2){
		if(m&1)swap(n,m);
		if((n==7||n==3)&&m==2)ans(n*m-2);
		ans(n*m);
	}
	if(n%2==0){
		if(n==2&&m==2)ans(0);
		ans(n*m);
	}
	ans(n*m-1);
}
```