---
title: Computer Organization and Architecture
tags:
  - null
categories:
  - Computer Science
mathjax: false
date: 2019-06-09 08:37:26
---

讀書筆記
<!--more-->

5.4.4
instruction per cycle = 0.5
data read = 250/1000  = 0.125
data write = 100/1000 = 0.05
total read bandwidth =
instruction bandwidth -> 0.003(instruction cache miss rate) * 64(block size) * 0.5(instruction per cycle)
 + data read bandwidth

(1)
我想問data read bandwidth的部分要不要考慮data write的 cycle比例?
0.02(data cache miss rate) * (0.125 + 0.05) * 64(block size)
還是只要考慮read的部分
0.02(data cache miss rate) * (0.125) * 64(block size)

(2)
write through的情況下 data write的部分是不是  
Hit = 0.98 * 4(write to cache 1 word=4 byte) * 0.05
Miss = 0.02 * [64 + 4](memory 搬一個 block 到 cache 64 + cpu同時寫到cache和memory 4) * 0.05

5.4.5
write back的情況下 data write的部分是不是 
Hit = 0
Miss = 0.02 * [64 + 64 * 0.3] * 0.05 ( memory 搬一個 block 到 cache 64  + cpu把那個block的30%dirty bit 修改)


還有一題 5.1.4 = 8*8000 / 4(A) + 8 / 4(B) 這樣嗎?



Single  Cycle
CPI = 1
clock cycle rate = long
Multi
CPI = small

