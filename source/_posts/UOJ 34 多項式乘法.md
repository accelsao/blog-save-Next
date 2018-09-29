---
title: UOJ 34 多項式乘法
tags:
  - FFT
  - UOJ
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-28 18:55:19
---

FFT分析&模板

<!--more-->

FFT是甚麼??

$$C(x)=A(x)*B(x)$$
\\(N^2\\)的算法 直接降到 \\(NlogN\\)

如何做的?

這邊先引入**單位複數根**的概念
\\(w^n=1\\)的複數w 根剛好有n個{0,1...,n-1},\\(e^{2\pi ik/n}\\)
更棒的是它可以寫成\\(y=cos(2\pi k/n)+isin(2*\pi k/n)\\)
[Eulers formula](https://en.wikipedia.org/wiki/Euler%27s_formula) makes our life easier

# 單位複數根
## Lemma 1
$$W^{dk}_{dn}=(e^{2\pi i/dn})^{dk}=(e^{2\pi i/n})^k=W^{k}_n$$

## Lemma 2
$$W^{n/2}_n=W^{n/2}_{2*(n/2)}=W^1_2=e^{2\pi i/2}=w^{\pi i}=-1$$

經典公式:
$$e^{i\pi}=cos(\pi)+isin(\pi)=-1$$

## Lemma 3
$$(W^k_n)^2=W^{2*k}_{2*n/2}=W^k_{n/2}$$
$$(W^{k+n/2}_n)^2=W^{2k+n}_{n}=W^{2k}_n\cdot W^n_n=(W^k_n)^2$$

得出結論:
$$(W^k_n)^2=(W^{k+n/2}_n)^2=W^k_{n/2}$$

## Lemma 4
$$ W^{k+n/2}_n=e^{2\pi i*(k+n/2)/n}=e^{2\pi i*(k/n+1/2)}=e^{2\pi i *k/n}*e^{\pi i}=W^{k}_n * (-1) $$

$$W^{k+n/2}_n=-W^{k}_n$$

# FFT
$$A(x)=a_0+a_1x+a_xx^2+...+a_{n-1}x^{n-1}$$
用到分治法囉!!!
按照odd,even分
$$
\begin{align}
A_{even}=a_0+a_2x+...+a_{n-2}^{n/2-1}\\
A_{odd}=a_1+a_3x+...+a_{n-1}^{n/2-1}\\
A(x)=A_{even}+x*A_{odd}
\end{align}
$$

看到 $$(W^0_n)^2,(W^1_n)^2,(W^{n-1}_n)^2$$
記得Lemma 3嗎? 我們只要算到n/2就好了



考慮\\( A(x)=A^{even}_{x^2}+x*A^{odd}_{x^2} \\)

Now,\\( A(W^k_n)=A^{even}(W^{2k}_n)+W^k_n*A^{odd}(W^{2k}_n) \\),if k<n/2

And if \\(k\geq n/2\\),then \\(A(W^k_n)=A^{even}(W^{k-n/2}_{n/2})-W^{k-n/2}_n*A^{odd}(W^{k-n/2}_{n/2})\\)
Proof:

$$
\begin{align}
A^{even}(W^{2k}_n)+W^k_n*A^{odd}(W^{2k}_n) &= A^{even}(W^{k}_{n/2})+W^k_n*A^{odd}(W^{k}_{n/2}),\text{using Lemma 3}\\
A^{even}(W^{k}_{n/2})+W^k_n*A^{odd}(W^{k}_{n/2})&=A^{even}(W^{k}_{n/2})-W^{k-n/2}_n*A^{odd}(W^{k}_{n/2}),\text{using Lemma 4} \\
\end{align}
$$

(5)
since \\(W^{K+n/2}_n=-W^{K} , K=-n/2\\)



## Pseudocode from CLRS


RECURSIVE-FFT(a)
n=a.length()
If n=1 then return a #Base Case 
\\(w_n=e^{2\pi i/n}\\)
w=1
\\(a^{even}=(a_0,a_2,,,a_{n-2})\\)
\\(a^{odd}=(a_1,a_3,,,a_{n-1})\\)
\\(y^{even}=RECURSIVE-FFT(a^{even})\\)
\\(y^{odd}=RECURSIVE-FFT(a^{odd})\\)
for k=0 to n/2-1:

\\(\;\;\;\;y_k=y^{even}_k + w*y^{odd}_k\\) 


\\(\;\;\;\;y_{k+n/2}=y^{even}_k-w*y^{odd}_k\\)
	
	
\\(\;\;\;\;w*=wn\\)

return y 

# Bit reverse

![](https://i.imgur.com/lEe3Stg.jpg)


我們利用dp的概念
當我們處理到i的時候
i的反轉就相當於把前面1~len-1位的整串反轉 再把最後一位的數放在前面
例如001->100
1+ R(00) = 100
R(00)=00
或者 11100->00111
0+ R(1110) =00111
R(1110)=0111

R(x)=x的反轉

# 逆FFT
待續...


# Code
```cpp
#include<bits/stdc++.h>

using namespace std;
const int N=3e5+1;

const double pi=acos(-1.0);

int L,n,m;
int r[N];//bit reverse

struct data{
	double x,y;
	data(double X=0,double Y=0){
		x=X,y=Y;
	}

}a[N],b[N];

data operator +(data a,data b){
	return data(a.x+b.x,a.y+b.y);
}
data operator -(data a,data b){
	return data(a.x-b.x,a.y-b.y);
}
data operator *(data a,data b){
	return data(a.x*b.x-a.y*b.y,a.y*b.x+a.x*b.y);
}
void fft(data a[N],int opt){
	for(int i=0;i<n;i++)
		if(i<r[i])swap(a[i],a[r[i]]);////處理好的bit reverse 直接swap調整位置
		
	for(int i=1;i<n;i<<=1) {
	
		data wn=data(cos(pi/i),opt*sin(pi/i));
		int p=i<<1;
		for(int j=0;j<n;j+=p){
			data w=data(1,0);
			for(int k=0;k<i;k++,w=w*wn){
				data x=a[j+k],y=w*a[j+k+i];
				a[j+k]=x+y;
				a[j+k+i]=x-y;
				
			}
		}
	}


}
int main(){
	
	cin>>n>>m;
	for(int i=0;i<=n;i++)cin>>a[i].x;
	for(int j=0;j<=m;j++)cin>>b[j].x;
	m+=n;
	for(n=1;n<=m;n<<=1)L++;
	//cout<<L<<endl;
	for(int i=0;i<n;i++){
		r[i]=(r[i>>1]>>1)|((i&1)<<(L-1));
	}
	fft(a,1);fft(b,1);
	for(int i=0;i<n;i++)a[i]=a[i]*b[i];
	fft(a,-1);
	for(int i=0;i<=m;i++)
		cout<<(int)(a[i].x/n+0.5)<<" ";
	cout<<endl;
		
}
```

# Reference
[快速傅里叶变换(FFT)]{https://zhuanlan.zhihu.com/p/42565742}
[Tutorial on FFT/NTT — The tough made simple. ( Part 1 )](https://codeforces.com/blog/entry/43499)