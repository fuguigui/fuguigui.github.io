---
title: Reasoning over Time
date: 2020-01-28
tags: [probabilistic artificial intelligence, reinforcement learning]
categories: course notes
---

Before, we talk about static models, which means variables don't change possible values. For temporal models, in which variables' value states can change over time, approaches are generated from static model:

The basic idea is to create "copies" of variables, one per time step.

??? How should we model dependence over time? That is the dependence between variables.

A very natural kind of model is Markov chain. 

Markov chain can have orders: k-order means the current event depends only on the previous k events. k-order Markov chain can be easily reduced into 1-order Markov chain by creating k-length vectors.



# Inference Tasks

Settings:

- X1,X2,...,XT: unobserved (hidden) variables (called states) change over time.
- Y1, Y2,...,YT: observations

Tasks:

- Filtering: given the observations till now, what is the current state? P(Xt|y1:t)
- Prediction: given the observations, predict the next state: P(Xt+delta|y1:t)
- Smoothing: what is the probability of some event happened before given what are known now.P(Xt|y1:T) for 1<=t<=T
- MPE: arg max_{X1:T} P(X1:T|y1:T). Observing something, what is the most likely path?

These tasks can be solved by previously learned methods, e.g. variable elimination/belief propagation. But one problem is if we want to calculate for t, we need to start from the beginning. The complexity grows with time. Therefore, we want some model much efficient. 



# State space models

In general, a family of models that behave some Markovian process but not directly observed. We only observe some noisy estimates. Some examples: Hidden Markov model, Kalman Filter.

Particularly, we assume:

- Markov property holds over time: the current states only depends on the previous one, not the much older ones.
- conditional independence: yt only depends on xt, not the older xs. 
- stationary: the transition probability ( (1)Xi to Xi+1 not dependent on i, and (2) Xt to Yt, not dependent on t) is the same over the time. 

HMM(discrete): Xi categorical, Yi can be anything

Kalman Filters(continuous): Xi, Yi: continuous, follow Gaussian distributions

## HMM

### Inference

#### Filtering

Answer the question that how to update (efficiently) our belief of states given the previous states. Formally,

Given P(Xt|y1...t-1) and y_t, compute P(Xt|y1...t)

![condtion](http://latex.codecogs.com/svg.latex?%5CPr%5BX_t%7Cy_%7B1%3At%7D%5D%3D%5Cfrac%7B1%7D%7BZ%7D%5CPr%5BX_t%7Cy_%7B1%3At-1%7D%5D%5CPr%5By_t%7CX_t%2Cy_%7B1%3At-1%7D%5D%20%3D%5Cfrac%7B1%7D%7BZ%7D%5CPr%5BX_t%7Cy_%7B1%3At-1%7D%5D%5CPr%5By_t%7CX_t%5D)

with ![condition2](http://latex.codecogs.com/svg.latex?Z%3D%5Csum_x%5CPr%5BX_t%3Dx%7Cy_%7B1%3At-1%7D%5D%5CPr%5By_t%7CX_t%3Dx%5D)

#### Smoothing

Run sum product to calculate the interested marginals.

#### Prediction

![pred](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7Bt%2B1%7D%7Cy_%7B1%3At%7D%5D%3D%5Csum_x%5CPr%5BX_t%3Dx%2CX_%7Bt%2B1%7D%7Cy_%7B1%3At%7D%5D%3D%5Csum_x%20%5CPr%5BX_t%3Dx%7Cy_%7B1%3At%7D%5D%5CPr%5BX_%7Bt%2B1%7D%7CX_t%3Dx%2Cy_%7B1%3At%7D%5D%3D%5Csum_x%5CPr%5BX_t%3Dx%7Cy_%7B1%3At%7D%5D%5CPr%5BX_%7Bt%2B1%7D%7CX_t%3Dx%5D)

The computation complexity of condition and prediction doesn't grow with t. Reuse what are computed before.

## Kalman Filter

Employing Gaussian properties, encode two stuffs:

- transition between Xt+1 and Xt (Motion model, linear)
- transition between Yt and Xt (Sensor model, linear)

### Gaussian

Properties: 

- marginalization: for Gaussian distribution, marginalization is nothing but indexing/selection.
- conditioning: conditions of Gaussian's are still Gaussian. The new mean is the original mean, linearly corrected by observations. The new variance is also the original variance, corrected by the covariance and the variance of observations. It shrinks. *Variance can only decrease*.
- multiplication: a Gaussian random vector multiplied by a constant matrix is still Gaussian.
- summation: sums of Gaussian are Gaussian. New mean: the sum of means. New variance: the sum of variances.

### Inference

#### Filtering

How to do filtering fast? 

Similar to HMM (because we utilize the Markov chain structure, which is not changed), change the summation to integral.

#### Smoothing
#### Prediction
#### MPE

# Dynamic Bayesian Networks

????

Generalized from state space models, now there are more than one variable in each time step. Also, one variable in the current time can be influenced by several variables previously.

## Particle filtering



## Assumed density filtering

The key idea is similar to variational inference, that is projecting marginals to simple ones.