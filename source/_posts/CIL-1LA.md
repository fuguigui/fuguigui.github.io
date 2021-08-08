---
title: Linear Autoencoder
author: Lulu
date: 2019-07-04
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
---

# Pre-Questions
1. *What?* The framework of linear matrix low rank construction
2. *Problem?* To approximate a matrix with low-rank matrix. This is an encoding problem.
3. *Key?* Find the encoder $C$ and decoder $D$.
4. Formula? $Z = CX, X' = DZ=DCX$
5. Applications? feature engineering, saving storage space.
6. Further Development? Non-linear? Supervized, not auto?

- Dimension Reduction
- Linear Autoencoder
- Singular Value Decomposition
- Remarks

# Dimension Reduction
- *Object?* A matrix or a data point? what kinds of conditions should this matrix satisfy?
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

$$
\mathbb{R}^{N\times M}\Rightarrow \mathbb{R}^{N\times K}
$$

## Method
The methods should be selected according to the to-encode objects.
### Change Basis/Space
The original vector is expressed on one set of units, change the expression units, and express the vector by new units. Take picture for an example, a $100\times 100$ head photo, the original unit is pixel, now we change the units as four layers, a fixed head, the hair contour, part highlight one and part highlight two. Therefore a $100\times 100$ vector in the original space is expressed by $1\times 4$ vector in new space. 
In another word, the new basis may save a lot of information for the data. Problem: the data points must be similar in some way to make basis useful. 

### Linear

$$
Z=C\cdot X, C\in\mathbb{R}^{K\times M}
$$
### Non-linear
- what is the limitation and advantage of linear?
- what typical techniques in non-linear form?

### Unsupervised

- how to do it?

  That is the source of **auto** of in the name of linear autoencoder.

# Linear Autoencoder

Find a decoder matrix $D$ working on $Z$, which makes $DZ$ as close as possible to $X$.

## Idea

1. $$
   Z=C\cdot X
   $$

   

2. $$
   X' = D\cdot Z = D\cdot C\cdot X
   $$

   

3. Choose $D$ and $C$ to make $X'$ closed to $X$.

## Norms

There are many norms to evaluate how $X'$ is closed to $X$. In the slides, we use Frobenius Norm. 

- How about using other norms? 
- What is best solution under other norms?

### Frobenius Norm

- vector: like the traditional norm definition

- matrix:  generate the definition of vector, the squared root of the sum of the square of each entry.  

  - In a calculation format $\sqrt{\sum_{ij}a_{ij}^2} = \sqrt{\sum_i \sigma_i^2}$
  
  - in a matrix format, depends only on singular values of $A$.
  
  $$
    \|A\|_F^2 = \text{trace}(A^* A) = \text{trace}(AA^*)=\sum_i \sigma_i^2
  $$
  

### p-norm

$$
\|A\|_p:=\sup\{\|Ax\|_p: \|x\|_p = 1\}, \|x\|_p:=(\sum_i\|x_i\|^p)^{1/p}
$$

### Spectral norm (the matrix version of Euclidean norm)
That is 2-norm, when $p$ is 2.
$$
\|A\|_2:=\sup\{\|Ax\|_2:\|x\|_2=1\}=\sigma_1 = \sqrt{\lambda_\max(AA^\top)}
$$

- $\sigma$ refers to the singular value.

## Key Theorem: Eckart-Young Theorem

 - **SVD** is the best approximation matrix with the rank $k$, under the Frobenius Norm (also under the Euclidean norm)

 - the minimum Frobenius (Euclidean) norm loss is the sum of square of singular values from $k+1$.
$$
   J(\theta)=\sum_{l=k+1}\sigma_l^2
$$

## Singular Vector Decomposition

Linear Autoencoder provides a framework, and SVD is one method to get the $D$ and $C$. In SVD, $D = U_k$ and $C=U_k^\top$ . To use SVD, the future to-be-predicted items must have been included in the matrix. No on-the-fly new prediction or no new data points can be added just for prediction, but not being trained.

**Any matrix** can do SVD.

### Formula

$$
X = U\cdot D\cdot V^\top
$$

- $U$: orthogonal matrix
- $V$: orthogonal matrix
- $D$: diagonal matrix

### Interpretation

