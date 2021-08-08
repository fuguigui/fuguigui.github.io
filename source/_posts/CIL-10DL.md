---
title: Dictionary Learning
date: 2019-07-25
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
---



# Compressive Sensing

## Motivation

Compress data while gathering. This will save a lot of storage memory.

The problem is we also need to decompress the signals.

## Concept

- original signal:  $x\in\mathbb{R}^D$
  
- "root signal":  $z\in\mathbb{R}^D$
  
  This signal tells something essence of $x$. For example, we have 5 clusters in a 20-D data points. The  $z$ shows which cluster  $x$ belongs to. So,  $z$ can be very simple, $K$-sparse, here $K\ll D, K=5$ , but  may $x$ **look** complex.  We view  $z$ as the root of the signal $x$​.
  $$
  x=Uz
  $$
  
- measured signal, also compressed signal: $y\in\mathbb{R}^M, M<D$​. It is what we get after compression. $y=Wx$

- reconstructed signal: $\hat{x}=U\hat{z}$ . We get the reconstructed signal by find the "optimal" $\hat{z}$​.

  - does it assume that we have already known $U$ and $W$?

## Method

### Objective

Recover a sparse signal $x$ from measurements $y$. However, we cannot directly use the inversion of matrix. Because the matrix is invertible due to the rank. How should we do? We try to use the condition of the sparsity. Instead of find  $x$ directly, we find  $\hat{z}$​
$$
\hat{z}\in\arg_z\min \|z\|_0, \text{s.t. } y=WUz
$$

### Sufficient conditions

Given any orthonormal basis $U$ we can obtain a stable reconstruction for any $K$-sparse, compressible signal.

two conditions to be satisfied:

-  $W$ = Gaussian random projection, i.e. $w_{ij}\sim\mathcal{N}(0,\frac{1}{D})$
- $M\geq cK\log \frac{D}{K}$

### Algorithms

using (1) convex optimization or (2) matching pursuit to solve the objective.

# Dictionary Learning

In traditional encoding, we are given a basis $U\in\mathbb{R}^{D\times D}$ and directly code the "root" signal by $Uz$ to get $x$. However, it is not problem-specific. We try to learn a $U\in\mathbb{R}^{D\times L}$  to do sparse encoding and more problem-specific. 

Mathematically, it is to find $U\in\mathbb{R}^{D\times L}$​, s.t. $X\simeq U\cdot Z$

- subject to sparsity on $Z\in\mathbb{R}^{L\times N}$
- subject to column/atom norm constraint on $U\in\mathbb{R}^{D\times L}$

## Model

$$
(U^\ast, Z^\ast)\in\arg\min_{U,Z}\|X-U\cdot Z\|_F^2
$$

- not convex jointly in $U$ and $Z$, but separately

### Algorithm

Using **iteratively greedy algorithm** to solve, like EM algorithm

- coding step: 
  $$
  Z^{(t+1)}=\arg\min_Z\|X-U^{(t)}Z^{(t)}\|_F^2
  $$

This is **column separable** problem. 

$$
z_j^{(t+1)}=\arg\min_{z_j}\|x_j -U^{(t)}z_j^{(t)}\|_F^2, j\in 1,2,\cdots, N
$$

it is a sparse coding problem. Change the Frobenius norm as non-zero norm:
$$
z_n^{(t+1)}=\arg\min_{z_n}\|z_n\|_0
$$
s.t.
$$
\|x_n - U^tz_n\|_2\leq \sigma\cdot \|x_n\|_2
$$
​	- are these two equivalent???		

- dictionary update step:
  $$
  U^{(t+1)}=\arg\min_U\|X-U^{(t)}Z^{(t+1)}\|_F^2
  $$
  it is **not separable** for U. We use **approximation** strategy, updating one atom at a time

  1. Set $U=[u_1^t,u_2^t,\cdots, u_l^t, \cdots, u_L^t]$,  fix all other atoms except $u_l$
  2. Isolate $R_l^t$, the residual that is due to atom $u_l$
  3. Find $u_l^\ast$​ that minimizes $R_l^t$​, subject to $\|u_l^\ast\|_2=1$. 

#### Precise Solution of U

Actually, $u_l^\ast$​​ can be exactly calculated. The objective can be rewritten into 
$$
u_l^\ast = \arg\min_{u_l} \|R_l - u_l z_l^\top \|_F^2
$$

Here, $z_l$ is the $l$-th row of the matrix $Z$. $\|R_l - u_l z_l^\top \|_F^2$
$$
=\sum \|R_{l(\cdot, n)}-u_l z_{l,n}\|_F^2
$$

