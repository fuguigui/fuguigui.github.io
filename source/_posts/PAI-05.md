---
title: Reasoning over Time
date: 2020-01-28
tags: [AI, Reinforcement Learning, Bayesian, Markov]
categories: [Learning Notes]
mathjax: true
---

Before, we talk about static models, which means variables don't change possible values. For temporal models, in which variables' value states can change over time, approaches are generated from static model:

The basic idea is to create "copies" of variables, one per time step.

??? How should we model dependence over time? That is the dependence between variables.

A very natural kind of model is Markov chain. 

Markov chain can have orders: k-order means the current event depends only on the previous k events. k-order Markov chain can be easily reduced into 1-order Markov chain by creating k-length vectors.



# Inference Tasks

Settings:

- $X_1,X_2,\cdots,X_T$: unobserved or hidden variables, called states change over time.
- $Y_1, Y_2,\cdots,Y_T$: observations

Tasks:

- Filtering: given the observations till now, what is the current state? $P(X_t|y_{1:t})$
- Prediction: given the observations, predict the ne$X_t$ state: $P(X_{t+\delta}|y_{1:t})$​
- Smoothing: what is the probability of some event happened before given what are known now.$P(X_t|y_{1:T})$ for $1\leq t\leq T$
- MPE: $\arg \max_{X_{1:T}} P(X_{1:T}|y_{1:T})$. Observing something, what is the most likely path?

These tasks can be solved by previously learned methods, e.g. variable elimination/belief propagation. But one problem is if we want to calculate for $t$, we need to start from the beginning. The complexity grows with time. Therefore, we want some model much efficient. 



# State space models

In general, a family of models that behave some Markovian process but not directly observed. We only observe some noisy estimates. Some examples: Hidden Markov model, Kalman Filter.

Particularly, we assume:

- Markov property holds over time: the current states only depends on the previous one, not the much older ones.

- conditional independence: $y_t$ only depends on $x_t$, not the older $x_s$. 

- stationary: the transition probability 

  1. $X_i$ to $X_{i+1}$ not dependent on $i$, and 

  2. $X_t$​ to $Y_t$​, not dependent on $t$

     is the same over the time. 

discrete HMM: $X_i$​ categorical, $Y_i$ can be an$Y_t$hing

continuous Kalman Filters: $X_i, Y_i$: continuous, follow Gaussian distributions

## HMM

### Inference

#### Filtering

Answer the question that how to efficiently update our belief of states given the previous states. Formally,

Given $P(X_t|y_{1\cdots t-1})$ and $y_t$, compute $P(X_t|y{1\cdots t})$

$$
\Pr[X_t|y_{1:t}]=\frac{1}{Z}\Pr[X_t|y_{1:t-1}]\Pr[y_t|X_t,y_{1:t-1}] =\frac{1}{Z}\Pr[X_t|y_{1:t-1}]\Pr[y_t|X_t]
$$


with $ Z=\sum_x\Pr[X_t=x|y_{1:t-1}]\Pr[y_t|X_t=x]$

#### Smoothing

Run sum product to calculate the interested marginals.

#### Prediction

$$
\Pr[X_{t+1}|y_{1:t}]=\sum_x\Pr[X_t=x,X_{t+1}|y_{1:t}]=\sum_x \Pr[X_t=x|y_{1:t}]\Pr[X_{t+1}|X_t=x,y_{1:t}]
$$

$$
=\sum_x\Pr[X_t=x|y_{1:t}]\Pr[X_{t+1}|X_t=x]
$$

The computation complexity of condition and prediction doesn't grow with $t$. Reuse what are computed before.

## Kalman Filter

Employing Gaussian properties, encode two stuffs:

- transition between $X_{t+1}$ and $X_t$​ , Motion model, linear
- transition between $Y_t$ and $X_t$​ , Sensor model, linear

### Gaussian

Properties: 

- marginalization: for Gaussian distribution, marginalization is nothing but indexing/selection.
- conditioning: conditions of Gaussian's are still Gaussian. The new mean is the original mean, linearly corrected by observations. The new variance is also the original variance, corrected by the covariance and the variance of observations. It shrinks. *Variance can only decrease*.
- multiplication: a Gaussian random vector multiplied by a constant matrix is still Gaussian.
- summation: sums of Gaussian are Gaussian. New mean: the sum of means. New variance: the sum of variances.

### Inference

#### Filtering

How to do filtering fast? 

Similar to HMM, because we utilize the Markov chain structure, which is not changed, change the summation to integral.

#### Smoothing
#### Prediction
#### MPE

# Dynamic Bayesian Networks

????

Generalized from state space models, now there are more than one variable in each time step. Also, one variable in the current time can be influenced by several variables previously.

## Particle filtering



## Assumed density filtering

The key idea is similar to variational inference, that is projecting marginals to simple ones.