- $U: M\times M$​​, map the $M$​​ objects to $M$​​ classes, the entry $U_{ij}$​​ is the object $i$​​ 's value on class $j$​​. 


- $V: N\times N$​​, map the $N$​​ objects to $N$​​ classes, the entry $V_{ij}$​​ is the object $i$​​ 's value on class $j$​​

- $D: D_{ii}$ express the strength or expression ability of the class $i$ or the strength of each factor.

### SGD
Stochastic gradient descending method.
- why to use this method? Any problem of solving the SVD directly?
  
  - it is used for CF. It optimizes only **over known ratings** (no need to input missing values). It tries to find low rank approximation $Q$ and $P$ for $X$, let
    $$
    X=Q\cdot P, X\in\mathbb{R}^{M\times N}, Q\in\mathbb{R}^{M\times K}, P\in\mathbb{R}^{K\times N}
    $$

### Relationships
- to linear autoencoder: $C = U_k^\top, D=U_k$ or $C=AU_k^\top, D=U_kA^{-1}$
- SGD UV forms to the original UDV forms
## Principle Component Analysis

- why we use PCA?
	- compared to SVD, which aims to minimize the **norm (Frobenius/Euclidean) ** of the whole **difference matrix**  $X-X'$ directly, PCA focuses on the "inner structure" of the matrix $X$, trying to consume the **variance** among $x_i$'s as large as possible.
- what is the advantage of PCA? 

- what is the basic idea of PCA?
	- works on the **variance matrix** $AA^\top\in\mathbb{R}^{M\times M}$, not the original matrix $A\in\mathbb{R}^{M\times N}$.
	- minimize **Euclidean distance**
	- in other words, we want to find the direction where the variance is at the biggest (Therefore, this direction captures the change of the data at most, the data vary less on other directions.)
  - important **!!!**: direction of **smallest reconstruction error** <-> direction of **largest data variance** 

- what is relation to the **orthogonal projection**?
	- orthogonal projection corresponds to **euclidean** distance, which is the minimization goal of PCA.
	- the **eigenvalue** can be interpreted as the **variance** in the dimension specified by the corresponding eigenvector.

### Key Formula

- $<v,u>u=(uu^\top)v$,  means the projection of $v$ to the direction $u$.

- $(I-uu^\top)v=v-<v,u>u$ means the orthogonal branch of $v$ to $u$.

