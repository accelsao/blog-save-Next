---
title: Linear classification - Support Vector Machine and Softmax
tags:
  - Loss function
  - Softmax classifier
  - Support vector machine
  - CS231n
  - Linear Classification
categories:
- CS231n Convolutional Neural Networks for Visual Recognition Course Notes
mathjax: true
date: 2018-09-23 17:57:11
---

**Linear Classification**

The approach will have two major components:
**score function**:maps the raw data to class scores
**loss function**:quantifies the agreement between the predicted scores and the ground truth labels


<!--more-->

# Linear score function

First define score function that maps the pixel values of an image to confidence scores for each class
Ex:
$$ f(x_i, W, b) =  W x_i + b $$
assune that image \\(x_i\\) has all of its pixels flattened out to a single column vector of shape [D x 1]
The matrix **W** (of size [K x D]), and the vector **b** (of size [K x 1]) are the parameters of the function
**W -> weights, b -> bias**

# Loss function

We can control **W** and we want to set them so that the predicted class scores are consistent with the ground truth labels in the training data.
Then , we can know whether we are doing good job or not

## Multiclass Support Vector Machine(SVM) loss

SVM “wants” the correct class for each image to a have a score higher than the incorrect classes by some fixed margin Δ

Define the score for the j-th class : \\(s_j = f(x_i, W)_j \\)

SVM loss for the i-th examples :
$$L_i = \sum_{j\neq y_i} \max(0, s_j - s_{y_i} + \Delta)$$


Ex: **s = [13,−7,11]**

\\(L_i = \max(0, -7 - 13 + 10) + \max(0, 11 - 13 + 10)\\)

We get zero loss for this pair because the correct class score (13) was greater than the incorrect class score (-7) by at least the margin 10. Though the dif = -7-13 = -20 , but we only care about the dif at least 10.

Note that $$f(x_i; W) = Wx_i$$
Rewrite it to : $$L_i = \sum_{j\neq y_i} \max(0, w_j^T x_i - w_{y_i}^T x_i + \Delta)$$



### Regularization
The bug above is that W is not necessary unique.
Ex: If W correctly classify, then the loss is 0. Then \\(\lambda W\\) where \\(\lambda >1\\) will also give zero loss 

$$R(W) = \sum_k\sum_l W_{k,l}^2$$

$$ L = \underbrace{ \frac{1}{N} \sum_i L_i }_\text{data loss} + \underbrace{ \lambda R(W) }_\text{regularization loss} \\\\ $$

or
$$ L = \frac{1}{N} \sum_i \sum_{j\neq y_i} \left[ \max(0, f(x_i; W)_j - f(x_i; W)_{y_i} + \Delta) \right] + \lambda \sum_k\sum_l W_{k,l}^2 $$

and it improve generalization.

### Code

