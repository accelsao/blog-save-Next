---
title: Haskell Learning
tags:
  - Haskell
categories:
  - Programming Language
  
mathjax: true
date: 2018-10-11 20:15:25
---

Haskell 相關
[99題](https://wiki.haskell.org/99_questions/1_to_10)
[github項目](https://github.com/bitemyapp/learnhaskell)
[Learn Haskell in 10 minutes](https://wiki.haskell.org/Learn_Haskell_in_10_minutes)

<!--more-->

# Compile with ghc

被ghc搞爆了...
好好地裝了個[ghc platform](https://www.haskell.org/platform/windows.html)懶人包
結果要用ghc編譯一直不知道從哪下手...
最後終於又被stackoverflow救了[here](https://stackoverflow.com/questions/6306233/compiling-haskell-hs-in-windows-to-a-exe)
直接新增Haskell到Username底下
再把hello.hs放進去用cmd打開
C:\Users\Username>ghc Haskell\hello.hs 這樣就可以compile了
出現如下
```
C:\Users\Jay>ghc Haskell/hello.hs
[1 of 1] Compiling Main             ( Haskell\hello.hs, Haskell\hello.o )
Linking Haskell\hello.exe ...
```
然後再輸入exe
```
C:\Users\Jay>Haskell\hello.exe
Hi

這樣就出現Hi了~
# 因為我放在Haskell底下所以輸入 Haskell\hello.exe
```

# 基本語法
```haskell
let a=5
給a值5
```
`(:)`conse 將數據增加列表上
```haskell
Prelude> let s=[1,2,3]
Prelude> 0:s
[0,1,2,3]


特別的是 他只能接受 val:list
不能 list:value(s:0)
不能 list:list(s:t) t=[2,3,4]
可以 0:1:s
不能 val:val(0:1)
```

tupple to list
```haskell
Prelude> (2,4):[]
[(2,4)]
Prelude> ([2,4],[2,2])
([2,4],[2,2])
Prelude> ([2,4],[],[2,2])
([2,4],[],[2,2])
Prelude> ([])
[]
Prelude> ():[]
[()]
```

```haskell
do{n<-readLn;print(n^2)}
input:4
output:16
```
Types
似乎是重要概念
```haskell
Prelude> 5.0::Int
error
Prelude> 5.0::Double
5.0
Prelude> 3::Double
3.0
Prelude> 1::Int
1
```
```haskell
Prelude> :t 1
1 :: Num p => p
Prelude> :t 1.0
1.0 :: Fractional p => p
Prelude> :t True
True :: Bool
Prelude> :t 'A'
'A' :: Char
Prelude> :t "aa"
"aa" :: [Char]
```

Case
```haskell
classify age = case age of 0 -> "newborn"
                           1 -> "infant"
                           2 -> "toddler"
                           _ -> "senior citizen"

main=do
	n <- readLn
	let s =classify n
	putStrLn s
```

Using libraries
```haskell
module Main where
import qualified Data.Map as M
errorsPerLine=M.fromList
	[("Jay",100),("PT",-100)]
main=do
	name <- getLine
	case M.lookup name errorsPerLine of
		Nothing -> putStrLn "404"
		Just val -> do {
			putStr "Errors per line: ";
			print val;
			print (2+3)			
		}
		
```

# 99Q - Q1
Get last element
```haskell
Prelude> s=[0,10,2]
Prelude> last(s)
2
Prelude> s
[0,10,2]
```
另外一個寫不出來 棄坑囉 F***