- Goal: find 
	$$
	(u,\mu)\to\arg\min[\frac{1}{n}\sum_{i=1}^n]\|(I-uu^\top)(x_i-\mu)\|^2
	$$
	
	1. it is easy to get $\mu=\frac{\sum_i x_i}{n}$, that means **to use PCA, we should center the data**
	
	2. the goal becomes: 
	   $$
	   u: \arg\min[\frac{1}{n}\sum_{i=1}^n \|(I-uu^\top)x_i'\|^2], x_i'=x_i-\frac{1}{n}\sum_{j=1}x_j
	   $$
	
	3. Helper formula: 
	   $$
	   \|v-w\|^2 = <v-w, v-w>=\|v\|^2+\|w\|^2-2<v,w>
	   $$
	   
	4. $$\to \|I-(uu^\top)x_i\|^2=\|x_i - uu^\top x_i\|^2=\|x_i\|^2 + \|uu\top x_i\|^2-2<x_i,uu\top x_i>=x_i^\top x_i+(u^\top u-2)x_i^\top uu^\top x_i$$​
	
	5. $$
	   u:\arg\min\frac{1}{n}\sum_{i=1}^n x_ix_i^\top + \frac{1}{n}\sum_{i=1}^n(u^\top u-2)u^\top x_ix_i^\top u
	   $$
	
	6. Add the **norm constraint**, 
	
	7. $$
	   \|u\|=1, u=\arg\max\frac{1}{n}\sum_{i=1}^nu^\top x_ix_i^\top u=\frac{1}{n}u^\top XX^\top u, X\in\mathbb{R}^{k\times n}
	   $$
	
	8. using Lagrange inequation to solve this optimization problem $L(u,\lambda)=u^\top\Sigma u+\lambda<u,u>, \Sigma\in\mathbb{R}^{k\times k}$, we get $\Sigma u = \lambda u$

### Calculation

- why does it correspond to the eigen-value/vector?
	
	- from the solution process of Lagrange constraint. 
	
- what is the influence of the magnitude of eigenvalue?

	- in the goal: 
		$$
	  \frac{1}{n}\sum_{i=1}^n\|(I-uu^\top)x_i\|^2=\frac{1}{n}\sum_{i=1}^n x_ix_i^\top-\frac{1}{n}u^\top XX^\top u=\frac{1}{n}\sum_{i=1}^n x_ix_i^\top-\frac{\lambda}{n}
	  $$
	
	- so, the larger the eigenvalue is, the smaller the goal will be. Also, since all the eigenvalues are non-negative, this goal will become smaller and smaller with more eigenvalues included.
	
- is it expensive to do PCA decomposition?

- The reconstruction error (using Frobenius norm) of PCA $err=\sum_{i=K+1}^M\lambda_i$​ 

  Proof:
  $$
  \begin{align}
  
   err & = \frac{1}{N}\|\tilde{\bar{X}} - \bar{X}\|_F^2 = \frac{1}{N}\|(U_KU_K^\top - I_d)\bar{X}\|_F^2\\
  & =\frac{1}{N}\text{trace}[(U_KU_K^\top - I_d)\cdot \bar{X}\bar{X}^\top \cdot (U_K U_K^\top - I_d)^\top] \\
  & = \text{trace}[(U_KU_K^\top - I_d)\cdot \Sigma \cdot (U_K U_K^\top - I_d)]\\
  & = \text{trace}[(U_KU_K^\top - I_d)\cdot U\Lambda U^\top \cdot (U_K U_K^\top - I_d)] \\
  & = \text{trace}[(U_KU_K^\top U - U)\cdot \Lambda \cdot (U^\top U_K U_K^\top - U^\top)] \\
  & = \text{trace}[([U_K; 0] - U)\Lambda([U_K; 0] - U)^\top] \\
  & = \text{trace}(\sum_{i=K+1}^D \lambda_i u_i u_i^\top) = \sum_{i=K+1}^D \lambda_i\cdot \text{trace}(u_i u_i^\top)\\
  & \overset{\text{unit norm}}{=}\sum_{i=K+1}^D \lambda_i 
  \end{align}
  $$
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
- to linear autoencoder: $C=U_k^\top\in\mathbb{R}^{k\times m}, D=U_k\in\mathbb{R}^{m\times k}$,  If all eigenvalues are different, PCA is **unique and interpretable !!**
- from SVD: 
  - Variance matrix:
    $$
    A=UDV^\top,\Sigma=AA^\top=UDV^\top VD^\top U^\top=UD^2U^\top=U\Gamma U^\top, \Sigma\in\mathbb{R}^{k\times k}
    $$
  
  - Similarity matrix: $A^\top A=V\Lambda V^\top\in\mathbb{R}^{n\times n}$
- to SVD:
	- **U**: eigenvectors of  $AA^\top$
	- **V**: eigenvectors of $A^\top A$ 
### Solution Algorithm

- power method (for iterative view):
  
  - $$
  v_{t+1}=\frac{Av_t}{\|Av_t\|},\lim_{t\to\infty}v_t=u_1
    $$
  
  - constraint: $<u_1,v_0>\neq 0,\lambda_1>\lambda_j, \forall j>1$ 
  
- from SVD (for matrix view): good for mid-sized problems.

### Implementation
1. Calculate the empirical mean $\bar{x}=\frac{\sum_{n=1}^N x_n}{N}$.

2. Center the data $\bar{X}=X-M$.

3. Compute the covariance matrix $\Sigma=\frac{1}{N}\bar{X}\bar{X}^\top$

   The difference between the covariance matrix of the $XX^\top$  and  that of $\bar{X}\bar{X}^\top$is 
   $$
   \bar{X}\bar{X}^\top = XX^\top - MX^\top
   $$

4. Eigenvalue Decomposition.

5. Model Selection, pick $K\leq D$  and keep the projections associated with the top  $K$ eigenvalues. 

6. Transform the data onto the new basis of $K$ dimensions. 
   $$
   \bar{Z}=U_K^\top\bar{X}
   $$

7. Reconstruction.

   1. $\tilde{\bar{X}}=U_K\bar{Z}$​
   2. $\tilde{X}=\tilde{\bar{X}}+M$