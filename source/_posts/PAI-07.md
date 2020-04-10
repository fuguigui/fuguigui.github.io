---
title: Learning
date: 2020-01-28
tags: [courses, Probability, AI]
categories: Probabilistic Artificial Intelligence
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

A very basic estimation is MLE (maximum likelihood estimation).

![mle](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%2A%20%3D%20%5Carg_%7B%5Ctheta%7D%5Cmax%5Csum_%7Bj%3D1%7D%5En%5Csum_%7Bi%3D1%7D%5EN%5Clog%5CPr%5BX_j%5E%7B%28i%29%7D%7CX_%7BPa_j%7D%5E%7B%28i%29%7D%2C%5Ctheta_%7Bj%7CPa_j%7D%5D)

We divide the whole parameters to individual sets on each variable. Global optimization can be composed of local optimization. Formally,

![ind-mle](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%2A_%7Bj%7CPa_j%7D%20%3D%20%5Carg_%7B%5Ctheta%7D%5Cmax%20L_j%28%5Ctheta_j%7CPa_j%29%5CRightarrow%5Ctheta%5E%2A_%7Bj%7CPa_j%7D%3D%5Cfrac%7BCount%28X_j%2CX_%7BPa_j%7D%29%7D%7BCount%28X_%7BPa_j%7D%29%7D)

When X_j follows Bernoulli distribution.

This strategy requires complete data (all variables are observable).

- If there are unobserved variables: EM-algorithm

