---
title: Generative Models
date: 2019-07-18
tags: [computational intelligence]
categories: course notes
---



# Generative Models

- generate what?
  - according to given data, generate new data like them.
- methods?
  - variational autoencoders
  - generative adversarial network

## Deep Generative Models

- what is deep generative model?
  - Deep means "deep neural network". The DNN is used to learn a distribution which is as similar as possible to the true data distribution.
  - The problem setting is: we want to learn any kind of data distribution using **unsupervised** learning.
- what is it relationship with VAE?
  - VAE is one of the most commonly used and efficient approaches of Deep Generative models. 
  - GAN is another typical example. 

# Variational autoencoders (VAE)

- what does variational mean?

  - alter, or explore variations on *data you already have*, and not just in a random way either, but in a desired, *specific* direction. 
  - don't expect the exactly same point as the input, but **randomly sample from the latent space**, or generate variations on an input image, from a **continuous** latent space.
  - if the latent space is discrete, and I sample a variation from the gaps between points, the decoder will have no idea of this data point. The decoder has no idea how to deal with the region of the latent space.

- how does it work?

  - Design a **continuous**, allowing easily random sampling and interpolating latent space.
- this is a **latent variable model **? [zhihu](https://zhuanlan.zhihu.com/p/21741426)

  - Speaking of learning latent variable, we use EM algorithm before. There, the latent variable **h** is a discrete variable, has limited possible numbers. (Considering the clustering problem, usually **h** denotes whether the variable **x_i** belongs to the class **c_j**, there are totally N\*M h values.) But here, the **key assumption** is that **h is continuous**.  

  - Recap of the mixture clustering problem, there we include latent variable and write the log-likelihood of x according to the latent variable as:

    ![img](http://latex.codecogs.com/svg.latex?%5Clog%20p%28x%3B%5Ctheta%29%3D%5Clog%5B%5CSigma_%7Bj%3D1%7D%5EK%5Cpi_j%20p%28x%3B%5Ctheta_j%29%5D%3D%5Clog%5B%5CSigma_%7Bj%3D1%7D%5EK%20q_j%5Cfrac%7B%5Cpi_j%20p%28x%3B%5Ctheta_j%29%7D%7Bq_j%7D%5D)

    ![img](http://latex.codecogs.com/svg.latex?%5Cgeq%20%5CSigma_%7Bj%3D1%7D%5EKq_j%5B%5Clog%20p%28x%3B%5Ctheta_j%29%2B%5Clog%5Cpi_j%20-%5Clog%20q_j%5D)

  - Here, for continuous latent variable, it should be written as:

      ![img](http://latex.codecogs.com/svg.latex?%5Clog%20p%28x%3B%5Ctheta%29%3D%5Clog%28%5Cint p%28x%2ch%29dh%29%3D%5Clog%5Cint%20q%28h%7Cx%29%5Cfrac%7Bp%28x%2Ch%29%7D%7Bq%28h%7Cx%29%7Ddh)

      we can follow the former method and rewrite it go get a lower variational bound:

      ![img](http://latex.codecogs.com/svg.latex?%5cgeq%5Cint%20q%28h%7Cx%29%5B%5Clog%20p%28x%7Ch%29p%28h%29-%5Clog%20q%28h%7Cx%29%5Ddh%20%3D%20B%28q%2Cx%29)

      Then, we want to use some simple distribution family to approximate ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29), which maximizes ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29)

  - we can also rewrite ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29) as ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29%20%3D%20-%20D%5Bq%28h%7Cx%29%7C%7Cp%28h%29%5D%2B%5Cint%20q%28h%7Cx%29%5Clog%20p%28x%7Ch%29dh)
    - the first item is KL divergence
    - the second item can be viewed as reconstruction error.
    - Usually, the prior ![img](http://latex.codecogs.com/svg.latex?p%28h%29) is very simple to calculate directly.
    - Now, our goal is to find ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29) and ![img](http://latex.codecogs.com/svg.latex?p%28x%7Ch%29), which maximize the ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29)
  - What VAE does is not making assumption on ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29) but do the variable replacement. Using ![img](http://latex.codecogs.com/svg.latex?f%28x%2c%5cepsilon%29%5csim q%28h%7cx%29)
    
    - the advantage is that if the variance of q() could be large, but we will use its gradient to rectify itself. The error may become larger. But if we use ![img](http://latex.codecogs.com/svg.latex?%5cepsilon) which comes from a stable distribution, the variance of the estimator will reduce a lot.

- [antoher link](https://towardsdatascience.com/deep-generative-models-25ab2821afd3)

## Compare to linear autoencoder

- The basic idea is the same as linear autoencoder: representing our data into low-dimensional latent variables, and reconstruct from it to minimize the reconstruction error.
- we can use DNN to realize non-linear.
- Different: the model latent variables are soft regions, i.e. continuous areas, instead of points.
- KL divergence enforces us to use Gaussian prior. Without it, the model would learn ![img](http://latex.codecogs.com/svg.latex?%5Csigma%5cto 0), falling to a normal autoencoder.

## Compare to autoencoder network

[medium link](https://towardsdatascience.com/intuitively-understanding-variational-autoencoders-1bfe67eb5daf)

Autoencoder:

- encoder: preserve as much as information as possible in limited encoding
- decoder: take the encoding and properly reconstruct it into a "full image".

Problem: the fundamental problem with autoencoders is that the latent space may **not be continuous**, or allow easy interpolation. 

Classic autoencoder learns a deterministic function that compresses the data while VAE learn the parameters of a probability distribution representing the data. We can sample from the distribution and generate new input data samples. It is a **generative** model.

instead of outputing an encoding vector of size n, it outputs two vectors of size n: a vector of means ![img](http://latex.codecogs.com/svg.latex?%5Cmu) and another vector of standard deviations ![img](http://latex.codecogs.com/svg.latex?%5Csigma)

That means, even for the same input, the encoded latent variables may vary a little due to the randomness.

By varying the encoding of one sample, a latent point is developing into smooth latent spaces on a local scale, that is, for similar samples.

## ELBO

Evidence Lower BOund.

![img](http://latex.codecogs.com/svg.latex?ELBO%28%5Cphi%2C%5Ctheta%29%20%3D%20E_%7Bq_%7B%5Cphi%7D%7D%5B%5Clog%20p_%7B%5Ctheta%7D%28x%7Cz%29%2B%5Clog%5Cfrac%7Bp%28z%29%7D%7Bq_%7B%5Cphi%7D%28z%7Cx%29%7D%5D%20%3D%20E_%7Bq_%7B%5Cphi%7D%7D%5B%5Clog%20p_%7B%5Ctheta%7D%28x%7Cz%29%5D-KL%28q_%7B%5Cphi%7D%28z%7Cx%29%7C%7Cp%28z%29%29)

- what is the reconstruction error?
  - The first item reflects the reconstruction error. To maximize ELBO, we want to maximize the first item, which means maximizing the likelihood of the generated data x_i.
  - This is related to reconstruction quality.
- To minimize the KL item, encouraging the posterior distribution to be close to the prior distribution on the latent variable. It works as a regularizer.
- Combining these two objectives, we can achieve the clusters and variation ability.

### KL divergence

- what is the function of KL divergence here?
  - make encodings, *all* of which are as close as possible to each other while still being distinct,
  - allowing smooth interpolation (because the lack of holes), 
  - and enabling the construction of *new* samples.

The KL divergence between two probability distributions simply measures how much they *diverge* from each other. Minimizing the KL divergence here means optimizing the probability distribution parameters **(μ** and **σ)** to closely resemble that of the target distribution

For VAEs, the KL loss is equivalent to the *sum* of all the KL divergences between the *component* ![img](http://latex.codecogs.com/svg.latex?X_i%5csim N%28%5Cmu,%5csigma_i%5E2%29) in **X**, and the standard normal. It’s minimized when ![img](http://latex.codecogs.com/svg.latex?%5Cmu_i%3D0,%5csigma_i%3D1).

Intuitively, this loss encourages the encoder to distribute all encodings (for all types of inputs, eg. all MNIST numbers), **evenly around the center of the latent space**. If it tries to “cheat” by clustering them apart into specific regions, away from the origin, it will be penalized.

A good thing of VAE is that it can learn generative model and inference model at the same time.

## Reparametrization Trick

Replace ![img](http://latex.codecogs.com/svg.latex?z%20%3D%20g_%7B%5Cphi%7D%28%5Cepsilon%2Cx%29), so that we can do Monte Carol sampling and differentiate ![img](http://latex.codecogs.com/svg.latex?z) and backpropagation can be used to tune the parameters in the model.



Using simple ![img](http://latex.codecogs.com/svg.latex?%5Cepsilon,%5Cepsilon%5Csim f%28x%2C%5Cepsilon%29) to replace  ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29)



Gradient of expectation -> Expectation of gradient -> average of stochastic gradient.



## Framework
The encoder and decoder networks are trained to solve the optimization problem  ![img](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%2A%2C%5Cphi%5E%2A%3D%5Carg%5Cmax_%7B%5Ctheta%2C%5Cphi%7D%20L%28x%5E%7B%28i%29%7D%2C%5Ctheta%2C%5Cphi%29).

- the encoder network (inference step)
  - Maximize ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29) with respect to ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29), given ![img](http://latex.codecogs.com/svg.latex?p%28x%7Ch%29)
  - optimize the regularizer term, by making the approximate posterior distribution ![img](http://latex.codecogs.com/svg.latex?q_%7B%5Cphi%7D) as close to the latent variable prior.
  - sample a latent sample ![img](http://latex.codecogs.com/svg.latex?z%5csim q_%7B%5Cphi%7D) and pass it to the decoder network.
- the decoder network (genera)
  - Maximize ![img](http://latex.codecogs.com/svg.latex?B%28q%2Cx%29) with respect to ![img](http://latex.codecogs.com/svg.latex?p%28x%7Ch%29), given ![img](http://latex.codecogs.com/svg.latex?q%28h%7Cx%29)
  - produces samples ![img](http://latex.codecogs.com/svg.latex?%5chat%7Bx%7D) that maximize the reconstruction quality with respect to the original input.
- Both terms of ELBO are differentiable, we can use a training method like SGD to train the VAE model end-to-end.

# GAN (Generative Adversarial Network)

Two components: a generator and a discriminator.

![img](http://latex.codecogs.com/svg.latex?%5Cmin_%7BG%7D%5Cmax_%7BD%7DV%28D%2CG%29%20%3D%20E_%7Bx%5Csim%20P_%7Bdata%7D%28x%29%7D%5B%5Clog%20D%28x%29%5D%2BE_%7Bz%5Csim%20P_z%28z%29%7D%5B%5Clog%281-D%28G%28z%29%29%29%5D)

- generator: given the labels, generate new false data point and mix them with the real data, try to use a distribution that make the "false" data looks like the real data as large as possible. 
  - **Minimize** the likelihood of the data: 
    - when the data is from the real data set, make the likelihood of the data be real as low as possible: 
    - when the data is from the fake dataset, make the likelihood of the data be fake as low as possible
- discriminator: given the data points, try to find the real data and false data as precise as possible. In other words, try to label data as their real label.
  - **Maximize** the likelihood.
    - when the data is real, try to discriminate is as real
    - when the data is fake, try to discriminate is as fake.

This is a saddle-point problem. The objective is opposite in two components. 

![img](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%2A%20%3A%3D%5Carg%5Cmin_%7B%5Ctheta%5Cin%5CTheta%7D%5C%7B%5Csup_%7B%5Cphi%5Cin%5CPhi%7Dl%28%5Ctheta%2C%5Cphi%29%5C%7D)

## Methods

SGD as a heuristic:

![img](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%7Bt%2B1%7D%3D%5Ctheta%5Et-%5Ceta%5Cnabla_%7B%5Ctheta%7Dl%28%5Ctheta%5Et%2C%5Cphi%5Et%29)

![img](http://latex.codecogs.com/svg.latex?%5Cphi%5E%7Bt%2B1%7D%3D%5Cphi %5Et%2B%5Ceta%5Cnabla_%7B%5Cphi%7Dl%28%5Ctheta%5E%7Bt%2B1%7D%2C%5Cphi%5Et%29)

## Practical Considerations

Need to balance generator and discriminator: may train some at different speeds.



# VAE VS GAN

## Quality

- VAE: more blurry
  - caused by pixel-wise factorization and local loss
  - high-frequency details are poorly correlated.
- GAN: sharper

## Train

- VAE: somewhat easier to train
- GAN: very hard to train

## Applications

- GAN:
  - learn an **implicit** density, can only be used for generating
- VAE: 
  - learn an **explicit** density, can generate and encode.

# Auto-regressive model

Generate output one variable at a time.

![img](http://latex.codecogs.com/svg.latex?p%28x_1%2C%5Ccdots%2Cx_m%29%3D%5CPi_%7Bt%3D1%7D%5Em%20p%28x_t%7Cx_%7B1%3At-1%7D%29)

## Pixel-CNN

CNN can only use information about pixels above and to the left of the current pixel

can be realized by masking a filter.

Every time a pixel is predicted, it is fed back to the network to predict the next pixel

Slow process.