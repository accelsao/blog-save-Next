---
title: Numpy, Pandas, Matplotlib
tags:
  - Numpy
  - Matplotlib
  - Pandas
categories:
  - Learning Notes
mathjax: false
date: 2018-07-04 15:55:16
---

Numpy, Pandas, Matplotlib cheatsheets

<!--more-->
# Numpy

## Numpy.random

### numpy.random.randint
```py
np.random.randint(10,size=(1,10))-5
[[-1 -5  0 -3  2  4  0  3  0  4]]
# range(0~9)-5
```

```py
# 將numpy取max(0,val)
a=np.random.randint(10,size=(1,10))-5
print(a)
a[a<0]=0
print(a)

[[ 2 -2  4 -3 -2  3 -5 -4  2 -2]]
[[2 0 4 0 0 3 0 0 2 0]]
```

### numpy.random.randn()
```py
Random floats sampled from Gaussian distribution of mean 0 and variance 1
For random samples from \\(N(\mu,\sigma^{2})\\)
`sigma  *  np.random.randn(...)  +  mu`

N(3, 6.25)=2.5 * np.random.randn(2, 4) + 3

```

## Numpy.choose

```py
choices = [[0, 1, 2, 3], [10, 11, 12, 13],
[20, 21, 22, 23], [30, 31, 32, 33]]

np.choose([2, 3, 1, 0], choices)
# np.choose(a,c)  ==  np.array([c[a[I]][I] for  I  in  ndi.ndindex(a.shape)])
# 輸出 choices[2][0],[3][1],[1][2],[0][4]
array([20, 31, 12,  3])

np.choose([2, 4, 1, -1], choices, mode='clip') 
# indices > n-1 -> n-1, < 0 -> 0
# a[i] -> a[min(max(i,0),n-1)]
array([20, 31, 12,  3])

np.choose([2, 4, 1, 0], choices, mode='wrap') 
# a[i]->a[i%n]
array([20,  1, 12,  3])
```


```py
a = [[2, 0, 1], [1, 2, 0], [1, 0, 1]]
choices = [1, 10, 100]
np.choose(a, choices)

array([[100,   1,  10],
       [ 10, 100,   1],
       [ 10,   1,  10]])
```


```py
# Level_1黑魔法
a = np.array([0, 1]).reshape((2,1,1))
c1 = np.array([1, 2, 3]).reshape((1,3,1))
c2 = np.array([-1, -2, -3, -4, -5]).reshape((1,1,5))
np.choose(a, (c1, c2))
# result is 2x3x5, res[0,:,:]=c1, res[1,:,:]=c2

array([[[ 1,  1,  1,  1,  1],
        [ 2,  2,  2,  2,  2],
        [ 3,  3,  3,  3,  3]],

       [[-1, -2, -3, -4, -5],
        [-1, -2, -3, -4, -5],
        [-1, -2, -3, -4, -5]]])
```

## Numpy.mean()

```py
import numpy as np  
a=np.random.randint(2,size=(1,3))
print(a)
b=np.random.randint(2,size=(1,3))
print(b)
c=(a==b).mean()
print(c)

[[0 1 1]]
[[0 0 1]]
0.6666666666666666
```

## Numpy.linspace

```py
import numpy as np  
a=np.linspace(2.0, 3.0, num=5)
print(a)
a=np.linspace(2.0, 3.0, num=5,endpoint=0)
print(a)
a=np.linspace(2.0, 3.0, num=5, retstep=1)
b,c=a
print(b)
print(c)

[2.   2.25 2.5  2.75 3.  ]
[2.  2.2 2.4 2.6 2.8]
[2.   2.25 2.5  2.75 3.  ]
0.25
```

## Numpy.choice

```py
import numpy as np  

# equivalent to np.random.randint(0,5,3)
a=np.random.choice(5, 3)
print(a)

# probability
a=np.random.choice(5,3,p=[0,0,0,0.1,0.9])
print(a)

# unique
a=np.random.choice(5, 3, replace=0)
print(a)

# unique+probability
a=np.random.choice(5, 3, replace=False, p=[0.1, 0, 0.3, 0.6, 0])
print(a)


[0 4 2]
[4 4 4]
[3 0 1]
[2 3 0]
```

