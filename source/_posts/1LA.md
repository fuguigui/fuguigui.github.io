---
title: Linear Autoencoder
date: 2019-07-04
tags: [courses, machine learning, data analysis]
categories: Computational Intelligence
---



# Linear Autoencoder

# Pre-Questions
1. what? The framework of linear matrix low rank construction
2. Problem? To approximate a matrix with low-rank matrix. This is an encoding problem.
3. Key? Find the encoder C and decoder D.
4. Formula? Z = CX, X' = DZ=DCX
5. Applications? feature engineering, saving storage space.
6. Further Development? Non-linear? Supervized, not auto?

- Dimension Reduction
- Linear Autoencoder
- Singular Value Decomposition
- Remarks

# Dimension Reduction
- Object? A matrix or a data point? what kinds of conditions should this matrix satisfy?
	- A matrix, a series of data points. The data points have the same length (dimension)
- How to deal with missing values?
  - delete the data points
  - fill up with random values
- Methods: select or reconstruct?
	- both, but in linear encoding reconstruction can only be in the **linear** form.
- Algorithms? Efficiency?
  - iterative method
- Evaluation? what index is used to evaluate the dimension reduction?
	- evaluate the **distance** between matrices before and after encoding. 
	  - SVD is the best under Frobenius/Euclidean Norm.
	  - PCA is the best under the Euclidean distance.

In general, this is an encoding problem, use less bit to encode the original information while keep the information loss as little as possible.
## Aim
Work for a group of data points with the same dimension, not for only one data point.
### Mathematical Description

