---
title: UVa 10679 I Love Strings!!
tags:
  - AC自動機
  - Strings
  - UVa
  - KMP
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-26 22:53:35
---


待補
# Problem
字串匹配題 給你Text T和n個Pattern P 如果P在T裡面則輸出y否則n
[題目](https://uva.onlinejudge.org/index.php?option=onlinejudge&Itemid=99999999&page=show_problem&category=&problem=1620&mosmsg=Submission+received+with+ID+22028113)

<!--more-->

# 解法
模版題
以下用三種方法解


# KMP



## 時間複雜度分析
匹配文本Text 需要\\(O(len(T))\\)
每個Pattern建構nxt(fail函數)需要\\(O(len(P))\\)

這題需要\\(Q(O(len(T))+O(len(P)))\\)
Q是詢問數
但這題因為文本匹配到就可以結束匹配所以不會到O(len(T))


因為這坑很多人填了
所以我只負責開車[从头到尾彻底理解 KMP](http://wiki.jikexueyuan.com/project/kmp-algorithm/define.html)

## Code
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+10;
int T;
char s[N],t[N];
int nxt[N];
//建造fail函數 
void pre(){
	//s=Text t=Pattern
	int i=0,j=-1,n=strlen(t);
	nxt[0]=-1;
	while(i<n){
		if(j==-1||t[i]==t[j])
			nxt[++i]=++j;
		else j=nxt[j];//失配 指針指向上一個
	}
}
bool kmp(){
	pre();
	int i=0,j=0,slen=strlen(s),tlen=strlen(t);
	while(i<slen&&j<tlen){
		if(j==-1||s[i]==t[j])
			i++,j++;
		else
			j=nxt[j];
	}
	if(j==tlen)
		return 1;
	return 0;
}
int main(){
	scanf("%d",&T);
	while(T--){
		scanf("%s",s);
		int n;
		cin>>n;
		for(int i=0;i<n;i++){
			cin>>t;
			if(kmp())
				cout<<"y\n";
			else cout<<"n\n";
		}
	
	}
}

```

# AC自動機

## 分析

(待補)

[AC自动机-解釋](https://www.jianshu.com/p/9249cedfb1aa)
[模板來源](https://blog.csdn.net/a1325136367/article/details/78888673)

## Code
```cpp
#include<bits/stdc++.h>
#define Re(a,b) memset(a,b,sizeof a)
using namespace std;
const int N=1e6+10;
int T;
char s[N],t[N];
const int Sg=52;//sigma num of char type
int vis[N];
int ans[N];
struct Word{
	int ch[N][Sg];
	int fail[N];// 
	int end[N];//Pattern結尾標記 
	int last[N];
	int sz;

	void clear(){
		Re(ch[0],0);
		fail[0]=end[0]=last[0]=sz=0;	
	}
	int idx(char c){if(c>='a'&&c<='z')return c-'a';	return c-'A'+26;}
	void insert(char *s,int id){
		int len=strlen(s);
		int p=0;
		for(int i=0;i<len;i++){
			int x=idx(s[i]);
			if(!ch[p][x]){
				ch[p][x]=++sz;
				Re(ch[sz],0);
				fail[sz]=end[sz]=last[sz]=0;
			}
			p=ch[p][x];
		}
		end[p]=1;//結尾標記 字串index 
		ans[id]=p;
	}
	void get_fail(){
		queue<int> q;
		fail[0]=0; 
		//x stand for sigma 
		for(int x=0;x<Sg;x++) {
			if(ch[0][x]) {
				q.push(ch[0][x]) ;
				last[ch[0][x]]=fail[ch[0][x]]=0; 
			} 
		}
		while(q.size()) {
			int u=q.front();q.pop();
			//cout<<u<<endl;
			for(int x=0;x<Sg;x++) {
				if(!ch[u][x]) {
					ch[u][x]=ch[fail[u]][x]; //如果他沒有點的話 我們會直接給他fail指向的點的編號 
					continue;
				}
				
				int v=ch[u][x]; 

				q.push(v); 
				fail[v]=ch[fail[u]][x]; //fail[v] 是當前點u 的失配點  fail[u] 在trie上的編號 
				last[v]=end[fail[v]]?fail[v]:last[fail[v]];
				//如果fail[v]  就是結尾點 那我們直接標記last[v] 
				//否則 我們就需要找這個 {"非結尾點" 所標記的結尾點 } last[fail[v]] 

			} 
		} 
	
	}
	void find(char *s) {
		int p=0,len=strlen(s);
		
		for(int i=0;i<len;i++){
			int x=idx(s[i]);
			while(p&&!ch[p][x])p=fail[p];
			p=ch[p][x];
			int cur=p;
			while(cur&&!vis[cur]){
				vis[cur]=1;
				cur=last[cur];
			}
		}
	}
}word;
int main(){
	scanf("%d",&T);
	while(T--){
		scanf("%s",s);
		word.clear();
		memset(vis,0,sizeof(vis));
		int n;
		scanf("%d",&n);
		for(int i=0;i<n;i++){
			scanf("%s",t);
			word.insert(t,i);
			
		}
		word.get_fail();
		word.find(s);
		for(int i=0;i<n;i++){
			if(vis[ans[i]]){
				cout<<"y\n";
			}
			else cout<<"n\n";
		}
	}
}
```




# 題目Sample I/O
Input
```
2
abcdefghABCDEFGH
2
abc
abAB
xyz
1
xyz
```
Output
```
y
n
y
```