- if the count is small, the estimation could be imprecise. In this case, we can use **pseudo-counts**: make prior assumptions about parameters to correct the estimated parameters.

  - for Bernoulli distribution, the prior is equivalent to make Beta prior over parameters. For instance, ![beta-prior](http://latex.codecogs.com/svg.latex?%5Ctheta%5Csim%20Beta%28%5Calpha_c%2C%5Calpha_l%29%20%3D%20%5Cfrac%7B1%7D%7BB%28%5Calpha_c%2C%5Calpha_l%29%7D%5Ctheta%5E%7B%5Calpha_c-1%7D%281-%5Ctheta%29%5E%7B%5Calpha_l-1%7D)

    Then observe nc and nl counts specifically, ![beta-post](http://latex.codecogs.com/svg.latex?%5CPr%5B%5Ctheta%7Cn_c%2Cn_l%5D%3D%5Cfrac%7B1%7D%7BZ%7D%5Cfrac%7BBeta%28%5Calpha_c%2C%5Calpha_l%29%7D%5Ctheta%5E%7B%5Calpha_c-1%7D%281-%5Ctheta%29%5E%7B%5Calpha_l-1%7D%5Ctheta%5E%7Bn_c%7D%281-%5Ctheta%29%5E%7Bn_l%7D%5Csim%20Beta%28%5Calpha_c%2Bn_c%2C%5Calpha_l%2Bn_%3B%29)

### MAP

Another view is to treat the parameters as shared variables. They are prior. Given data, which can be treated as evidence, we want to find which values of these variables are most possible. Solve via MAP.

![map](http://latex.codecogs.com/svg.latex?%5Chat%7B%5Ctheta%7D%5Cin%5Carg_%7B%5Ctheta%7D%5Cmax%5CPr%5B%5Ctheta%7Cf%5E%7B%28i%29%7D%2Cw%5E%7B%28i%29%7D%5D)

In this way, if we want to query the conditional probability given data, we can use the MAP estimation to do that. For instance, F,W are variables, D are the dataset.

![map-infer](http://latex.codecogs.com/svg.latex?%5CPr%5BF%7CW%3Dw%2CD%5D%5Csimeq%5CPr%5BF%7Cw%2C%5Chat%7B%5Ctheta%7D%5D)

### Bayesian learning

Instead of learning the unknown parameters, just marginalize them, sum them out.

![bay-learn](http://latex.codecogs.com/svg.latex?%5CPr%5BF%7CW%3Dw%2CD%5D%3D%5Cint%5CPr%5BF%2C%5Ctheta%7Cw%2CD%5Dd%5Ctheta%3D%5Cint%20%5CPr%5BF%7Cw%2C%5Ctheta%5D%5CPr%5B%5Ctheta%7Cw%2CD%5Dd%5Ctheta)



## Structure learning

### Scoring

How to score a graph structure?

Given a scoring strategy, how to find the "optimal" structure?

### MLE

Only mle will cause the children fully-connected to all of its parents.->Overfit

#### Regularization

- BIC: 
- special family of graphs

### MAP

The general idea is to condition on what we have, the dataset, and find the most probable explanation for the unknown variables, W.

Placing a prior distribution P(W) on the parameters W

![map](http://latex.codecogs.com/svg.latex?%5Chat%7Bw%7D%3D%5Carg_w%5Cmax%5CPr%5Bw%7Cx_%7B1%3An%7D%2Cy_%7B1%3An%7D%5D%3D%5Carg_w%5Cmin-%5Clog%28%5CPr%5Bw%5D%29-%5Csum_%7Bi%3D1%7D%5EN%5Clog%5CPr%5By_i%7Cx_i%2Cw%5D)



- mini-batch SGD for MAP estimation to accelerate. The algorithm:

  1. initialize w0

  2. For t=0,1,2,...

     ![weight](http://latex.codecogs.com/svg.latex?w_%7Bt%2B1%7D%3Dw_t-%5Cfrac%7B%5Cepsilon_t%7D%7B2%7D%28%5Cnabla%5Clog%5CPr%5Bw_t%5D%2B%5Cfrac%7BN%7D%7Bk%7D%5Csum_%7Bi%3D1%7D%5Ek%5Cnabla%5Clog%5CPr%5By_%7Bt%2Ci%7D%7Cx_%7Bt%2Ci%7D%2Cw_t%5D%29)

  - (x_{t,i},y_{t,i}) is the i-th data point in the t-th mini-batch of k points
  - learning rate satisfies ![learningrate](http://latex.codecogs.com/svg.latex?%5Csum_t%5Cepsilon_t%5Cto%5Cinfty%2C%5Csum_t%5Cepsilon_t%5E2%3C%5Cinfty)

# Bayesian learning

Different from MAP, Bayesian learning turns attention to prediction. Skip the unknown parameters and use them directly. That means given a new data point x', what is the probability of Y(x')? To perform this task, we sum out whatever we don't know:

![blearning](http://latex.codecogs.com/svg.latex?%5CPr%5BY%7Cx%27%2Cx_%7B1%3AN%7D%2Cy_%7B1%3AN%7D%5D%3D%5Cint%20%5CPr%5Bw%7Cx_%7B1%3AN%7D%2Cy_%7B1%3AN%7D%5D%5CPr%5BY%7Cw%2Cx%27%5Ddw)

- the first item on the right side: calculate how likely of a model given our data
- the second item: how likely is the prediction given the model and the new data point.
- this model not only considers the most likely weight (which MAP chooses), but also all the other weights.
- However, estimate the whole weight space may be undoable.

Several algorithms can make this task doable.

## Bayesian learning with Gaussian process

### Gaussian process

It is a normal distribution over functions

Pin on these functions finite points and these finite marginals are multivariate Gaussians.

Particularly, the prediction of one data point follows one-dimension Gaussian distribution. We need to learn the mu function and kernel function.

![gp](http://latex.codecogs.com/svg.latex?%5CPr%5Bf%28x%29%5D%5Csim%20%5Cmathcal%7BN%7D%28f%28x%29%3B%5Cmu%28x%29%2C%5Csigma%5E2%28x%29%29)

#### kernel(covariance) functions

  - symmetric: k(x,x')=k(x',x) for all x,x'

  - positive semi-definite: for all A, Sigma_{AA} is positive semi-definite matrix <=> all eigenvalues of 

    ![learningrate](http://latex.codecogs.com/svg.latex?%5CSigma_%7BAA%7D%5Cgeq%200%5CLeftrightarrow%20%5Cforall%20x%2C%20x%5CSimga_%7BAA%7Dx%5Cgeq%200)

  - Examples:

    - squared exponential kernel: 

      ![expkernel](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BK%7D%28x%2Cx%27%29%3D%5Cexp%28-%7C%7Cx-x%27%7C%7C_2%5E2%2Fh%5E2%29) when x=x', the covariance is 1. h smaller-> more punish on distance->more fluctuation

    - exponential kernel: ![expkernel](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BK%7D%28x%2Cx%27%29%3D%5Cexp%28-%7C%7Cx-x%27%7C%7C_1%2Fh%5E2%29) very unsmooth.

    - linear kernel (Bayesian linear regression): 

      ![expkernel](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BK%7D%28x%2Cx%27%29%3Dx%5ETx%27)

This kind of models gives us the ability to predict for the uncertainty. How much uncertain on some data points (estimate the variance).

## Bayesian deep learning

work on high-dimension data.



- SGLD: generate from mini-batch SGD, with a more random noise item:

  ![weight](http://latex.codecogs.com/svg.latex?w_%7Bt%2B1%7D%3Dw_t-%5Cfrac%7B%5Cepsilon_t%7D%7B2%7D%28%5Cnabla%5Clog%5CPr%5Bw_t%5D%2B%5Cfrac%7BN%7D%7Bk%7D%5Csum_%7Bi%3D1%7D%5Ek%5Cnabla%5Clog%5CPr%5By_%7Bt%2Ci%7D%7Cx_%7Bt%2Ci%7D%2Cw_t%5D%29+%5Ceta_t) where ![weight](http://latex.codecogs.com/svg.latex?%5Ceta%5Csim%5Cmathcal%7BN%7D%280%2C%5Cepsilon%5E2%29)

  To avoid being stuck on some local areas.


## Summary

Why Bayesian learning are so popular? Because it captures two kinds of uncertainty:

- aleatoric uncertainty: noise in observations given perfect knowledge of the model. For instance, flipping a coin, even we know the coin is fair, the result is still uncertain.
- epistemic uncertainty: uncertainty about parameters due to the lack of data. For instance, I don't know what kind of coin I have when flipping it.