```py
import numpy as np
from random import shuffle

def svm_loss_naive(W, X, y, reg):
    """
    Structured SVM loss function, naive implementation (with loops).

    Inputs have dimension D, there are C classes, and we operate on minibatches
    of N examples.

    Inputs:
    - W: A numpy array of shape (D, C) containing weights.
    - X: A numpy array of shape (N, D) containing a minibatch of data.
    - y: A numpy array of shape (N,) containing training labels; y[i] = c means
    that X[i] has label c, where 0 <= c < C.
    - reg: (float) regularization strength

    Returns a tuple of:
    - loss as single float
    - gradient with respect to weights W; an array of same shape as W
    """
    dW = np.zeros(W.shape) # initialize the gradient as zero

    # compute the loss and the gradient
    num_classes = W.shape[1]
    num_train = X.shape[0]
    loss = 0.0
    for i in range(num_train):
        scores = X[i].dot(W)
        correct_class_score = scores[y[i]]
        for j in range(num_classes):
            if j == y[i]:
                continue
            margin = scores[j] - correct_class_score + 1 # note delta = 1
            if margin > 0:
                loss += margin

        # Right now the loss is a sum over all training examples, but we want it
        # to be an average instead so we divide by num_train.
        loss /= num_train

        # Add regularization to the loss.
        loss += reg * np.sum(W * W)

    #############################################################################
    # TODO:                                                                     #
    # Compute the gradient of the loss function and store it dW.                #
    # Rather that first computing the loss and then computing the derivative,   #
    # it may be simpler to compute the derivative at the same time that the     #
    # loss is being computed. As a result you may need to modify some of the    #
    # code above to compute the gradient.                                       #
    #############################################################################
    dW=dW/num_train+2*reg*W

    return loss, dW


def svm_loss_vectorized(W, X, y, reg):
    """
    Structured SVM loss function, vectorized implementation.

    Inputs and outputs are the same as svm_loss_naive.
    """
    loss = 0.0
    dW = np.zeros(W.shape) # initialize the gradient as zero

    #############################################################################
    # TODO:                                                                     #
    # Implement a vectorized version of the structured SVM loss, storing the    #
    # result in loss.                                                           #
    #############################################################################
    # s: A numpy array of shape (N, C) containing scores

    
    num_train=X.shape[0]
    scores=np.dot(X,W)

    # scores.T shape= C,N , y shape= N,
    # np choose (N,) = (N,:)
    correct_class_scores = np.choose(y, scores.T) 

    mask = np.ones(scores.shape, dtype=bool)
    mask[range(num_train), y] = False
    scores_ = scores[mask].reshape(num_train, -1)

    margin = scores_ - correct_class_scores[..., np.newaxis] + 1

    # discard margin < 0
    margin[margin < 0] = 0

    # Average our data loss over the size of batch and add reg. term to the loss.
    loss = np.sum(margin) / num_train
    loss += reg * np.sum(W * W)
  
    #############################################################################
    #                             END OF YOUR CODE                              #
    #############################################################################


    #############################################################################
    # TODO:                                                                     #
    # Implement a vectorized version of the gradient for the structured SVM     #
    # loss, storing the result in dW.                                           #
    #                                                                           #
    # Hint: Instead of computing the gradient from scratch, it may be easier    #
    # to reuse some of the intermediate values that you used to compute the     #
    # loss.                                                                     #
    #############################################################################
    original_margin = scores - correct_class_scores[...,np.newaxis] + 1

    # Mask to identiy where the margin is greater than 0 (all we care about for gradient).
    pos_margin_mask = (original_margin > 0).astype(float)

    # Count how many times >0 for each image but dont count correct class hence -1
    sum_margin = pos_margin_mask.sum(1) - 1

    # Make the correct class margin be negative total of how many > 0
    pos_margin_mask[range(pos_margin_mask.shape[0]), y] = -sum_margin

    # Now calculate our gradient.
    dW = np.dot(X.T, pos_margin_mask)

    # Average over batch and add regularisation derivative.
    dW = dW / num_train + 2 * reg * W
	#############################################################################
	#                             END OF YOUR CODE                              #
	#############################################################################

    return loss, dW

```

## Softmax classifier

\\( f(x_i; W) =  W x_i \\) stays unchanged
but L funcion replace with **cross-entropy loss**:

$$ L_i = -\log\left(\frac{e^{f_{y_i}}}{ \sum_j e^{f_j} }\right) \hspace{0.5in} \text{or equivalently} \hspace{0.5in} L_i = -f_{y_i} + \log\sum_j e^{f_j} $$

\\(f_j(z) = \frac{e^{z_j}}{\sum_k e^{z_k}}\\) is called **softmax function**

**Information theory view** The cross-entropy between a “true” distribution p and an estimated distribution q is defined as:
$$ H(p,q) = - \sum_x p(x) \log q(x) $$

\\(q = e^{f_{y_i}}  / \sum_j e^{f_j}\\) 

the cross-entropy can be written in terms of entropy and KL divergence \\(H(p,q) = H(p) + D_{KL}(p||q)\\), and the entropy of the delta function p is zero,this is also equivalent to minimizing the KL divergence between the two distributions


