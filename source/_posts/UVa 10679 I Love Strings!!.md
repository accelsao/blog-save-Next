---
title: UVa 10679 I Love Strings!!
tags:
  - Aho–Corasick algorithm
  - Strings
  - Codeforces
categories:
  - Competitive Programming
mathjax: true
date: 2018-09-26 22:53:35
---

10679 - I Love Strings!!
[傳送Gate](https://uva.onlinejudge.org/index.php?option=onlinejudge&Itemid=99999999&page=show_problem&category=&problem=1620&mosmsg=Submission+received+with+ID+22028113)

待補

<!--more-->


KMP
AC自動機
BM


# Code
## AC自動機
```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<set>
#include<vector>
#include<queue>
#include<stack>
#include<map>
using namespace std;
#define rep(a,b) for(int i = a; i <= b; i++)
#define dec(a,b) for(int i = a; i >= b; i--)
#define root 1,1,n
#define ls 2*rt
#define rs 2*rt+1
#define mid (L+R)/2
#define lson ls,L,mid
#define rson rs,mid+1,R
#define fi first
#define se second
#define mes(a) memset(a,0,sizeof(a))
typedef long long int ll;
const int mx = 1e6+5;
char s[mx];
int ans[mx];
bool vis[mx];
struct trie{
    int ch[mx][52];
    int v[mx],f[mx],last[mx];
	//v 每個單詞結尾 非0 
	// last[i]=j  代表j是i單詞後墜 
    int sz;
    void init(){mes(ch[0]),sz = 1,f[0] = v[0] = last[0] = 0;}
    int idx(char c){if(c>='a'&&c<='z')return c-'a';	return c-'A'+26;}
    void insert(char *s,int id){
        int len = strlen(s);
        int u = 0;
        for(int i = 0; i < len; i++){
            int d = idx(s[i]);
            if(!ch[u][d]){
                mes(ch[sz]);
                f[sz] = 0;
                v[sz] = 0;
                last[sz] = 0;
                ch[u][d] = sz++;
            }
            u = ch[u][d];
        }
        ans[id] = u;
        v[u] = 1;
    }
    void getfail(){
        queue<int>q;
        for(int d = 0; d < 52; d++){
            if(!ch[0][d]){
                ch[0][d] = 0;
                continue;
            }
            int ret = ch[0][d];
            last[ret] = f[ret] = 0;
            q.push(ret);
        }
        while(!q.empty()){
            int u = q.front();
            q.pop();
            for(int d = 0; d < 52; d++){
                if(!ch[u][d]){
                    ch[u][d] = ch[f[u]][d];
                    continue;
                }
                int ret = ch[u][d];
                f[ret] = ch[f[u]][d];
                last[ret] = v[f[ret]]?f[ret]:last[f[ret]];
                q.push(ret);
            }
        } 
    }
    void find(char *s){
    	int u = 0;
    	int len = strlen(s);
    	for(int i = 0; i < len; i++){
    		int d = idx(s[i]);
    		int ret = ch[u][d];
    		while(ret){
    			vis[ret] = 1;
    			ret = last[ret];
			}
			u = ch[u][d];
		}
    }
}word; 
char str[mx];
int main(){
	int t;
	scanf("%d",&t);
	while(t--){
		scanf("%s",s);
		int n;
		memset(vis,0,sizeof(vis));
		word.init();
		scanf("%d",&n);
		for(int i = 0; i < n; i++){
			scanf("%s",str);
			word.insert(str,i); 
		}
		word.getfail();
		word.find(s);
		for(int i = 0;i < n; i++){
			if(vis[ans[i]])
				puts("y");
			else
				puts("n");
		}
	}
	return 0;
}

```
```
2
abcdddd
5
abcde
a
dd
cdd
ABC
abcdddd
5
abcde
a
dd
cdd
DDDd
```



# 參考
[AC自动机	](https://www.jianshu.com/p/9249cedfb1aa)
[Code](https://blog.csdn.net/a1325136367/article/details/78888673)