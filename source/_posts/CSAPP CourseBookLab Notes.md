---
title: CSAPP Course/Book/Lab Notes
tags:
  - Bitwise operator
categories:
  - Computer System
mathjax: true
date: 2018-10-09 22:56:03
---

CSAPP CMU 15-213 鎮校神課 (?
	
主要是LAB練習 書太厚了還不太想啃
視頻的話 應該不會寫才會去補
下定決心慢慢啃系統了...希望可以堅持 (基本上絕不在window上搞linux 最討厭搞環境了 森77)

[Schedule](http://www.cs.cmu.edu/~213/schedule.html)
[bilibili](https://www.bilibili.com/video/av20304787)
[Github](https://github.com/accelsao/CS-APP/tree/master)
[Lab Assignments](http://csapp.cs.cmu.edu/3e/labs.html)

<!--more-->

# Data Lab

看來會學到不少黑魔法
目前寫起來基本上都有條件
**val都需要是32bit**

## 求ABS()
還有..現在才知道rightshift是implementation-dependent(根據機器不同的定義會發生不同的事情)
我想說為什麼-2147483648右移31會跑出-1...
```c
mask = x>>31
abs(x)=(x+mask) ^ mask
```
這裡假設右移的定義是**sign-extension且x is twos complement 32-bit value **
所以正數的mask會是0,負數會是-1(0xffffffff)
後面的操作 只有負數會受影響
x+mask 相當於 x-1
x^mask 相當於 取補數 (x+x補數)=-1

這樣求到補數因為先前有-1了所以剛好就會是原數
根本魔法

## 求not但是不使用'!'
也是有趣的一題
我們會發現到0要變1 非0要變0
要先找出不同點
考慮負數 0還是0 非0 = -(非0)
0 and 111111(\\(2^k-1\\)) =0
非0 and 11111(\\(2^k-1\\)) =非0
可惜上面這個好像沒啥用

用負數來算的話可以發現
如果x非0 sign bit 一定不同
相反的 一定相同
那我們對sign bit xor即可
```c
neg_x=(~x)+1;
sign_x=x>>31&1
sign_neg_x=neg_x>>31&1;
return !sign_x^sign_neg_x;
```

## bit count with bitwise operation
威力更強的黑魔法
這個例子直接說明清楚 感覺是分治? 這概念真是萬用
```
So if I have number 395 in binary 0000000110001011 (0 0 0 0 0 0 0 1 1 0 0 0 1 0 1 1)
After the first step I have:      0000000101000110 (0+0 0+0 0+0 0+1 1+0 0+0 1+0 1+1) = 00 00 00 01 01 00 01 10
In the second step I have:        0000000100010011 ( 00+00   00+01   01+00   01+10 ) = 0000 0001 0001 0011
In the fourth step I have:        0000000100000100 (   0000+0001       0001+0011   ) = 00000001 00000100
In the last step I have:          0000000000000101 (       00000001+00000100       )
```
```c
int bitCount(int x) {
	x = (x&0x55555555)+((x>>1)&0x55555555);
	x = (x&0x33333333)+((x>>2)&0x33333333);
	x = (x&0x0F0F0F0F)+((x>>4)&0x0F0F0F0F);
	x = (x&0x00FF00FF)+((x>>8)&0x00FF00FF);
	x = (x&0x0000FFFF)+((x>>16)&0x0000FFFF);
	return x;
}
# '+'比'&'優先
```
## bitParity
return 1 如果0有odd個(同時1也會是odd個)
這題我也想不到...沒想到可以用xor來**匹配**
而且也有分治的概念(還是要算二分?)
Too Crazy...
```c
int bitParity(int x){	
	x = x ^ (x<<16);
	x = x ^ (x<<8);
	x = x ^ (x<<4);
	x = x ^ (x<<2);
	x = x ^ (x<<1);
	return (x>>31)&1;
}
```

## BitReverse

寫FFT的時候會用到呢 實用的黑魔法 雖然FFT用DP建就好了
以下黑魔法只適用32bit
```c
int bitReverses(int x) {
    x = ((x>>1)&0x55555555) | ((x&0x55555555)<<1);
    x = ((x>>2)&0x33333333) | ((x&0x33333333)<<2);
    x = ((x>>4)&0x0F0F0F0F) | ((x&0x0F0F0F0F)<<4);
    x = ((x>>8)&0x00FF00FF) | ((x&0x00FF00FF)<<8);
    //x = ((x>>16)&0x0000FFFF) | ((x&0x0000FFFF)<<16); x&0x0000FFFF 因為只在後16bit and 起來也跟自己一樣 因此省略
    x = ((x>>16)&0x0000FFFF) | (x<<16);
    return x;
}
```
因為題目限制輸入type`int` 所以`>>`的時候會因為sign-extension跑出多餘的bit 所以要控制在30bit裡面 不要動到sign bit

## x ? y : z
就是常見的conditionl operation
x非0輸出y否則z
在一個有趣的問題 有點黑魔法
這是我的想法
```c
int conditional(int x, int y, int z) {
	int neg_one=~0;
	int Y=(!x)+neg_one,Z=!(!x)+neg_one;
	return (y&Y)|(z&Z);
}
```
這是別人的
```c
int conditional(int x, int y, int z) {
  return z ^ ((y ^ z) & ((!x) + ~0));
}
```

我的想法是先決定 Y還是Z + -1 讓他一個是0 一個是-1
這樣(-1)&(我要的) | (0)&(我不要的) =我要的

別人的想法是 非0的一樣出現-1 把**y,z 一個有一個沒有的抓出來**
再考慮 因為我非0要輸出Y 所以(上述結果)^(Z) 這樣其中**只有Z有的**就沒了,**只有Y有的**保留
如果x=0,整串都在跟0處理 結果為Z
想法超棒的

## dividePower2
給(x,n) 要求\\(x/2^n\\)
正數? 當然直接>>n
但負數 題目要求無條件捨去
所以(-33,2)=-2
但直接>>n 會出現-3
根據**觀察** 負數會無條件進位
解決方法就是加上\\(2^n-1\\)就OK了

```c
//int sign=(x>>31)&1+~0; 如果他不是sign-extension
int sign = x>>31;
這可能會常見到
效果是所有\\(x,x\geq0\\) sign=0 else -1
&val後可以有**正數沒變化**and**負數有val**
```

```c
int dividePower2(int x, int n) {
    int sign=x>>31;
    return (x + (1<<(n&sign))+ ~0 )>>n;
}
```

## fitsBits 
英文超破 看不是很懂...
return 1 if x can be represented as an n-bit, two's complement integer.
看了超超超久 終於看懂了...
[救我的blog](https://zhuanlan.zhihu.com/p/28335741)
[stackoverflow](https://stackoverflow.com/a/24668329/8322368)
只要x在**n-bit,2補數**的範圍內就是True
fitsBits(-5,4)=1
4的二補數範圍 非負:7~0 , 負:-8 ~ -1 
N的範圍(\\(2^N-1 , -(2^N)\\))
注意n-bit 要包含sign-bit 
所以除了sign bit外 第n位以上不是0就1 只有1~n-1位不一定 如果我們把32位 從32~1左到右來看的話
所以先往右移(n-1)位 那只會剩下全0or全1
+1後 只有0or 1的可能 那我們在右移1 就會是0
除此以外都是非0
```c
int fitsBits(int x, int n) {	
	return !(((x>>(n+(~0)))+1)>>1);
}
```