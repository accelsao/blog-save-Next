---
title: An Overview of Gradient descent optimization algorithms
tags:
  - CS231n
  - Gradient descent
categories:
  - Gradient descent
date: 2018-07-04 19:40:24
mathjax: true
---

Gradient descent is one of the most popular algorithms to perform optimization and by far the most common way to optimize neural networks
We are first going to look at the different variants of gradient descent


Gradient descent is a way to minimize an objective function \\(J(\theta)\\) parameterized by a model's parameters \\(\theta \in \mathbb{R}^d\\) 
by updating the parameters in the opposite direction of the gradient of the objective function \\(\nabla_\theta J(\theta)\\) with respect to the parameters


<!--more-->

# Gradient descent variants

There are three variants of gradient descent,which differ in how much data we use to compute the gradient of the objective function


## Batch gradient descent

Vanilla gradient descent or  batch gradient descent, computes the gradient of the cost function w.r.t. to the parameters θ for the entire training dataset:

$$\theta = \theta - \eta \cdot \nabla_\theta J( \theta)$$

As we need to calculate the gradients for the whole dataset to perform just one update, batch gradient descent can be very slow and is intractable for datasets that don't fit in memory

```py
for i in range(nb_epochs):
  params_grad = evaluate_gradient(loss_function, data, params)
  params = params - learning_rate * params_grad
```

## Stochastic gradient descent

Stochastic gradient descent (SGD) in contrast performs a parameter update for each training example \\(x^{(i)}\\) and label \\(y^{(i)}\\)
$$ \theta = \theta - \eta \cdot \nabla_\theta J( \theta; x^{(i)}; y^{(i)}) $$

```py
for i in range(nb_epochs):
  np.random.shuffle(data)
  for example in data:
    params_grad = evaluate_gradient(loss_function, example, params)
    params = params - learning_rate * params_grad
```
## Mini-batch gradient descent
Mini-batch gradient descent finally takes the best of both worlds and performs an update for every mini-batch of n training examples:
$$ \theta = \theta - \eta \cdot \nabla_\theta J( \theta; x^{(i:i+n)}; y^{(i:i+n)}) $$

```py
for i in range(nb_epochs):
  np.random.shuffle(data)
  for batch in get_batches(data, batch_size=50):
    params_grad = evaluate_gradient(loss_function, batch, params)
    params = params - learning_rate * params_grad
```

## Challenges
* chossing proper learning rate is difficult
* Learning rate schedules try to adjust the learning rate during training by e.g. annealing, i.e. reducing the learning rate according to a pre-defined schedule or when the change in objective between epochs falls below a threshold. These schedules and thresholds, however, have to be defined in advance and are thus unable to adapt to a dataset's characteristics
* Another key challenge of minimizing highly non-convex error functions common for neural networks is avoiding getting trapped in their numerous suboptimal local minima. Dauphin argue that the difficulty arises in fact not from local minima but from saddle points, i.e. points where one dimension slopes up and another slopes down. These saddle points are usually surrounded by a plateau of the same error, which makes it notoriously hard for SGD to escape, as the gradient is close to zero in all dimensions.
* Additionally, the same learning rate applies to all parameter updates. If our data is sparse and our features have very different frequencies, we might not want to update all of them to the same extent, but perform a larger update for rarely occurring features.

# Gradient descent optimization algorithms

## Momentum

Momentum is  a method that helps accelerate SGD in the relevant direction and dampens oscillations
It does this by adding a fraction \\(\gamma \\)
$$
\begin{align}
\begin{split}
v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta) \\  
\theta &= \theta - v_t
\end{split}
\end{align}
$$
The momentum term \\(\gamma \\) is usually set to 0.9 or a similar value.
The momentum term increases for dimensions whose gradients point in the same directions and reduces updates for dimensions whose gradients change directions. As a result, we gain faster convergence and reduced oscillation.

