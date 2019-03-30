---
title: UOJ 34 多項式乘法
tags:
  - FFT
  - UOJ
categories:
  - Algorithms
mathjax: true
date: 2018-09-28 18:55:19
---

FFT如何幫乘法加速

<!--more-->

$$C(x)=A(x)*B(x)$$
\\(N^2\\) 的算法 直接降到 \\(NlogN\\)

如何做的?

這邊先引入**單位複數根**的概念
和神奇的公式 \\(e^{i\theta}=cos(\theta) + i * sin(\theta)\\)
\\(w^n=1\\) 的複數w 根剛好有n個
$$w^0_{n},w^1_{n} ... w^{n-1}_{n}$$
where \\(w_{n}=e^{2\pi i/n}\\)

## Lemma 1
$$W^{dk}_{dn}=(e^{2\pi i/dn})^{dk}=(e^{2\pi i/n})^k=W^{k}_n$$

## Lemma 2
$$W^{n/2}_n=W^{n/2}_{2*(n/2)}=W^1_2=e^{2\pi i/2}=e^{\pi i}=cos(\pi) + i * sin(\pi) = -1$$

## Lemma 3
$$(W^k_n)^2=W^{2*k}_{2*n/2}=W^k_{n/2}$$
$$(W^{k+n/2}_n)^2=W^{2k+n}_{n}=W^{2k}_n\cdot W^n_n=(W^k_n)^2$$

得出結論:
$$(W^k_n)^2=(W^{k+n/2}_n)^2=W^k_{n/2}$$

## Lemma 4
$$ W^{k+n/2}_n=e^{2\pi i*(k+n/2)/n}=e^{2\pi i*(k/n+1/2)}=e^{2\pi i *k/n}*e^{\pi i}=W^{k}_n * (-1) $$

$$W^{k+n/2}_n=-W^{k}_n$$

# FFT - Coefficient form to Point value form

首先我們希望n是power of 2, 所以把不足的係數補0

按照odd,even分

$$\begin{equation}
\begin{aligned}
A_{even} &= a_0 + a_2 * x + ... + a_{n-2} * x^{n/2-1} \\
A_{odd}  &= a_1 + a_3 * x + ... + a_{n-1} * x^{n/2-1} \\
A(x) &= A_{even}(x^2) + x * A_{odd}(x^2)
\end{aligned}
\end{equation}\label{eq1}$$


我們知道 $ A\_{even}(x^{2}) $ 和 $ A\_{odd}(x^2) $ 需要 $ (w\_n^0)^2,(w\_n^1)^2,...,(w\_n^{n/2-1})^2 $
根據**Lemma 3**可以改為 $ w\_{n/2}^0,w\_{n/2}^1,...,w\_{n/2}^{n/2-1} $

Now, $ A(w^k\_n) = A\_{even}(w^{2k}\_n) + w^{k}\_{n} \ast A\_{odd}(w^{2k}\_n) $, if $ k < n/2 $
And if $ k\geq n/2 $, then $ A(w^k\_n)=A^{even}(w^{k-n/2}\_{n/2})-w^{k-n/2}\_n\ast A^{odd}(w^{k-n/2}\_{n/2}) $

