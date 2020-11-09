---
title: Dictionary Learning
date: 2019-07-25
tags: [computational intelligence]
categories: course notes
---



# Compressive Sensing

## Motivation

Compress data while gathering. This will save a lot of storage memory.

The problem is we also need to decompress the signals.

## Concept

- original signal: 
  $$
  x\in\mathbb{R}^D
  $$

- "root signal": 
  $$
  z\in\mathbb{R}^D
  $$
  

  This signal tells something essence of ![img](http://latex.codecogs.com/svg.latex?x). For example, we have 5 clusters in a 20-D data points. The ![img](http://latex.codecogs.com/svg.latex?z) shows which cluster ![img](http://latex.codecogs.com/svg.latex?x) belongs to. So, ![img](http://latex.codecogs.com/svg.latex?z) can be very simple, ![img](http://latex.codecogs.com/svg.latex?K)-sparse (![img](http://latex.codecogs.com/svg.latex?K%5Cll D),  ![img](http://latex.codecogs.com/svg.latex?K%3D5) here) , but ![img](http://latex.codecogs.com/svg.latex?x) may **look** complex.  We view ![img](http://latex.codecogs.com/svg.latex?z) as the root of the signal ![img](http://latex.codecogs.com/svg.latex?x).

  ![img](http://latex.codecogs.com/svg.latex?x%3DUz)

- measured signal, also compressed signal: ![img](http://latex.codecogs.com/svg.latex?y%5Cin%5Cmathbb%7BR%7D%5EM%2CM%3C D). It is what we get after compression. 

  ![img](http://latex.codecogs.com/svg.latex?y%3DWx)

- reconstructed signal: ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx%7D%3DU%5Chat%7Bz%7D). We get the reconstructed signal by find the "optimal" ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bz%7D).

  - does it assume that we have already known ![img](http://latex.codecogs.com/svg.latex?U) and ![img](http://latex.codecogs.com/svg.latex?W)?

## Method

### Objective

Recover a sparse signal ![img](http://latex.codecogs.com/svg.latex?x) from measurements ![img](http://latex.codecogs.com/svg.latex?y). However, we cannot directly use the inversion of matrix. Because the matrix is invertible due to the rank. How should we do? We try to use the condition of the sparsity. Instead of find ![img](http://latex.codecogs.com/svg.latex?x) directly, we find ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bz%7D) 

![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bz%7D%5Cin%5Carg_%7Bz%7D%5Cmin%5C%7Cz%5C%7C_0%2C%20s.t.%20y%3DWUz)

### Sufficient conditions

Given any orthonormal basis ![img](http://latex.codecogs.com/svg.latex?U) we can obtain a stable reconstruction for any ![img](http://latex.codecogs.com/svg.latex?K)-sparse, compressible signal.

two conditions to be satisfied:

-  ![img](http://latex.codecogs.com/svg.latex?W) = Gaussian random projection, i.e. ![img](http://latex.codecogs.com/svg.latex?w_%7Bij%7D%5Csim%20%5Cmathcal%7BN%7D%280%2C%5Cfrac%7B1%7D%7BD%7D%29)
- ![img](http://latex.codecogs.com/svg.latex?M%5Cgeq%20cK%5Clog%28%5Cfrac%7BD%7D%7BK%7D%29)

### Algorithms

using (1) convex optimization or (2) matching pursuit to solve the objective.

# Dictionary Learning

In traditional encoding, we are given a basis ![img](http://latex.codecogs.com/svg.latex?U%5Cin%5Cmathbb%7BR%7D%5E%7BD%5Ctimes D%7D) and directly code the "root" signal by ![img](http://latex.codecogs.com/svg.latex?Uz) to get ![img](http://latex.codecogs.com/svg.latex?x). However, it is not problem-specific. We try to learn a ![img](http://latex.codecogs.com/svg.latex?U%5Cin%5Cmathbb%7BR%7D%5E%7BD%5Ctimes L%7D) to do sparse encoding and more problem-specific. 

Mathematically, it is to find ![img](http://latex.codecogs.com/svg.latex?U%5Cin%5Cmathbb%7BR%7D%5E%7BD%5Ctimes L%7D), s.t.

![img](http://latex.codecogs.com/svg.latex?X%5Csimeq%20U%5Ccdot%20Z)

- subject to sparsity on ![img](http://latex.codecogs.com/svg.latex?Z%5Cin%5Cmathbb%7BR%7D%5E%7BL%5Ctimes N%7D)
- subject to column/atom norm constraint on ![img](http://latex.codecogs.com/svg.latex?U%5Cin%5Cmathbb%7BR%7D%5E%7BD%5Ctimes L%7D)



## Model

![img](http://latex.codecogs.com/svg.latex?%28U%5E%2A%2C%20Z%5E%2A%29%5Cin%5Carg%5Cmin_%7BU%2CZ%7D%5C%7CX-U%5Ccdot%20Z%5C%7C_F%5E2)

- not convex jointly in ![img](http://latex.codecogs.com/svg.latex?U) and ![img](http://latex.codecogs.com/svg.latex?Z), but separately

### Algorithm

Using **iteratively greedy algorithm** to solve (like EM algorithm)

- coding step: 

  ![img](http://latex.codecogs.com/svg.latex?Z%5E%7B%28t%2B1%29%7D%3D%5Carg%5Cmin_Z%20%5C%7CX-U%5E%7B%28t%29%7DZ%5E%7B%28t%29%7D%5C%7C_F%5E2)

This is **column separable** problem. 

![img](http://latex.codecogs.com/svg.latex?z%5E%7B%28t%2B1%29%7D_j%3D%5Carg%5Cmin_%7Bz_j%7D%20%5C%7Cx_j-U%5E%7B%28t%29%7Dz_j%5E%7B%28t%29%7D%5C%7C_F%5E2,j%5Cin1,2,%5Ccdots N)

it is a sparse coding problem. Change the Frobenius norm as non-zero norm:

![img](http://latex.codecogs.com/svg.latex?z_n%5E%7B%28t%2B1%29%7D%3D%5Carg%5Cmin_%7Bz_n%7D%5C%7Cz_n%5C%7C_0)

​								s.t. ![img](http://latex.codecogs.com/svg.latex?%5C%7Cx_n-U%5Etz_n%5C%7C_2%5Cleq%5Csigma%5Ccdot%5C%7Cx_n%5C%7C_2)

​	- are these two equivalent???		

- dictionary update step:

  ![img](http://latex.codecogs.com/svg.latex?U%5E%7B%28t%2B1%29%7D%3D%5Carg%5Cmin_U%20%5C%7CX-U%5E%7B%28t%29%7DZ%5E%7B%28t%2B1%29%7D%5C%7C_F%5E2)

  it is **not separable** for U. We use **approximation** strategy, updating one atom at a time

  1. Set ![img](http://latex.codecogs.com/svg.latex?U%20%3D%20%5Bu_1%5Et%2Cu_2%5Et%2C%5Ccdots%2Cu_l%2C%5Ccdots%2Cu_L%5Et%5D), fix all other atoms except ![img](http://latex.codecogs.com/svg.latex?u_l)
  2. Isolate ![img](http://latex.codecogs.com/svg.latex?R_l^t), the residual that is due to atom ![img](http://latex.codecogs.com/svg.latex?u_l)
  3. Find ![img](http://latex.codecogs.com/svg.latex?u_l%5E*) that minimizes ![img](http://latex.codecogs.com/svg.latex?R_l%5Et), subject to ![img](http://latex.codecogs.com/svg.latex?%5C%7Cu_l%5E%2A%5C%7C_2%3D1). 

#### Precise Solution of U

Actually, ![img](http://latex.codecogs.com/svg.latex?u_l%5E*) can be exactly calculated. The objective can be rewritten into 

![img](http://latex.codecogs.com/svg.latex?u_l%5E%2A%20%3D%5Carg%5Cmin_%7Bu_l%7D%5C%7CR_l-u_l%20z_l%5ET%5C%7C_F%5E2)

Here, ![img](http://latex.codecogs.com/svg.latex?z_l) is the ![img](http://latex.codecogs.com/svg.latex?l)-th row of the matrix ![img](http://latex.codecogs.com/svg.latex?Z).

![img](http://latex.codecogs.com/svg.latex?%5C%7CR_l-u_lz_l%5ET%5C%7C_F%5E2%3D%5CSigma_%7Bn%3D1%7D%5EN%5C%7CR_%7Bl%28%5Ccdot%2Cn%29%7D-u_lz_%7Bl%2Cn%7D%5C%7C_F%5E2)

![img](http://latex.codecogs.com/svg.latex?%3D%5CSigma_%7Bn%3D1%7D%5EN%5CSigma_%7Bi%3D1%7D%5EL%28R_%7Bl%28i%2Cn%29%7D-u_%7Bl%28i%29%7Dz_%7Bl%2Cn%7D%29%5E2)

![img](http://latex.codecogs.com/svg.latex?%3D%5CSigma_%7Bi%3D1%7D%5EL%5CSigma_%7Bn%3D1%7D%5EN%28R_%7Bl%28i%2Cn%29%7D-u_%7Bl%28i%29%7Dz_%7Bl%2Cn%7D%29%5E2)

Using Lagrange equation and derivative by ![img](http://latex.codecogs.com/svg.latex?u_%7Bl%28i%29%7D), we get

![img](http://latex.codecogs.com/svg.latex?u_%7Bl%28i%29%7D%20%3D%20%5Cfrac%7B%5CSigma_%7Bn%3D1%7D%5ENR_%7Bl%28n%2Ci%29%7Dz_n%7D%7B%5Clambda%2B%5CSigma_%7Bn%3D1%7D%5ENz_n%5E2%7D)

Normalized to replace ![img](http://latex.codecogs.com/svg.latex?%5Clambda), we can get 

![img](http://latex.codecogs.com/svg.latex?u%5E*_%7Bl%28i%29%7D%20%3D%20%5Cfrac%7B%5CSigma_%7Bn%3D1%7D%5ENR_%7Bl%28n%2Ci%29%7Dz_n%7D%7B%5CSigma_%7Bn%3D1%7D%5EN%5CSigma_%7Bi%3D1%7D%5ELR_%7Bl%28n%2Ci%29%7Dz_n%7D)

After getting ![img](http://latex.codecogs.com/svg.latex?u_l%5E*), we still need to update ![img](http://latex.codecogs.com/svg.latex?z_l).

Compared to the below approximate solution, this method is much computation consuming, needing multiple times iterations. But the SVD solution directly catches the essence of the matrix, only needing one time computation.

#### Approximate solution of U

We can also solve ![img](http://latex.codecogs.com/svg.latex?u_l%5E*) by approximate K-SVD update. K means the sparsity is K. 

![img](http://latex.codecogs.com/svg.latex?%5C%7CZ%5E%2A%5C%7C_0%5Cleq%20K)

Since our objective is to minimize the residual:

![img](http://latex.codecogs.com/svg.latex?%5C%7CR_l-u_l%20z_l%5ET%5C%7C_F%5E2)

Actually, it is to approximate ![img](http://latex.codecogs.com/svg.latex?R_l) by a rank-1 matrix ![img](http://latex.codecogs.com/svg.latex?u_l%20z_l%5ET)

**SVD can be viewed as decompose a matrix by the sum of several rank-1 matrices.**

![img](http://latex.codecogs.com/svg.latex?R_l%3D%5CSigma_%7Bi%7D%5Csigma u_iv_i%5ET)

The framework is:

1. we do SVD decomposition of ![img](http://latex.codecogs.com/svg.latex?R)
2. take the first left-singular vector ![img](http://latex.codecogs.com/svg.latex?u_l%5E%2A%20%3Du_1)
3. update ![img](http://latex.codecogs.com/svg.latex?z%3D%5Csigma_1v_1%5ET)

The 1st singular value realization is: 

1. we extract the active data points from the whole ![img](http://latex.codecogs.com/svg.latex?N) points at ![img](http://latex.codecogs.com/svg.latex?l).

   ![img](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BN%7D%5Cleftarrow%20%5C%7Bn%7CZ_%7Bln%7D%5Cneq%200%2C%201%5Cleq%20n%5Cleq%20N%5C%7D)

2. calculate ![img](http://latex.codecogs.com/svg.latex?R), by

   ![img](http://latex.codecogs.com/svg.latex?R%5Cleftarrow X_%7B%3A%2C%5Cmathcal%7BN%7D%7D-UZ_%7B%28%3A%2C%5Cmathcal%7BN%7D%29%7D)
   
3. define ![img](http://latex.codecogs.com/svg.latex?g%5Cleftarrow%20z_%7B%28l%2C%5Cmathcal%7BN%7D%29%7D%5ET)

4. ![img](http://latex.codecogs.com/svg.latex?u_l%5E%2A%5Cleftarrow%20%5Cfrac%7BRg%7D%7B%5C%7CRg%5C%7C%7D)

5. ![img](http://latex.codecogs.com/svg.latex?z_%7B%28l%2C%5Cmathcal%7BN%7D%29%7D%5Cleftarrow%20g%5ET)

   

   In the step 4, we use the power iteration, the power is only 1 here.![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7BR%5Ekg%7D%7B%5C%7CR%5Ekg%5C%7C%7D)

   The theory basis is if we do SVD for ![img](http://latex.codecogs.com/svg.latex?R_l), 

   - ![img](http://latex.codecogs.com/svg.latex?u_l%5E*) is the first left-singular vector.
     - any proof
     - from the above solution. The "precise" solution is not a good solution. We should use SVD approximation. Since the precise solution of u is based on z, it is only optimal given z. However, we will update z after getting u. So, u will not be good enough under the new z. We need to iterate multiple times until convergence.
     - But in SVD, we directly use the 1st singular value, no need to iterating.

#### Initialization

- random atoms. Normal distribution and scale to unit
- samples from ![img](http://latex.codecogs.com/svg.latex?X): sample ![img](http://latex.codecogs.com/svg.latex?L) times  from ![img](http://latex.codecogs.com/svg.latex?X), according to the uniform distribution of index. Then scale to the unit.
- fixed overcomplete dictionary, such as DCT.

## Adaptation

## Applications

## image reconstruction: 

- train a lot of data to get the dictionary
- learn a sparse code ![img](http://latex.codecogs.com/svg.latex?z) for a given data ![img](http://latex.codecogs.com/svg.latex?x)
- modify ![img](http://latex.codecogs.com/svg.latex?z) and get ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bz%7D)
- reconstruct ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx%7D%3DU%5Chat%7Bz%7D)

## Speech denoise

The record is composed of the original speech and interferer. We want to remove the interferer and leave the original speech.

### Method

1. learn dictionaries for speech ![img](http://latex.codecogs.com/svg.latex?U_c) and interferer ![img](http://latex.codecogs.com/svg.latex?U_n) specifically.
2. Encoding the measured signal ![img](http://latex.codecogs.com/svg.latex?x)on these two dictionaries ![img](http://latex.codecogs.com/svg.latex?%5Bz_c%2Cz_n%5D). 
3. Drop the encoded signals on the interferer dictionary, only keep the speech encoded signals ![img](http://latex.codecogs.com/svg.latex?%5Bz_c%2C0%5D). 
4. Recover the "pure"  signal ![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx%7D%3DU_cz_c)

#### Learning step

learn the two dictionaries

![img](http://latex.codecogs.com/svg.latex?%28U%5E%2A%2CZ%5E%2A%29%5Cin%5Carg%5Cmin_%7BU%2CZ%7D%5C%7CX-U%5Ccdot%20Z%5C%7C_F%5E2)

​							s.t. ![img](http://latex.codecogs.com/svg.latex?%5C%7Cu_%7B%28%3A%2Cd%29%7D%5E%2A%5C%7C_2%3D1%2C%5Cforall%20d%20%3D%201%2C2%2C%5Ccdots%2CL)

![img](http://latex.codecogs.com/svg.latex?%5C%7CZ%5E%2A%5C%7C_0%5Cleq%20K)

#### Enhancement step

Using the learned dictionaries to encode the signals and drop noise signals.

![img](http://latex.codecogs.com/svg.latex?%28z_%7B%28s%29%7D%5E%2A%2Cz_%7B%28i%29%7D%5E%2A%29%5Carg%5Cmin_%7Bz%5E%7B%28s%29%7D%2Cz%5E%7B%28i%29%7D%7D%5C%7CX-%5BU%5E%7B%28s%29%7DU%5E%7B%28i%29%7D%5D%5Ccdot%5Bz%5E%7B%28s%29%7D%20z%5E%7B%28i%29%7D%5D%5ET%5C%7C_2)

​									s.t. ![img](http://latex.codecogs.com/svg.latex?%5C%7Cz_%7Bs%7D%5E%2A%5C%7C_0%2B%5C%7Cz_%7Bi%7D%5E%2A%5C%7C_0%5Cleq%20K)



![img](http://latex.codecogs.com/svg.latex?%5Chat%7Bx%7D%3DU_s%5E%2Az_%7Bs%7D%5E%2A)