Some implementations exchange the signs in the equations
```py
v = gamma * v - learning_rate * dtheta # integrate velocity
theta += v # integrate position
```

set **v** to 0
\\(\gamma \\) usaually set as [0.5,0.9,0.95,0.99]


## Nesterov accelerated gradient

Nesterov accelerated gradient (NAG) 
We know that we will use our momentum term \\(\gamma v_{t-1}\\) to move the parameters \\(\theta \\)

Computing \\(\theta -\gamma v_{t-1}\\) thus gives us an approximation of the next position of the parameters or where our parameters are going to be.


$$
\begin{align}
\begin{split}
v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta - \gamma v_{t-1} ) \\  
\theta &= \theta - v_t
\end{split}
\end{align}
$$

核心想法如下圖
<hr>
![](http://cs231n.github.io/assets/nn3/nesterov.jpeg)

`x`會通過`mu * v`來改變位置
那麼直接計算`x + mu * v`的梯度而不是`x`的
```py
x_ahead = x + mu * v
# 算 x_ahead 的梯度 dx_ahead
v = mu * v - learning_rate * dx_ahead
x +=v
```

```py
v_prev = v # Save v
v = mu * v - learning_rate * dx
```

另外一種
```
v_prev = v # back this up
v = mu * v - learning_rate * dx # velocity update stays the same
x += -mu * v_prev + (1 + mu) * v # position update changes form
```
來源公式推導
* [Advances in optimizing Recurrent Networks by Yoshua Bengio, Section 3.5](https://arxiv.org/pdf/1212.0901v2.pdf)
* [Ilya Sutskever's thesis (pdf) contains a longer exposition of the topic in section 7.2](http://www.cs.utoronto.ca/~ilya/pubs/ilya_sutskever_phd_thesis.pdf)



## Adagrad

Adagrad is an algorithm for gradient-based optimization that does just this: It adapts the learning rate to the parameters, performing smaller updates
for parameters associated with frequently occurring features, and larger updates for parameters associated with infrequent features.

Previously, we performed an update for all parameters \\(\theta\\) at once as every parameter  \\(\theta_i\\) used the same learning rate \\(\eta \\)
As Adagrad uses a different learning rate for every parameter \\(\theta_i\\) at every time step t, we first show Adagrad's per-parameter update, which we then vectorize.
For brevity, we use \\(g_t\\) to denote the gradient at time step t
$$ g_{t, i} = \nabla_\theta J( \theta_{t, i} ) $$

$$ \theta_{t+1, i} = \theta_{t, i} - \eta \cdot g_{t, i} $$

In its update rule, Adagrad modifies the general learning rate \\(\eta \\) at each time step t for every parameter \\(\theta_i\\) based on the past gradients that have been computed for \\(\theta_i\\):

$$\theta_{t+1, i} = \theta_{t, i} - \dfrac{\eta}{\sqrt{G_{t, ii} + \epsilon}} \cdot g_{t, i}$$


\\(G_{t} \in \mathbb{R}^{d \times d}\\) here is a diagonal matrix where each diagonal element **i**,**i** is the sum of the squares of the gradients  w.r.t. \\(\theta_i\\) up to time step t,
while \\(\epsilon\\) is a smoothing term  that avoids division by 0

$$ \theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{G_{t} + \epsilon}} \odot g_{t} $$


```
# Assume the gradient dx and parameter vector x
cache += dx**2
x += - learning_rate * dx / (np.sqrt(cache) + eps)
```
`cache` size 和 梯度矩陣一樣
跟蹤每個參數的梯度平方和
收到高梯度時的更新效果減弱,
低梯度效果增強
eps 防止除0
缺點:過於激進且過早停止學習

One main benefit is that we only need to tune learning rate at the first time,and leave it alone

Adagrad's main weakness is its accumulation of the squared gradients in the denominator:
Since every added term is positive, the accumulated sum keeps growing during training.
This in turn causes the learning rate to shrink and eventually become infinitesimally small at which point the algorithm is no longer able to acquire additional knowledge.
The following algorithms aim to resolve this flaw.


## Adadelta

Reduce Adagrad aggressive,monotonically decreasing learning rate
Instead of accumulating all past squared gradients, Adadelta restricts the window of accumulated past gradients to some fixed size **w**.
Instead of inefficiently storing w previous squared gradients, the sum of gradients is recursively defined as a decaying average of all past squared gradients.
The running average \\(E[g^2]_t\\) at time step t then depends (as a fraction \\(\gamma\\) similarly to the Momentum term) only on the previous average and the current gradient:

$$ E[g^2]_t = \gamma E[g^2]_{t-1} + (1 - \gamma) g^2_t $$

We set \\(\gamma\\) to a similar value as the momentum term, around 0.9.
For clarity, we now rewrite our vanilla SGD update in terms of the parameter update vector \\(\Delta \theta_t\\)
$$
\begin{align}
\begin{split}
\Delta \theta_t &= - \eta \cdot g_{t, i} \\
\theta_{t+1} &= \theta_t + \Delta \theta_t \end{split}
\end{align}
$$

Previous form

$$ \Delta \theta_t = - \dfrac{\eta}{\sqrt{G_{t} + \epsilon}} \odot g_{t} $$

Replace
$$ \Delta \theta_t = - \dfrac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t} $$

As the denominator is just the root mean squared (RMS) error criterion of the gradient
$$ \Delta \theta_t = - \dfrac{\eta}{RMS[g]_{t}} g_t $$

The authors note that the units in this update (as well as in SGD, Momentum, or Adagrad) do not match, i.e. the update should have the same hypothetical units as the parameter. To realize this, they first define another exponentially decaying average, this time not of squared gradients but of squared parameter updates
$$E[\Delta \theta^2]_t = \gamma E[\Delta \theta^2]_{t-1} + (1 - \gamma) \Delta \theta^2_t$$

he root mean squared error of parameter updates is thus:
$$ RMS[\Delta \theta]_{t} = \sqrt{E[\Delta \theta^2]_t + \epsilon} $$

Since RMS[Δθ]t is unknown, we approximate it with the RMS of parameter updates until the previous time step. Replacing the learning rate η in the previous update rule with RMS[Δθ]t−1 finally yields the Adadelta update rule:

$$
\begin{align}
\begin{split}
\Delta \theta_t &= - \dfrac{RMS[\Delta \theta]_{t-1}}{RMS[g]_{t}} g_{t} \\
\theta_{t+1} &= \theta_t + \Delta \theta_t 
\end{split}
\end{align}
$$

With Adadelta, we do not even need to set a default learning rate, as it has been eliminated from the update rule

## RMSprop
Adagrad修改版
讓Adagrad不那麼激進,單調降低學習率
用梯度平方的平均
```
cache = decay_rate * cache + (1 - decay_rate) * dx**2
x += - learning_rate * dx / (np.sqrt(cache) + eps)
```
`decay_rate` 超參數 常用[0.9,0.99,0.999]
比起Adagrad更新不會單調降低

## Adam
類似RMSProp 動量版

```
# 簡化版
m = beta1*m + (1-beta1)*dx
v = beta2*v + (1-beta2)*(dx**2)
x += - learning_rate * m / (np.sqrt(v) + eps)
```
整體來說比 RMSprop 優
但也可以使用 SGD+Nesterov
論文建議值
`eps=1e-8` `beta1=0.9` `beta2=0.999`
完整版有`bias` 因為`m` `v`初始0

# Reference
[An overview of gradient descent optimization algorithms](http://ruder.io/optimizing-gradient-descent/index.html#nesterovacceleratedgradient)
[CS231n课程笔记翻译：神经网络笔记3（下）](https://zhuanlan.zhihu.com/p/21798784?refer=intelligentunit)
[First-order (SGD), momentum, Nesterov momentum](http://cs231n.github.io/neural-networks-3/#sgd)