**Practical issues: Numeric stability**
$$
\frac{e^{f_{y_i}}}{\sum_j e^{f_j}}
= \frac{Ce^{f_{y_i}}}{C\sum_j e^{f_j}}
= \frac{e^{f_{y_i} + \log C}}{\sum_j e^{f_j + \log C}}
$$
We are free to choose the value of C .This will not change any of the results, but we can use this value to improve the numerical stability of the computation. A common choice for C is to set \\(\log C = -\max_j f_j\\)

```py
f = np.array([123, 456, 789]) # example with 3 classes and each having large scores
p = np.exp(f) / np.sum(np.exp(f)) # Bad: Numeric problem, potential blowup

# instead: first shift the values of f so that the highest number is 0:
f -= np.max(f) # f becomes [-666, -333, 0]
p = np.exp(f) / np.sum(np.exp(f)) # safe to do, gives the correct answer

```
## SVM vs. Softmax

Softmax classifier provides “probabilities” for each class
EX:
$$[1, -2, 0] \rightarrow [e^1, e^{-2}, e^0] = [2.71, 0.14, 1] \rightarrow [0.7, 0.04, 0.26]$$
Where the steps taken are to exponentiate and normalize to sum to one

In practice, SVM and Softmax are usually comparable

![](http://cs231n.github.io/assets/svmvssoftmax.png)


The SVM does not care about the details of the individual scores: if they were instead [10, -100, -100] or [10, 9, 9] the SVM would be indifferent since the margin of 1 is satisfied and hence the loss is zero.
However, these scenarios are not equivalent to a Softmax classifier, which would accumulate a much higher loss for the scores [10, 9, 9] than for [10, -100, -100]


the Softmax classifier is never fully happy with the scores it produces: the correct class could always have a higher probability and the incorrect classes always a lower probability and the loss would always get better. However, the SVM is happy once the margins are satisfied and it does not micromanage the exact scores beyond this constraint. 


### Code
```py
def svm_loss(x, y):
    """
    Computes the loss and gradient using for multiclass SVM classification.

    Inputs:
    - x: Input data, of shape (N, C) where x[i, j] is the score for the jth
      class for the ith input.
    - y: Vector of labels, of shape (N,) where y[i] is the label for x[i] and
      0 <= y[i] < C

    Returns a tuple of:
    - loss: Scalar giving the loss
    - dx: Gradient of the loss with respect to x
    """
    N = x.shape[0]
    correct_class_scores = x[np.arange(N), y]
    # or correct_class_scores=np.choose(y,x.T)
    margins = np.maximum(0, x - correct_class_scores[:, np.newaxis] + 1.0)
    # delta=1.0
    # only consider wrong class margin
    margins[np.arange(N), y] = 0
    loss = np.sum(margins) / N
    num_pos = np.sum(margins > 0, axis=1)
    dx = np.zeros_like(x)
    dx[margins > 0] = 1
    dx[np.arange(N), y] -= num_pos
    dx /= N
    return loss, dx
```
```py
def softmax_loss(x, y):
    """
    Computes the loss and gradient for softmax classification.

    Inputs:
    - x: Input data, of shape (N, C) where x[i, j] is the score for the jth
      class for the ith input.
    - y: Vector of labels, of shape (N,) where y[i] is the label for x[i] and
      0 <= y[i] < C

    Returns a tuple of:
    - loss: Scalar giving the loss
    - dx: Gradient of the loss with respect to x
    """
    shifted_logits = x - np.max(x, axis=1, keepdims=True)
    Z = np.sum(np.exp(shifted_logits), axis=1, keepdims=True)
    log_probs = shifted_logits - np.log(Z)
    probs = np.exp(log_probs)
    N = x.shape[0]
    loss = -np.sum(log_probs[np.arange(N), y]) / N
    dx = probs.copy()
    dx[np.arange(N), y] -= 1
    dx /= N
    return loss, dx

```


# Summary
* We defined a score function from image pixels to class scores(Weights **W** and biases **b**)
* Unlike kNN classifier, the advantage of this parametric approach is that once we learn the parameters we can discard the training data. Additionally, the prediction for a new test image is fast since it requires a single matrix multiplication with W, not an exhaustive comparison to every single training example.
* bias trick
* loss function

# Reference

[cs231n notes](http://cs231n.github.io/linear-classify/#softmax)