**Proof:**
$$
\begin{align}
A_{even}(w^{2k}_n)+w^k_n*A_{odd}(w^{2k}_n) &= A_{even}(w^{k}_{n/2})+w^k_n*A_{odd}(w^{k}_{n/2}),\text{using Lemma 3}\\
A_{even}(w^{k}_{n/2})+w^k_n*A_{odd}(w^{k}_{n/2})&=A_{even}(w^{k}_{n/2})- w^{k-n/2}_n\ast A_{odd}(w^{k}_{n/2}),\text{using Lemma 4} \\
\end{align}
$$
since \\(k' = k+n/2\\)
\\(w_n^{k'+n/2}= -w^{k'}\\)



## Pseudocode from CLRS


RECURSIVE-FFT(a)
n=a.length()
If n=1 then return a #Base Case
$ w\_n = e^{2\pi i/n} $
w = 1

\\(a^{even}=(a\_0,a\_2,,,a\_{n-2})\\)
\\(a^{odd}=(a\_1,a\_3,,,a\_{n-1})\\)
\\(y^{even}=RECURSIVE-FFT(a^{even})\\)
\\(y^{odd}=RECURSIVE-FFT(a^{odd})\\)
for k=0 to n/2-1:

\\(\;\;\;\;y_k=y^{even}_k + w*y^{odd}_k\\) 


\\(\;\;\;\;y_{k+n/2}=y^{even}_k-w*y^{odd}_k\\)
	
	
\\(\;\;\;\;w\;\ast =w_n\\)

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

和FFT 只差 \\(-1/n\\)


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

## 差不多版本
```cpp
#include <bits/stdc++.h>
using namespace std;
const double pi=acos(-1);
const int N=2e5;
int n,m;
int rev[N],ans[N];
char a[N],b[N];
struct Complex{
	double real,image;
	Complex(double _real=0,double _image=0):real(_real),image(_image){}
	Complex operator + (const Complex rhs){
		return Complex(real+rhs.real,image+rhs.image);
	}
	Complex operator - (const Complex rhs){
		return Complex(real-rhs.real,image-rhs.image);
	}
	Complex operator * (const Complex rhs){
		return Complex(real*rhs.real-image*rhs.image,real*rhs.image+image*rhs.real);
	}
}A[N],B[N];



int read(){
	int len_a,len_b;
	scanf("%s %s",a,b);
//	puts(a);
//	puts(b);
	len_a=strlen(a);
	len_b=strlen(b);
	int n,m,l;
	m=len_a+len_b;
	for(int i=0;i<len_a;i++)
		A[i].real=a[len_a-i-1]-'0';
//	for(int i=0;i<len_a;i++)
//		printf("%.lf ",A[i].real);
//	puts("");
	for(int i=0;i<len_b;i++)
		B[i].real=b[len_b-i-1]-'0';
	l=0;
	for(n=1;n<m;n<<=1)l++;
	for(int i=0;i<n;i++)
		rev[i]=(rev[i>>1]>>1)|((i&1)<<(l-1));
	return n;
} 

void FFT(Complex a[],int n,int sign){
//	for(int i=0;i<n;i++)
//		printf("%lf ",a[i].real);puts("");
	
	for(int i=0;i<n;i++)
		if(rev[i]<i){
			swap(a[i],a[rev[i]]);
		}
//	for(int i=0;i<n;i++)
//		printf("%lf ",a[i].real);puts("");

	for(int i=2;i<=n;i<<=1){
		Complex dw(cos(2*pi/i),sin(2*pi*sign/i));
		for(int j=0;j<n;j+=i){
			Complex w(1,0);
			for(int k=0;k<(i>>1);k++){
				Complex u=a[j+k]; //y_k(0)
				Complex v=a[j+k+(i>>1)]*w; //y_k(1) * w 
				a[j+k]=u+v;
				a[j+k+(i>>1)]=u-v;
				w=w*dw;
			}
		}
	}
	if(sign==-1)
		for(int i=0;i<n;i++){
//			printf("%lf ",a[i].real);
//			ans[i]=(int)round(a[i].real/n); 
			ans[i]=(int)(a[i].real/n+0.5);
		}

}
void print_FFT(int n){
	for(int i=0;i<n;i++)
		if(ans[i]>=0){
			ans[i+1]+=ans[i]/10;
			ans[i]%=10;
		}
	int m=n-1;
	while(!ans[m])m--;
	for(int i=m;~i;i--)
		printf("%d",ans[i]);
	printf("\n");
}
int main(){	
	int n=read();
//	for(int i=0;i<n;i++)
//		printf("%lf ",A[i].real);
//	puts("");
//	for(int i=0;i<n;i++)
//		printf("%d ",rev[i]);puts("");
	FFT(A,n,1);
//	for(int i=0;i<n;i++)
//		printf("%lf ",B[i].real);
//	puts("");
	FFT(B,n,1);
	
	for(int i=0;i<n;i++){
//		printf("%lf %lf\n",A[i].real,B[i].real);
		A[i]=A[i]*B[i];
	}
	FFT(A,n,-1);
	print_FFT(n);		
}
/*
3 3
123 
324
*/
```

# Reference
[快速傅里叶变换(FFT)](https://zhuanlan.zhihu.com/p/42565742)
[Tutorial on FFT/NTT — The tough made simple. ( Part 1 )](https://codeforces.com/blog/entry/43499)