$$
=\sum_{n=1}^N\sum_{i=1}^L[R_{l(i,n)}-u_{l(i)}z_{l,n}]^2
$$

$$
=\sum_{i=1}^L\sum_{n=1}^N[R_{l(i,n)}-u_{l(i)}z_{l,n}]^2
$$

Using Lagrange equation and derivative by $u_{l(i)}$​, we get
$$
u_{l(i)}=\frac{\sum_{n=1}^N R_{l(n,i)}z_n}{\lambda + \sum_{n=1}^N z_n^2}
$$
Normalized to replace $\lambda$​, we can get 
$$
u_{l(i)}^\ast = \frac{\sum_{n=1}^N R_{l(n,i)}z_n}{\sum_{n=1}^N\sum_{i=1}^L R_{l(n,i)}z_n}
$$
After getting $u_l^\ast$, we still need to update $z_l$.

Compared to the below approximate solution, this method is much computation consuming, needing multiple times iterations. But the SVD solution directly catches the essence of the matrix, only needing one time computation.

#### Approximate solution of U

We can also solve $u_l^\ast$ by approximate K-SVD update. K means the sparsity is $K$.
$$
\|Z^\ast\|_0\leq K
$$
Since our objective is to minimize the residual:
$$
\|R_l - u_lz_l^\top\|_F^2
$$
Actually, it is to approximate $R_l$ by a rank-1 matrix $u_lz_l^\top$

**SVD can be viewed as decompose a matrix by the sum of several rank-1 matrices.**
$$
E_l = \sum_{i}\sigma u_i v_i^\top
$$
The framework is:

1. we do SVD decomposition of $R$
2. take the first left-singular vector $u_l^\ast = u_1$
3. update $z=\sigma_1v_1^\top$ 

The 1st singular value realization is: 

1. we extract the active data points from the whole $N$ points at $l$​.
   $$
   \mathcal{N}\leftarrow \{n|Z_{ln}\neq 0, 1\leq n\leq N\}
   $$

2. calculate $R$​​, by $R\leftarrow X_{:,\mathcal{N}} - UZ_{(:,\mathcal{N})}$​

3. define $g\leftarrow z_{l,\mathcal{N}}^\top$ 

4. $u_l^\ast\leftarrow\frac{Rg}{\|Rg\|}$

5. $z_{l,\mathcal{N}}\leftarrow g^\top$

   In the step 4, we use the power iteration, the power is only 1 here. $\frac{R^kg}{\|R^kg\|}$

   The theory basis is if we do SVD for $R_l$, 

   - $u_l^\ast$ is the first left-singular vector.
     - any proof
     - from the above solution. The "precise" solution is not a good solution. We should use SVD approximation. Since the precise solution of u is based on z, it is only optimal given z. However, we will update z after getting u. So, u will not be good enough under the new z. We need to iterate multiple times until convergence.
     - But in SVD, we directly use the 1st singular value, no need to iterating.

#### Initialization

- random atoms. Normal distribution and scale to unit
- samples from $X$: sample $L$ times  from $X$,  according to the uniform distribution of index. Then scale to the unit.
- fixed overcomplete dictionary, such as DCT.

## Adaptation

## Applications

## image reconstruction: 

- train a lot of data to get the dictionary
- learn a sparse code $z$ for a given data $x$
- modify $z$ and get $\hat{z}$
- reconstruct $\hat{x}=U\hat{z}$

## Speech denoise

The record is composed of the original speech and interferer. We want to remove the interferer and leave the original speech.

### Method

1. learn dictionaries for speech $U_c$ and interferer  $U_n$ specifically.
2. Encoding the measured signal $x$on these two dictionaries $[z_c, z_n]$​.
3. Drop the encoded signals on the interferer dictionary, only keep the speech encoded signals $[z_c, 0]$. 
4. Recover the "pure"  signal $\hat{x}=U_c z_c$

#### Learning step

learn the two dictionaries

$$
(U^\ast, Z^\ast)\in\arg\min_{U,Z}\|X-U\cdot Z\|_F^2
$$


s.t. $\|u^\ast_{(:,d)}\|_2=1,\forall d=1,2,\cdots, L$ , $\|Z^\ast\|_0\leq K$

#### Enhancement step

Using the learned dictionaries to encode the signals and drop noise signals.
$$
(z^{(s)\ast}, z^{(i)\ast})\leftarrow \arg\min \|X-[U^{(s)}U^{(i)}]\cdot [z^{(s)}z^{(i)}]^\top\|_2
$$
s.t. $\|z_s^\ast\|_0 + \|z_i^\ast\|_0\leq K$, $\hat{x}=U_s^\ast z_s^\ast$ 
