---
title: Sparse Coding
date: 2019-07-22
tags: [computational intelligence]
categories: course notes
---



# Sparse Coding

Encode an entity by fewer digits. What we need: a dictionary to encoding and the input signal. We will return the output signal.

- how to build the dictionary (find the suitable basis)?
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

![img](http://latex.codecogs.com/svg.latex?f%28k%29%3D%5CSigma_%7Bn%7Da_n%5Csin%28%5Comega_n%20k%29)

- continuous:![img](http://latex.codecogs.com/svg.latex?a_n%20%3D%20%5Cfrac%7B1%7D%7B%5Cpi%7D%5Cint_%7B-%5Cpi%7D%5E%7B%5Cpi%7Df%28x%29%5Csin nx%20dx)
- discrete: ![img](http://latex.codecogs.com/svg.latex?a_n%3D%5CSigma_%7Bk%3D1%7D%5ENx_k%5Csin%28%5Comega_n%20k%29)

![img](http://latex.codecogs.com/svg.latex?f%28x%29%3D%5CSigma_%7Bn%3D1%7D%5Cfrac%7B4%7D%7B%282n-1%29%5Cpi%7D%5Csin%282n-1%29x)

- not sufficient for localized signals

#### In 2-D

- procedure:
  - first take FT of the columns then FT of the rows. (can be interchanged)
- Interpretation:
  - large changes in the pixel values: high frequency, e.g. edges, background objects.

### Discrete cosine transform

- 1-D:

  ![img](http://latex.codecogs.com/svg.latex?z_k%20%3D%20%5CSigma_%7Bn%3D0%7D%5E%7BN-1%7Dx_n%5Ccos%5B%5Cfrac%7B%5Cpi%7D%7BN%7D%28n%2B%5Cfrac%7B1%7D%7B2%7D%29k%5D)

- 2-D:

  ![img](http://latex.codecogs.com/svg.latex?z_%7Bk_1%2Ck_2%7D%3D%5CSigma_%7Bn_1%3D0%7D%5E%7BN_1-1%7D%5CSigma_%7Bn_2%3D0%7D%5E%7BN_2-1%7Dx_%7Bn_1%2Cn_2%7D%5Ccos%5B%5Cfrac%7B%5Cpi%7D%7BN_1%7D%28n_1%2B%5Cfrac%7B1%7D%7B2%7Dk_1%29%5D%5Ccos%5B%5Cfrac%7B%5Cpi%7D%7BN_2%7D%28n_2%2B%5Cfrac%7B1%7D%7B2%7D%29k_2%5D)



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
  - the representation of ![img](http://latex.codecogs.com/svg.latex?x) in terms of ![img](http://latex.codecogs.com/svg.latex?u_1%2Cu_2%27%5Ccdots%2Cu_l) is **not unique**.

Goal: want to find sparse representation ![img](http://latex.codecogs.com/svg.latex?z) of ![img](http://latex.codecogs.com/svg.latex?x)

![img](http://latex.codecogs.com/svg.latex?z%5E%2A%5Cin%20%5Carg%5Cmin_%7Bz%5Cin%20R%5El%7D%5C%7Cz%5C%7C_0)

where ![img](http://latex.codecogs.com/svg.latex?%20Uz%3Dx%2CU%3D%5Bu_1%7Cu_2%7C%5Ccdots%7Cu_n%5D) . Here ![img](http://latex.codecogs.com/svg.latex?%5C%7Cz%5C%7C_0) counts the number of non-zero elements.



This is a NP-hard problem.

## Example

### Gabor wavelet

- have a look of this.

## Coherence

### Definition

![img](http://latex.codecogs.com/svg.latex?m%28U%29%3D%5Cmax_%7Bi%2Cj%3Ai%5Cneq%20j%7D%7Cu_i%5ETu_j%7C)

it measures the linear dependency for a dictionary

### Property

- why a new atom ![img](http://latex.codecogs.com/svg.latex?u) added to orthogonal B, will lead to the coherence increasing at least ![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B1%7D%7B%5Csqrt%7BD%7D%7D)

## Reconstruction

- what is the object of signal reconstruction? X or Z? To reconstruct the original signal X? Or represent X by Z? Find X or find Z?
- how to solve for the general dictionary?



### Methods

#### Matching pursuit

- procedure
  1. initialize: residual ![img](http://latex.codecogs.com/svg.latex?r_0%3Dx) and approximation ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx_0%7D%3D0)
  2. repeat:
     1. find ![img](http://latex.codecogs.com/svg.latex?j%5E%2A%20%3D%20%5Carg%5Cmax_j%7C%5Clangle%20r_i%2Cu_j%5Crangle%7C)
     2. compute better approximation ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx_%7Bi%2B1%7D%7D%5Cleftarrow%5Chat%7Bx_i%7D%2B%5Clangle%20r_i%2Cu_%7Bj%5E%2A%7D%5Crangle%20u_%7Bj%5E%2A%7D)
     3. update residual ![img](http://latex.codecogs.com/svg.latex?r_%7Bi%2B1%7D%5Cleftarrow r_i-%5Clangle%20r_i%2Cu_%7Bj%5E%2A%7D%5Crangle%20u_%7Bj%5E%2A%7D)
- Convergence: is the ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx_i%7D%5Cto x)?
  - Yes, it is. We can prove that by proving ![img](http://latex.codecogs.com/svg.latex?%5C%7Cr_i%5C%7C_2%5E2%5Cto 0)
  - we can prove that by proving ![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5C%7Cr_%7Bi%2B1%7D%5C%7C_2%5E2%7D%7B%5C%7Cr_i%5C%7C_2%5E2%7D%5Cleq%5Cgamma%2C%5Cexists%5Cgamma%3C%201)
  - the last condition can be satisfied when ![img](http://latex.codecogs.com/svg.latex?u) span ![img](http://latex.codecogs.com/svg.latex?%5Cmathbb%7BR%7D%5En)
- Residual minimization
  - matching pursuit greedily reduces the residual energy at every iteration.
  - it can be proved that minimize the residual equals to maximize the parameters with the condition of orthogonal.
    - the orthogonal only aims at the current selected atom. The residual is orthogonal to this atom, but may not be orthogonal to other atoms. The basis atoms don't need to be orthogonal to each other.

#### Convex optimization

- try to find under which condition of U, can this two problem (0-norm and 1-norm) be equivalent?