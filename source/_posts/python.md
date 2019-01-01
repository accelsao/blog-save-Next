---
title: Python小東西
tags:
  - Python
categories:
  - Programming Language
mathjax: false
date: 2018-10-08 10:04:16
---

放一些Python小程式

<!--more-->

# python_youtube_download
`pip install pytube`
```py
from pytube import YouTube
YouTube('https://www.youtube.com/watch?v=GQ3V50XoLOM').streams.first().download('../owner')
# YouTube('video_url').streams.first().download('save_path')
```


