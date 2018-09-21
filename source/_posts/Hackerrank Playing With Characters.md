---
title: Hackerrank Playing With Characters
tags:
- C
- Hackerrank
categories:
- Programming language
  
mathjax: true
date: 2018-09-18 20:50:25
---
 
# Probelm

輸入
```
C
Language
Welcome To C!!
```

輸出一樣


<!--more-->


# Note
You can take a string as input in C using `scanf(“%s”, s)`. But, it accepts string only until it finds the first space.

In order to take a line as input, you can use `scanf("%[^\n]%*c", s);` where  is defined as char s[MAX_LEN] where  is the maximum size of .
 Here, `[]` is the scanset character. `^\n` stands for taking input until a newline isn't encountered.
 Then, with this `%*c`, it reads the newline character and here, the used `*` indicates that this newline character is discarded.

# Code
```c
#include<stdio.h> 
int main(){
	char s[100],sen[100];
	char ch;
    scanf("%c\n%[^\n]%*c%[^\n]%*c", &ch,&s,&sen);
    printf("%c\n%s\n%s\n",ch,s,sen);
}
```

`%[^\n]%*c` 如上述所說 才會讀至換行 類似 getline(cin,s) `%*c` 讀換行且discarded
