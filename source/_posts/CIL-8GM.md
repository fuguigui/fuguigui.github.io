---
title: Generative Models
date: 2019-07-18
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
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

# VAE: Variational autoencoders

- what does variational mean?

  - alter, or explore variations on *data you already have*, and not just in a random way either, but in a desired, *specific* direction. 
  - don't expect the exactly same point as the input, but **randomly sample from the latent space**, or generate variations on an input image, from a **continuous** latent space.
  - if the latent space is discrete, and I sample a variation from the gaps between points, the decoder will have no idea of this data point. The decoder has no idea how to deal with the region of the latent space.

- how does it work?

  - Design a **continuous**, allowing easily random sampling and interpolating latent space.
- this is a **latent variable model** ? [zhihu](https://zhuanlan.zhihu.com/p/21741426)

  - Speaking of learning latent variable, we use EM algorithm before. There, the latent variable $h$ is a discrete variable, has limited possible numbers. (Considering the clustering problem, usually $h$ denotes whether the variable $x_i$ belongs to the class $c_j$, there are totally $N\times M$  values.) But here, the **key assumption** is that $h$ is **continuous**.  

  - Recap of the mixture clustering problem, there we include latent variable and write the log-likelihood of x according to the latent variable as:
    $$
    \begin{align}
    \log p(x;\theta) & = \log[\sum_{j=1}^K\pi_j p(x;\theta_j)] = \log[\sum_{j=1}^K q_j\frac{\pi_j p(x;\theta_j)}{q_j}] \\
    & \geq \sum_{j=1}^K q_j[\log p(x;\theta_j)+\log\pi_j - \log q_j]\\
    \end{align}
    $$

  - Here, for continuous latent variable, it should be written as:
      $$
      \log p(x;\theta)=\log \int p(x,h)\mathrm{d}h = \log \int q(h|x)\frac{p(x,h)}{q(h|x)}\mathrm{d}h
      $$
      we can follow the former method and rewrite it go get a lower variational bound:
      $$
      \geq \int q(h|x)[\log p(x|h)p(h)-\log q(h|x)]\mathrm{d}h = B(q,x)
      $$
      Then, we want to use some simple distribution family to approximate $q(h|x)$, which maximizes $B(q,x)$

  - we can also rewrite $B(q,x)$​ as 
    $$
    B(q,x)=-D[q(h|x)||p(h)]+\int q(h|x)\log p(x|h)\mathrm{d}h
    $$
    
    - the first item is KL divergence
    - the second item can be viewed as reconstruction error.
    - Usually, the prior $p(h)$ is very simple to calculate directly.
    - Now, our goal is to find $q(h|x)$ and $p(x|h)$, which maximize the $B(q,x)$
    
  - What VAE does is not making assumption on $q(h|x)$ but do the variable replacement. Using $f(x,\epsilon)\sim q(h|x)$
    
    - the advantage is that if the variance of $q(\cdot)$ could be large, but we will use its gradient to rectify itself. The error may become larger. But if we use  $\epsilon$ which comes from a stable distribution, the variance of the estimator will reduce a lot.

- [antoher link](https://towardsdatascience.com/deep-generative-models-25ab2821afd3)

## Compare to linear autoencoder

- The basic idea is the same as linear autoencoder: representing our data into low-dimensional latent variables, and reconstruct from it to minimize the reconstruction error.
- we can use DNN to realize non-linear.
- Different: the model latent variables are soft regions, i.e. continuous areas, instead of points.
- KL divergence enforces us to use Gaussian prior. Without it, the model would learn $\sigma\to 0$, falling to a normal autoencoder.

## Compare to autoencoder network

[medium link](https://towardsdatascience.com/intuitively-understanding-variational-autoencoders-1bfe67eb5daf)

Autoencoder:

- encoder: preserve as much as information as possible in limited encoding
- decoder: take the encoding and properly reconstruct it into a "full image".

Problem: the fundamental problem with autoencoders is that the latent space may **not be continuous**, or allow easy interpolation. 

Classic autoencoder learns a deterministic function that compresses the data while VAE learn the parameters of a probability distribution representing the data. We can sample from the distribution and generate new input data samples. It is a **generative** model.

instead of outputing an encoding vector of size n, it outputs two vectors of size n: a vector of means $\mu$ and another vector of standard deviations $\sigma$

That means, even for the same input, the encoded latent variables may vary a little due to the randomness.

By varying the encoding of one sample, a latent point is developing into smooth latent spaces on a local scale, that is, for similar samples.

## ELBO

Evidence Lower BOund.
$$
\text{ELBO}(\phi,\theta)=\mathbb{E}_{q_\phi} [\log p_\theta(x|z)+\log p(z) - \log q_\phi(z|x)
$$

$$
= \mathbb{E}_{q_\phi}[\log p_\theta (x|z)]-KL[q_\phi (z|x)\parallel p(z)]
$$

- what is the reconstruction error?
  - The first item reflects the reconstruction error. To maximize ELBO, we want to maximize the first item, which means maximizing the likelihood of the generated data $x_i$.
  - This is related to reconstruction quality.
- To minimize the KL item, encouraging the posterior distribution to be close to the prior distribution on the latent variable. It works as a regularizer.
- Combining these two objectives, we can achieve the clusters and variation ability.

### KL divergence

- what is the function of KL divergence here?
  - make encodings, *all* of which are as close as possible to each other while still being distinct,
  - allowing smooth interpolation, because the lack of holes, 
  - and enabling the construction of *new* samples.

The KL divergence between two probability distributions simply measures how much they *diverge* from each other. Minimizing the KL divergence here means optimizing the probability distribution parameters $\mu$ and $\sigma$) to closely resemble that of the target distribution

For VAEs, the KL loss is equivalent to the *sum* of all the KL divergences between the *component* $X_i\sim \mathcal{N}(\mu, \sigma_i^2)$ in $X$, and the standard normal. It’s minimized when $\mu_i=0,\sigma_i=1$.

Intuitively, this loss encourages the encoder to distribute all encodings, for all types of inputs, e.g. all MNIST numbers, **evenly around the center of the latent space**. If it tries to “cheat” by clustering them apart into specific regions, away from the origin, it will be penalized.

A good thing of VAE is that it can learn generative model and inference model at the same time.

## Reparametrization Trick

Replace $z=g_\phi (\epsilon, x)$ , so that we can do Monte Carol sampling and differentiate  $z$ and backpropagation can be used to tune the parameters in the model.

Using simple $\epsilon, \epsilon\sim f(x,\epsilon)$  to replace $q(h|x)$ .

Gradient of expectation $\to$ Expectation of gradient $\to$ average of stochastic gradient.

## Framework
The encoder and decoder networks are trained to solve the optimization problem 
$$
\theta^*, \phi^* = \arg\max_{\theta, \phi} L(x^{(i)}, \theta, \phi)
$$

- the encoder network (inference step)
  - Maximize $B(q|x)$ with respect to $q(h|x)$, given $p(x|h)$
  - optimize the regularizer term, by making the approximate posterior distribution $q_\phi$ as close to the latent variable prior.
  - sample a latent sample $z\sim q_\phi$ and pass it to the decoder network.
- the decoder network (genera)
  - Maximize $B(q,x)$ with respect to $p(x|h)$ given $q(h|x)$
  - produces samples $\hat{x}$ that maximize the reconstruction quality with respect to the original input.
- Both terms of ELBO are differentiable, we can use a training method like SGD to train the VAE model end-to-end.

# GAN: Generative Adversarial Network

Two components: a generator and a discriminator.
$$
\min_G\max_D V(D,G)=\mathbb{E}_x [\log D(x)] + \mathbb{E}_z[\log (1-D(G(z)))]
$$

- generator: given the labels, generate new false data point and mix them with the real data, try to use a distribution that make the "false" data looks like the real data as large as possible. 
  - **Minimize** the likelihood of the data: 
    - when the data is from the real data set, make the likelihood of the data be real as low as possible: 
    - when the data is from the fake dataset, make the likelihood of the data be fake as low as possible
- discriminator: given the data points, try to find the real data and false data as precise as possible. In other words, try to label data as their real label.
  - **Maximize** the likelihood.
    - when the data is real, try to discriminate is as real
    - when the data is fake, try to discriminate is as fake.

This is a saddle-point problem. The objective is opposite in two components. 
$$
\theta^*:=\arg\min_{\theta\in\Theta}\{\sup_{\phi\in\Phi}l(\theta,\phi)\}
$$

## Methods

SGD as a heuristic:
$$
\theta^{t+1}=\theta^t - \eta\nabla_\theta l(\theta^t, \phi^t)
$$

$$
\phi^{t+1} = \phi^t + \eta\nabla_\theta l(\theta^{t+1},\phi^t)
$$

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

- GAN: learn an **implicit** density, can only be used for generating
- VAE: learn an **explicit** density, can generate and encode.

# Auto-regressive model

Generate output one variable at a time.
$$
p(x_1,\cdots, x_m)=\prod_{t=1}^m p(x_t|x_{1:t-1})
$$

## Pixel-CNN

CNN can only use information about pixels above and to the left of the current pixel

can be realized by masking a filter.

Every time a pixel is predicted, it is fed back to the network to predict the next pixel

Slow process.