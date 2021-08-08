---
title: Learning
date: 2020-01-28
tags: [AI, Reinforcement Learning, Baysian]
categories: [Learning Notes]
mathjax: true
---

So far, we talk about how to use a given model to do some tasks, e.g. inference, planning. But where do these models come from? Now, we turn attention to learn models from training data.

# Learning BN from data

What to learn?

- the structure of BN
- the parameters: conditional probability distributions

Different case: 

- all variables are observed
- only part of variables can be observed.



1. first assume the structure is given, to learn the parameters
2. learn the structure as well

## Parameter learning

### MLE

A very basic estimation is MLE, maximum likelihood estimation.

$$
\theta^* = \arg_{\theta}\max\sum_{j=1}^n\sum_{i=1}^N\log\Pr[X_j^{(i)}|X_{Pa_j}^{(i)},\theta_{j|Pa_j}]
$$


We divide the whole parameters to individual sets on each variable. Global optimization can be composed of local optimization. Formally,

$$
\theta^*_{j|Pa_j} = \arg_{\theta}\max L_j(\theta_j|Pa_j)\Rightarrow\theta^*_{j|Pa_j}=\frac{Count(X_j,X_{Pa_j})}{Count(X_{Pa_j})}
$$
When $X_j$ follows Bernoulli distribution.

This strategy requires complete data, all variables are observable.

- If there are unobserved variables: EM-algorithm

- if the count is small, the estimation could be imprecise. In this case, we can use **pseudo-counts**: make prior assumptions about parameters to correct the estimated parameters.

  - for Bernoulli distribution, the prior is equivalent to make Beta prior over parameters. For instance, 
    $$
    \theta\sim Beta(\alpha_c,\alpha_l) = \frac{1}{B(\alpha_c,\alpha_l)}\theta^{\alpha_c-1}(1-\theta)^{\alpha_l-1}
    $$
    
  - Then observe $n_c$ and $n_l$ counts specifically,
    $$
    \Pr[\theta|n_c,n_l]=\frac{1}{Z}\frac{Beta(\alpha_c,\alpha_l)}\theta^{\alpha_c-1}(1-\theta)^{\alpha_l-1}\theta^{n_c}(1-\theta)^{n_l}\sim Beta(\alpha_c+n_c,\alpha_l+n_;)
    $$
    

### MAP

Another view is to treat the parameters as shared variables. They are prior. Given data, which can be treated as evidence, we want to find which values of these variables are most possible. Solve via MAP.

$$
\hat{\theta}\in\arg_{\theta}\max\Pr[\theta|f^{(i)},w^{(i)}]
$$
In this way, if we want to query the conditional probability given data, we can use the MAP estimation to do that. For instance, $F,W$ are variables, $D$ are the dataset.

$$
\Pr[F|W=w,D]\simeq\Pr[F|w,\hat{\theta}]
$$

### Bayesian learning

Instead of learning the unknown parameters, just marginalize them, sum them out.

$$
\Pr[F|W=w,D]=\int\Pr[F,\theta|w,D]\mathrm{d}\theta=\int \Pr[F|w,\theta]\Pr[\theta|w,D]\mathrm{d}\theta
$$


## Structure learning

### Scoring

How to score a graph structure?

Given a scoring strategy, how to find the "optimal" structure?

### MLE

Only mle will cause the children fully-connected to all of its parents $\to$Overfit

#### Regularization

- BIC
- special family of graphs

### MAP

The general idea is to condition on what we have, the dataset, and find the most probable explanation for the unknown variables, W.

Placing a prior distribution $P(W)$ on the parameters W

$$
\hat{w}=\arg_w\max\Pr[w|x_{1:n},y_{1:n}]=\arg_w\min-\log(\Pr[w])-\sum_{i=1}^N\log\Pr[y_i|x_i,w]
$$

- mini-batch SGD for MAP estimation to accelerate. The algorithm:

  1. initialize $w_0$

  2. For $t=0,1,2,\cdots$ 

     $$
      w_{t+1}=w_t-\frac{\epsilon_t}{2}(\nabla\log\Pr[w_t]+\frac{N}{k}\sum_{i=1}^k\nabla\log\Pr[y_{t,i}|x_{t,i},w_t])
     $$
     
  
  - $(x_{t,i},y_{t,i})$ is the $i$-th data point in the $t$-th mini-batch of $k$ points
  - learning rate satisfies $\sum_t\epsilon_t\to\infty,\sum_t\epsilon_t^2:\infty$

# Bayesian learning

Different from MAP, Bayesian learning turns attention to prediction. Skip the unknown parameters and use them directly. That means given a new data point $x'$, what is the probability of $Y(x')$? To perform this task, we sum out whatever we don't know:

$$
\Pr[Y|x',x_{1:N},y_{1:N}]=\int \Pr[w|x_{1:N},y_{1:N}]\Pr[Y|w,x']\mathrm{d}w
$$

- the first item on the right side: calculate how likely of a model given our data
- the second item: how likely is the prediction given the model and the new data point.
- this model not only considers the most likely weight, which MAP chooses, but also all the other weights.
- However, estimate the whole weight space may be undoable.

Several algorithms can make this task doable.

## Bayesian learning with Gaussian process

### Gaussian process

It is a normal distribution over functions

Pin on these functions finite points and these finite marginals are multivariate Gaussians.

Particularly, the prediction of one data point follows one-dimension Gaussian distribution. We need to learn the mu function and kernel function.

$$
\Pr[f(x)]\sim \mathcal{N}(f(x);\mu(x),\sigma^2(x))
$$


#### kernel(covariance) functions

  - symmetric: $k(x,x')=k(x',x)$ for all $x,x'$

  - positive semi-definite: for all A, Sigma_{AA} is positive semi-definite matrix $\Leftrightarrow$ all eigenvalues of 

    $$
    \Sigma_{AA}\geq 0\Leftrightarrow \forall x, x\sum_{AA}x\geq 0
    $$

  - Examples:

    - squared exponential kernel: $\mathcal{K}(x,x')=\exp(-||x-x'||_2^2/h^2))$ when $x=x'$, the covariance is 1. h smaller $\to$​ more punish on distance $\to$ more fluctuation

    - exponential kernel: $\mathcal{K}(x,x')=\exp(-||x-x'||_1/h^2)$    very unsmooth.

    - linear kernel, Bayesian linear regression: 

      $$
       \mathcal{K}(x,x')=x^Tx'
      $$
      

This kind of models gives us the ability to predict for the uncertainty. How much uncertain on some data points, estimate the variance.

## Bayesian deep learning

work on high-dimension data.

- SGLD: generate from mini-batch SGD, with a more random noise item:
  $$
  w_{t+1}=w_t-\frac{\epsilon_t}{2}(\nabla\log\Pr[w_t]+\frac{N}{k}\sum_{i=1}^k\nabla\log\Pr[y_{t,i}|x_{t,i},w_t])+\eta_t)
  $$
  where $\eta\sim\mathcal{N}(0,\epsilon^2)$
  
  To avoid being stuck on some local areas.


## Summary

Why Bayesian learning are so popular? Because it captures two kinds of uncertainty:

- aleatoric uncertainty: noise in observations given perfect knowledge of the model. For instance, flipping a coin, even we know the coin is fair, the result is still uncertain.
- epistemic uncertainty: uncertainty about parameters due to the lack of data. For instance, I don't know what kind of coin I have when flipping it.

