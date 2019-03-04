---
title: 不存在的人 - StyleGAN
tags:
  - null
categories:
  - null
mathjax: true
date: 2019-02-18 16:06:37
---

這網站會生出StyleGAN的人臉圖 效果奇佳
[thispersondoesnotexist.com](https://thispersondoesnotexist.com/)

<!--more-->

StyleGAN 和一般 GAN 的差別示意圖
![](https://i.imgur.com/pM3fvwD.png)

其中的AdaIn定義為

$$
AdaIn(x_i,y) = y_{s,i}\frac{x_i - \mu(x_i)}{\sigma{(x_i)}} + y_{b,i}
$$

style y 是兩倍的dimension 一個是scales,另一個是bias

先對整張臉normalzie之後,在用其中一種style做affine
如此一來下一個convnet只會有一種style
直到下一個AdaIn

對於noise的方法 傳統的做法常常not work, StyleGAN 在每個Convnet後面都會add上noise 他造成的變化不會影響到整個global的範圍,pose,人臉基本不會變化
![](https://i.imgur.com/DB7jT3f.png)
因為整張圖用同個值scale and bias,所以全局的特徵,pose,lighting,background的style就會有相關
而noise對每個pixel都是獨立的,如果你要用noise改變pose的話 整張圖會失真 discriminator的penalty會增加來控制

我們對 disentanglement的定義大部分是 可以由linear subspaces組成的 latent spacce 每個subspace控制其中一種變數
就像是人臉用髮色,臉型,膚色等等特徵組成的感覺
![](https://i.imgur.com/2vkxb52.png)

所以StyleGAN的優點在於 有別於傳統只給予固定的分布 而是**可學習**的 連續分段映射
就像上圖可以學習 Unwrap 回去 W


# Disentangleent studies

下面介紹兩種quantifying disentanglement的方法

## Perceptual path length

interpolation of latent-space 可能會出現非線性的變化
特徵沒有在端點出現的可能會在中間出現,這樣代表entangled, 變異的因數就沒有恰當的分離
我們用一種測量方法來看不同的interpolation會有怎樣的變化
直觀上來說 越是不彎曲的latent-space 會更perceptually smoother

我們用perceptually-based pairwise image distance當作權重差在兩個VGG16的embedding之間
這裡對latent-space Z 用slerp(spherical interpolation operation) 他是對於normalized input latent space 最適合的
為了focus在臉部特徵而不是背景 我們裁減臉部部分 因為正常來說d會是二次式 因此我們乘上 \\(\frac{1}{\epsilon ^2}\\)

而至於latent-space W 我們就用線性的interpolation 因為他沒有normalized

從圖表中可以看出包含input noise 的stylegan full-path length比較短
![](https://i.imgur.com/5WDSDNf.png)

 
