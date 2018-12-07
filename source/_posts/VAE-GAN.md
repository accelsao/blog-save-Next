---
title: VAE-GAN
tags:
  - GAN
  - VAE
categories:
  - GAN
mathjax: true
date: 2018-12-02 10:32:27
---



[Autoencoding beyond pixels using a learned similarity metric](https://arxiv.org/pdf/1512.09300v2.pdf)
-- Learning Notes

[vae-gan-tensorflow](https://github.com/zhangqianhui/vae-gan-tensorflow)

<!--more-->


# Autoencoding with learned similarity
## VAE
A VAE consists of two network that encode data sample x to latent sample z, and decode back again.
$$
\begin{equation} \label{eq1}
z \sim Enc(z)=q(z|x),\tilde x \sim Dec(z)=p(x|z)
\end{equation}
$$



VAE loss is minus the sum of the expected log likehood(the reconstruction error) and a prior regularization term

$$
\begin{equation} \label{eq2}
L_{VAE}=-E_{q(z|x)}[log\frac{p(x|z)p(z)}{q(z|x)}]=L_{like}^{pixel}+L_{prior}
\end{equation}
$$
with

$$
\begin{equation} \label{eq3}
L_{like}^{pixel}=-E_{q(z|x)}[log\ p(x|z)]
\end{equation}
$$

$$
\begin{equation} \label{eq4}
L_{prior}=D_{KL}(q(z|x)||p(z))
\end{equation}
$$
## GAN
$$
\begin{equation} \label{eq5}
L_{GAN} = log(Dis(x)) + log(1 − Dis(Gen(z)))
\end{equation}
$$
We aim to maximize the binary cross entropy

## Beyond element-wise reconstruction error with VAE/GAN
We combine the advantage of GAN as a generative model and VAE as the method that produces an encoder of data into the latent space z.
Since, element-wise reconstruction errors are not adequate for images and other signals with invariances,
we propose replacing Eq3.
Let Dis_l(x) denote the lth layer of discriminator
We introduce Gaussian observation model for Dis_l(x) with mean \\(Dis_l(\tilde x)\\) and identity covariance:
$$
\begin{equation} \label{eq6}
p(Dis_l(x)|z)=N(Dis_l(x)|Dis_l(\tilde x),I)
\end{equation}
$$

where \\(\tilde x ~ Dec(z)\\) is the sample from the decoder of x
replace Eq3 with
$$
\begin{equation} \label{eq7}
L_{like}^{Dis_l} = -E_{q(z|x)}[log\ p(Dis_l(x)|z)]
\end{equation}
$$
$$
\begin{equation} \label{eq8}
L=L_{prior}+L_{like}^{Dis_l}+L_{GAN}
\end{equation}
$$


### Weighting VAE vs. GAN
we use \\(\gamma \\) to weight the ability to reconstruct vs fooling the discriminator,
or can interpreted as weighting style and content
$$
\begin{equation} \label{eq9}
\theta _{Dec} \stackrel{+}\leftarrow -\nabla_{\theta_{Dec}(\gamma L_{like}^{Dis_l} - L_{Gan})}
\end{equation}
$$
### Discriminating based on samples from p(z) and \\(q(z|x)\\)
We obtain better result using samples from \\(q(z|x)\\)
in addition to our prior p(z) in the GAN Loss
$$
\begin{equation} \label{eq10}
L_{GAN} = log(Dis(x)) + log(1 − Dis(Dec(z)))+log(1-Dis(Dec(Enc(x))))
\end{equation}
$$

![](https://i.imgur.com/ofa4Ddy.png)
![](https://i.imgur.com/4aSGcCK.png)

# Experiment
## Training stability
[Improved Techniques for Training GANs](https://arxiv.org/pdf/1606.03498.pdf)


