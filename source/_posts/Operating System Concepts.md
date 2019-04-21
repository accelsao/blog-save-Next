---
title: Operating System Concepts
tags:
  - Operating System
  - Process
categories:
  - Learning Notes
mathjax: false
date: 2019-04-20 15:16:05
---

操作系統龍書筆記

<!--more-->

# Process
## Zombie process
當一個子進程結束後, 但他仍然在進程表中. 一般這需要父進程來讀取` exit `狀態, 要是一直沒有將殭屍進程釋放(reaped), 就會導致資源耗竭
所以其實每個 children process 都會變成 zombie process
## Orphan process
這是父進程結束但子進程卻還在跑的進程, 這些會被`init`(pid=1)收養