![img](http://latex.codecogs.com/svg.latex?R%5E%7BN%5Ctimes M%7D%5CRightarrow R%5E%7BN%5Ctimes K%7D%0D%0A)

## Method
The methods should be selected according to the to-encode objects.
### Change Basis(Space)
The original vector is expressed on one set of units, change the expression units, and express the vector by new units. Take picture for an example, a 100*100 head photo, the original unit is pixel, now we change the units as four layers, a fixed head, the hair contour, part highlight one and part highlight two. Therefore a 100*100 vector in the original space is expressed by 1*4 vector in new space.  
In another word, the new basis may save a lot of information for the data. Problem: the data points must be similar in some way to make basis useful. 

### Linear

![img](http://latex.codecogs.com/svg.latex?Z%3DC%5Ccdot X%2CC%5CinR%5E%7BK%5Ctimes M%7D)

### Non-linear
- what is the limitation and advantage of linear?
- what typical techniques in non-linear form?

### Unsupervised

- how to do it?

  That is the source of **auto** of in the name of linear autoencoder.

# Linear Autoencoder

Find a decoder matrix **D** working on **Z**, which makes **DZ** as close as possible to **X**.

## Idea

1. ![img](http://latex.codecogs.com/svg.latex?Z%3DC%5Ccdot%20X)
2. ![img](http://latex.codecogs.com/svg.latex?X%27%3DD%5Ccdot%20Z%20%3D%20D%5Ccdot%20C%5Ccdot%20X)

3. Choose **D** and **C** to make **X'** closed to **X**.

## Norms

There are many norms to evaluate how **X'** is closed to **X**. In the slides, we use Frobenius Norm. 

- How about using other norms? 
- What is best solution under other norms?

### Frobenius Norm

- vector: like the traditional norm definition

- matrix:  generate the definition of vector, the squared root of the sum of the square of each entry.  

  - In a calculation format
  
  ![img](http://latex.codecogs.com/svg.latex?%5Csqrt%7B%5CSigma_%7Bij%7Da_%7Bij%7D%5E2%7D%3D%5Csqrt%7B%5CSigma_i%5Csigma_i%5E2%7D)
  
  - in a matrix format, depends only on singular values of A.
  
    ![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_F%5E2%3Dtrace%28A%5E*A%29%3Dtrace%28AA%5E*%29%3D%5CSigma_i%5Csigma_i%5E2)
  

### p-norm
![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_p%3A%3D%5Csup%5C%7B%5C%7CAx%5C%7C_p%3A%5C%7Cx%5C%7C_p%3D1%5C%7D%2C%5C%7Cx%5C%7C_p%3A%3D%28%5CSigma_i%5C%7Cx_i%5C%7C%5Ep%29%5E%7B1%2Fp%7D)

### Spectral norm (the matrix version of Euclidean norm)
That is 2-norm, when p is 2.
![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_2%3A%3D%5Csup%5C%7B%5C%7CAx%5C%7C_2%3A%5C%7Cx%5C%7C_2%3D1%5C%7D%3D%5Csigma_1%3D%5Csqrt%7B%5Clambda_%7Bmax%7D%28AA%5ET%29%7D)

- ![img](http://latex.codecogs.com/svg.latex?%5Csigma) refers to the singular value.

## Key Theorem: Eckart-Young Theorem

 - **SVD** is the best approximation matrix with the rank **k**, under the Frobenius Norm (also under the Euclidean norm)

 - the minimum Frobenius (Euclidean) norm loss is the sum of square of singular values from **k+1**.

   ![img](http://latex.codecogs.com/svg.latex?J%28%5Ctheta%29%20%3D%5CSigma_%7Bl%3Dk%2B1%7D%5Csigma_l%5E2)

## Singular Vector Decomposition

Linear Autoencoder provides a framework, and SVD is one method to get the **D** and **C**. In SVD, **D = U_k** and **C=U_k^T** . To use SVD, the future to-be-predicted items must have been included in the matrix. No on-the-fly new prediction or no new data points can be added just for prediction, but not being trained.

**Any matrix** can do SVD.

### Formula

![img](http://latex.codecogs.com/svg.latex?X%3DU%5Ccdot%20D%5Ccdot%20V%5E%20T)

- **U**: orthogonal matrix
- **V**: orthogonal matrix
- **D**: diagonal matrix

### Interpretation

- **U**: **M\*M**, map the **M** objects to **M** classes, the entry **U_ij** is the object **i** 's value on class **j. 


- **V**: **N\*N**, map the **N** objects to **N** classes, the entry **V_ij** is the object **i** 's value on class **j**

- **D**: D_ii express the strength or expression ability of the class i or the strength of each factor.

### SGD
Stochastic gradient descending method.
- why to use this method? Any problem of solving the SVD directly?
  - it is used for CF. It optimizes only **over known ratings** (no need to input missing values). It tries to find low rank approximation **Q** and **P** for **X**, let  ![img](http://latex.codecogs.com/svg.latex?X%3DQ%5Ccdot%20P%2CX%5Cin%20R%5E%7BM%5Ctimes%20N%7D%2CQ%5Cin%20R%5E%7BM%5Ctimes%20K%7D%2CP%5Cin%20R%5E%7BK%5Ctimes%20N%7D)

### Relationships
- to linear autoencoder: ![img](http://latex.codecogs.com/svg.latex?C%3DU_k%5ET%2CD%3DU_k), or ![img](http://latex.codecogs.com/svg.latex?C%3DAU_k%5ET%2CD%3DU_kA%5E%7B-1%7D%)
- SGD UV forms to the original UDV forms
## Principle Component Analysis

- why we use PCA?
	- compared to SVD, which aims to minimize the **norm (Frobenius/Euclidean) ** of the whole **difference matrix X-X'** directly, PCA focuses on the "inner structure" of the matrix **X**, trying to consume the **variance** among **x_i**'s as large as possible.' 
- what is the advantage of PCA? 

- what is the basic idea of PCA?
	- works on the **variance matrix** ![img](http://latex.codecogs.com/svg.latex?AA%5ET%5Cin%20R%5E%7BM%5Ctimes%20M%7D), not the original matrix ![img](http://latex.codecogs.com/svg.latex?A%5Cin%20R%5E%7BM%5Ctimes%20N%7D).
	- minimize **Euclidean distance**
	- in other words, we want to find the direction where the variance is at the biggest (Therefore, this direction captures the change of the data at most, the data vary less on other directions.)
  - important **!!!**: direction of **smallest reconstruction error** <-> direction of **largest data variance** 

- what is relation to the **orthogonal projection**?
	- orthogonal projection corresponds to **euclidean** distance, which is the minimization goal of PCA.
	- the **eigenvalue** can be interpreted as the **variance** in the dimension specified by the corresponding eigenvector.

### Key Formula

- ![img](http://latex.codecogs.com/svg.latex?%3Cv%2Cu%3Eu%3D%28uu%5ET%29v), means the projection of v to the direction u.
- ![img](http://latex.codecogs.com/svg.latex?%28I-uu%5ET%29v%3Dv-%3Cv%2Cu%3Eu), means the orthogonal branch of v to u.
- Goal: find ![img](http://latex.codecogs.com/svg.latex?%28u%2C%5Cmu%29%5Cto%5Carg%5Cmin%5B%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%5C%7C%28I-uu%5ET%29%28x_i-%5Cmu%29%5C%7C%5E2%5D)
	1. it is easy to get ![img](http://latex.codecogs.com/svg.latex?%5Cmu%3D%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7Dx_i), that means **to use PCA, we should center the data**
	2. the goal becomes: ![img](http://latex.codecogs.com/svg.latex?u:%5Carg%5Cmin%5B%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%5C%7C%28I-uu%5ET%29x_i'%5C%7C%5E2%5D,x_i'%3Dx_i-%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bj%3D1%7Dx_j)
	3. Helper formula: ![img](http://latex.codecogs.com/svg.latex?%5C%7Cv-w%5C%7C%5E2%3D%3Cv-w%2Cv-w%3E%3D%5C%7Cv%5C%7C%5E2%2B%5C%7Cw%5C%7C%5E2-2%3Cv%2Cw%3E)
	4. ![img](http://latex.codecogs.com/svg.latex?%5Cto%5C%7C%28I-uu%5ET%29x_i%5C%7C%5E2%3D%5C%7Cx_i-uu%5ETx_i%5C%7C%5E2%3D%5C%7Cx_i%5C%7C%5E2%2B%5C%7Cuu%5ETx_i%5C%7C%5E2-2%3Cx_i%2Cuu%5ETx_i%3E%3Dx_i%5ETx_i%2B%28u%5ETu-2%29x_i%5ETuu%5ETx_i)  ![img](http://latex.codecogs.com/svg.latex?u:%5Carg%5Cmin %5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En x_ix_i%5ET+%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%28u%5ETu-2%29u%5ETx_ix_i%5ETu)
	5. Add the **norm constraint**, ![img](http://latex.codecogs.com/svg.latex?%5C%7Cu%5C%7C%3D1,u:%5Carg%5Cmax%20%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%20u%5ETx_ix_i%5ETu%3D%5Cfrac%7B1%7D%7Bn%7Du%5ETXX%5ETu,X%5Cin%20k%5Ctimes%20n)
	6. using Lagrange inequation to solve this optimization problem ![img](http://latex.codecogs.com/svg.latex?L%28u%2C%5Clambda%29%3Du%5ET%5CSigma%20u%2B%5Clambda%3Cu%2Cu%3E,%5CSigma%5Cin%20k%5Ctimes%20k), we get ![img](http://latex.codecogs.com/svg.latex?%5CSigma%20u%3D%5Clambda%20u)

### Calculation

- why does it correspond to the eigen-value/vector?
	
	- from the solution process of Lagrange constraint. 
	
- what is the influence of the magnitude of eigenvalue?

	- in the goal: ![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%5C%7C%28I-uu%5ET%29x_i%5C%7C%5E2%3D%20%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%20x_ix_i%5ET-%5Cfrac%7B1%7D%7Bn%7Du%5ETXX%5ETu%3D%20%5Cfrac%7B1%7D%7Bn%7D%5CSigma_%7Bi%3D1%7D%5En%20x_ix_i%5ET-%5Cfrac%7B1%7D%7Bn%7D%5Clambda)
	- so, the larger the eigenvalue is, the smaller the goal will be. Also, since all the eigenvalues are non-negative, this goal will become smaller and smaller with more eigenvalues included.
	
- is it expensive to do PCA decomposition?

- The reconstruction error (using Frobenius norm) of PCA ![img](http://latex.codecogs.com/svg.latex?err%3D%5CSigma_%7Bi=K+1%7D%5EM%20%5Clambda_i), **Proof**:

  ![img](./imgs/2pca_1.png)
### Iterative View

This view provides a method to calculate the main direction once at a time, then remove this direction to find the second best direction.

- why the 1st principle eigenvalue of the residual variance matrix is the 2nd principle eigenvalue of the original variance matrix?
	- think of the diagonal format of the matrix, cut the first diagonal entry from the diagonal matrix, the left diagonal matrix corresponds to that of the residual variance matrix. Therefore, the biggest of this new matrix is the "second" (also, could be the same) of the old one.

### Matrix View

This view gives the mathematical proof and solution of the problem, but not says how to do the calculation, which may cost a lot.

- are all the eigen-vectors orthogonal to each other?
	- Yes. The orthogonality comes from two part:
	1. the eigen-vectors with different eigen-values, a **theorem** guarantees they are orthogonal. *Theorem*: Distinct eigenvalues of **symmetric** matrices have orthogonal eigenvectors. 
	2. the eigen-vectors with the same eigen-value, you can always **do orthogonalization** on them and make them orthogonal.
- **Spectral Theorem** guarantees the **orthogonal decomposition** for **symmetric** matrix. Matrix is **diagonalizable** by an **orthogonal matrix** if and only if it is **symmetric**. 

### relationships
- to linear autoencoder: ![img](http://latex.codecogs.com/svg.latex?C%3DU_k%5ET%5Cin%20k%5Ctimes%20m%2C%20D%3DU_k%5Cin%20m%5Ctimes%20k). If all eigenvalues are different, PCA is **unique and interpretable !!**
- from SVD: 
  - Variance matrix:![img](http://latex.codecogs.com/svg.latex?A%3DUDV%5ET%2C%5CSigma%3DAA%5ET%3D%20UDV%5ET%20VD%5ETU%5ET%3DUD%5E2U%5ET%3DU%5CLambda%20U%5ET,%5CSigma%5Cin%20k%5Ctimes%20k)
  - Similarity matrix: ![img](http://latex.codecogs.com/svg.latex?A%5ETA%20%3DV%5CLambda%20V%5ET%5Cin%20n%5Ctimes%20n)
- to SVD:
	- **U**: eigenvectors of  ![img](http://latex.codecogs.com/svg.latex?AA%5ET)
	- **V**: eigenvectors of  ![img](http://latex.codecogs.com/svg.latex?A%5ETA)
### Solution Algorithm

- power method (for iterative view):
  - ![img](http://latex.codecogs.com/svg.latex?v_%7Bt%2B1%7D%3D%5Cfrac%7BAv_t%7D%7B%5C%7CAv_t%5C%7C%7D,%5Clim_%7Bt%5Cto%5Cinfty%7Dv_t%3Du_1)
  - constraint: ![img](http://latex.codecogs.com/svg.latex?%3Cu_1%2Cv_0%3E%5Cneq%200%2C%5Clambda_1%3E%5Clambda_j%20%5Cforall%20j%3E1)

- from SVD (for matrix view): good for mid-sized problems.

### Implementation
1. Calculate the empirical mean.![img](http://latex.codecogs.com/svg.latex?%5Cbar%7Bx%7D%3D%5Cfrac%7B1%7D%7BN%7D%5CSigma_%7Bn=1%7D%5ENx_n)
2. Center the data. ![img](http://latex.codecogs.com/svg.latex?%5Cbar%7BX%7D%20%3DX-M)
3. Compute the covariance matrix. ![img](http://latex.codecogs.com/svg.latex?%5CSigma%3D%5Cfrac%7B1%7D%7BN%7D%5Cbar%7BX%7D%5Cbar%7BX%7D%5ET)
   1. The difference between the covariance matrix of the ![img](http://latex.codecogs.com/svg.latex?XX%5ET) and  of![img](http://latex.codecogs.com/svg.latex?%5Cbar%7BX%7D%5Cbar%7BX%7D%5ET) is ![img](http://latex.codecogs.com/svg.latex?%5Cbar%7BX%7D%5Cbar%7BX%7D%5ET%20%3DXX%5ET-M%5Cdot%20X%5ET)

4. Eigenvalue Decomposition.
5. Model Selection, pick ![img](http://latex.codecogs.com/svg.latex?K%5Cleq%20D) and keep the projections associated with the top ![img](http://latex.codecogs.com/svg.latex?K) eigenvalues. 
6. Transform the data onto the new basis of ![img](http://latex.codecogs.com/svg.latex?K) dimensions. ![img](http://latex.codecogs.com/svg.latex?%5Cbar%7BZ%7D%3DU_K%5ET%5Cbar%7BX%7D)
7. Reconstruction.
   1.  ![img](http://latex.codecogs.com/svg.latex?%5Ctilt%7B%5Cbar%7BX%7D%7D%3DU_K%5Cbar%7BZ%7D)
   2. ![img](http://latex.codecogs.com/svg.latex?%5Ctilt%7BX%7D%3D%5Ctilt%7B%5Cbar%7BX%7D%7D+M)