---
title: Euler totient function
tags:
  - null
categories:
  - null
mathjax: true
date: 2019-05-14 19:27:45
---

構建 Euler's totient function C++
Input: \\(n ,n < 10^6\\)
Ouput: \\(\phi(n)\\)
<!--more-->
# Formula
公式都在wiki
我們用的是
$$
\phi(mn) = \phi(m) * \phi(n) * \frac{d}{\phi(d)}, d = gcd(m,n)
$$
所以我們窮舉質數m 和 n從2開始
if n%m==0:
$$
\phi(mn) = \phi(m * m * n/m) = \phi(m) * \phi(m * n/m) * \frac{m}{\phi(m)} = \phi(m * n/m) * m
$$
直到n%m!=0
如下
# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e6+10;
int main(){
	// Euler's totient function
	vector<int>phi(N);
	phi[1]=1;
	for(int i=2;i<N;i++){
		if(phi[i] == 0){
			phi[i] = i - 1;
			//printf("phi[%d] = %d\n", i, i - 1);
			for(int j=2; j * i < N; j++){
				if(phi[j]==0)continue;
				int k = j;
				int z = phi[i];
				while(k % i == 0){
					k /= i;
					z *= i;
				}
				
				phi[i * j] = z * phi[k];
				//printf("phi[%d] = %d\n",i * j, phi[i * j]);
			}
		}
	}
	int t;
	cin>>t;
	while(t--){
		int n;
		cin>>n;
		cout<<phi[n]<<endl;
	}
	
}

```

# References
[wiki](https://en.wikipedia.org/wiki/Euler%27s_totient_function)
[blog](http://peikailiao.blogspot.com/2013/08/euler-function.html)