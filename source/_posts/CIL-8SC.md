---
title: CIL8 Sparse Coding
date: 2019-07-22
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
---



# Sparse Coding

Encode an entity by fewer digits. What we need: a dictionary to encoding and the input signal. We will return the output signal.

- how to build the dictionary or find the suitable basis?
  - Fixed ahead: Fourier, wavelet
  - Build according to the data: PCA.

## Key idea

the dictionary should be orthogonal!

## Advantage

[csdn](https://blog.csdn.net/YZXnuaa/article/details/80054179)

Using as little resource as possible to learn as much knowledge as possible and accelerate the speed.

## Methods

### Fourier 

basis functions:
$$
f(k)=\sum_na_n\sin(\omega_n k)
$$

- continuous: $a_n=\frac{1}{\pi}\int_{-\pi}^\pi f(x)\sin nx \mathrm{d}x$

- discrete: $a_n = \sum_{k=1}^N x_k\sin(\omega_n k)$
  $$
  f(x)=\sum_{n=1}\frac{4}{(2n-1)\pi}\sin(2n-1)x
  $$

- not sufficient for localized signals

#### In 2-D

- procedure:
  - first take FT of the columns then FT of the rows. can be interchanged
- Interpretation:
  - large changes in the pixel values: high frequency, e.g. edges, background objects.

### Discrete cosine transform

- 1-D: $z_k=\sum_{n=0}^{N-1}x_n\cos\frac{(2n+1)\pi k}{2N}$​​​​

- 2-D: $z_{k_1, k_2}=\sum_{n_1=0}^{N_1-1}\sum_{n_2=0}^{N_2-1}\cos\frac{(2n_1+1)\pi k_1}{2N_1}\cos\frac{(2n_2+1)\pi k_2}{2N_2}$



### Wavelet

#### Haar Wavelet

- Construction: all basis functions have zero mean, that's why we add (1,1,...,1) vectors.
- use the **mother wavelet** to create the other Haar functions.
- use the scaling function to normalize it.

### PCA

PCA is too sensitive to noise. That is it is only valid for data having the Gaussian distribution.

## Applications

### 1-D signal

### 2-D signal

- how to do Fourier/wavelet transform on 2-d signals?

# Overcomplete Dictionary

## Key idea

do sparse coding by using a very large dictionary. Try to find the optimally **sparse** encoding for all the data.

- why to encode sparsely? The advantage of doing so?
  - maybe find clusters? Like, four clusters nodes in 3-D. They are sprayed into four clusters. If we use 4-D vectors to encode these points, the encoded vectors will be quite sparse. For example, in the original 3-D coding, we use 1-256 for each dimension, totally, we need 256\*3 bits. But in the 4-D coding, we use 0,1 for each dimension, we need 2\*4 bits. That makes sense.
- how to do this?
  - pick atoms from **a union** of bases, (not just one set of orthogonal basis), each one responsible for one characteristic.
- Compared to the former method, the bases are **no** longer linearly independent. 
  - the representation of $x$ in terms of  $u_1,u_2,\cdots, u_l$is **not unique**.

Goal: want to find sparse representation $z$ of $x$​
$$
z^\ast\in\arg\min_{z\in R^l}\|z\|_0
$$
where $Uz=x, U=[u_1|u_2|\cdots|u_n]$ . Here  $\|z\|_0$ counts the number of non-zero elements.



This is a NP-hard problem.

## Example

### Gabor wavelet

- have a look of this.

## Coherence

### Definition

$$
m(U)=\max_{i,j,i\neq j}|u_i^\top u_j|
$$

it measures the linear dependency for a dictionary

### Property

- why a new atom $u$ added to orthogonal B, will lead to the coherence increasing at least $\frac{1}{\sqrt{D}}$

## Reconstruction

- what is the object of signal reconstruction? $X$ or $Z$? To reconstruct the original signal $X$? Or represent $X$ by $Z$? Find $X$ or find $Z$?
- how to solve for the general dictionary?



### Methods

#### Matching pursuit

- procedure
  1. initialize: residual $r_0\leftarrow x$​ and approximation $\hat{x}_0=0$
  2. repeat:
     1. find $j^\ast=\arg\max_j|\langle r_i, u_j\rangle|$
     2. compute better approximation$\hat{x}_{i+1}\leftarrow \hat{x}_i +\langle r_i, u_{j^\ast} \rangle u_{j^\ast}$​ 
     3. update residual $r_{i+1}\leftarrow r_i - \langle r_i, u_{j^\ast}\rangle u_{j^\ast}$
- Convergence: is the $\hat{x}_i\to x$ ?
  - Yes, it is. We can prove that by proving $\|r_i\|_2^2\to 0$
  - we can prove that by proving $\frac{\|r_{i+1}\|_2^2}{\|r_i\|_2^2}\leq \gamma. \exists\gamma < 1$ 
  - the last condition can be satisfied when $u$ span $\mathbb{R}^n$
- Residual minimization
  - matching pursuit greedily reduces the residual energy at every iteration.
  - it can be proved that minimize the residual equals to maximize the parameters with the condition of orthogonal.
    - the orthogonal only aims at the current selected atom. The residual is orthogonal to this atom, but may not be orthogonal to other atoms. The basis atoms don't need to be orthogonal to each other.

#### Convex optimization

- try to find under which condition of U, can this two problem, 0-norm and 1-norm, be equivalent?