## Numpy.unravel_index

```py
In[]:
import numpy as np
from numpy import *
idx = np.unravel_index([0,1,10,23], (4,2,3))
c=np.random.randint(20,size=(4,2,3))
print(c)
print('---')
print(idx)
print('---')
print(c[idx])
Out[]:
[[[10  7 15]
  [13 16 13]]

 [[ 4  6  3]
  [17 14 13]]

 [[ 3  0 15]
  [ 6  2  2]]

 [[16  5 19]
  [18 16  0]]]
---
(array([0, 0, 1, 3], dtype=int64), array([0, 0, 1, 1], dtype=int64), array([0, 1, 1, 2], dtype=int64))
---
[10  7 14  0]

輸出第0th,1,10,23個數字的index 在shape(4,2,3)裡面 0-index

而且把4個index分散成四個array 第一個array 是4個要求數字的第一維 以此類推

0-th index = 0,0,0 # 第一個
1-th index = 0,0,1 
10-th index = 1,1,1
23-th index = 3,1,2 # 最後一個

```

## Numpy.ufunc.at

可以對np.array a 的指定位置 做`ufunc`的操作
[ufunc](https://docs.scipy.org/doc/numpy-1.14.0/reference/ufuncs.html)

```py
import numpy as np
a = np.array([1, 2, 3, 4])
print(a)
np.negative.at(a, [0, 1])
print(a)
np.add.at(a,[0,1],1)
print(a)
b=np.array([2,4])
np.add.at(a,[2,3],b)
print(a)
np.power.at(a,[2,3],b)
print(a)

[1 2 3 4]
[-1 -2  3  4]
[ 0 -1  3  4]
[ 0 -1  5  8]
[   0   -1   25 4096]
```

## numpy.where
丟進去condition來判斷輸出什麼 True:x False:y
```py
input:
condition=[[True, False], [True, False]]
x=[[1, 2], [3, 4]]
y=[[9, 8], [7, 6]]
print(np.where(condition,x,y))
output:
[[1 8]
 [3 6]]

input: 
x = np.arange(9.).reshape(3, 3)
np.where( x > 5 )
output:
(array([2, 2, 2]), array([0, 1, 2]))
 
```


# Matplotlib.pyplot

## Inline
```py
# Levle_2 黑魔法

%matplotlib inline

# 將圖片在jupyter notebook裡顯示

```


# Pandas

## pandas.DataFrame.iloc
Purely integer-location based indexing for selection by position.
簡單說是用來選取範圍

```py
Input
df = pd.DataFrame({'A':[1,2,3],
                   'B':[4,5,6],
                   'C':[7,8,9],
                   'D':[1,3,5],
                   'E':[5,3,6],
                   'F':[7,4,3]})
				   
print(df.iloc[:, :-1]) 沒有F col
Output:
   A  B  C  D  E
0  1  4  7  1  5
1  2  5  8  3  3
2  3  6  9  5  6

print(df.iloc[:,-1:]) 只有F
Output:
   F
0  7
1  4
2  3

print(df.iloc[:,1:3])
Output:
   B  C
0  4  7
1  5  8
2  6  9

print(df.iloc[0:1,1:3])
Output:
   B  C
0  4  7

print(df.iloc[0:1,3:3]) 當你index輸入錯誤的時候
Output:
Empty DataFrame
Columns: []
Index: [0]

print(df.iloc[0:1,3:8]) 不過這種居然不會出錯
Output:
   D  E  F
0  1  5  7

print(df.iloc[2:-1,:5])
Output:
Empty DataFrame
Columns: [A, B, C, D, E]
Index: []

print(df.iloc[0:2,6:8])
Output:
Empty DataFrame
Columns: []
Index: [0, 1]
```
