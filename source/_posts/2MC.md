---
title: Matrix Completion
date: 2019-07-05
tags: [courses, machine learning, data analysis]
categories: Computational Intelligence
---


# Matrix Completion

There are two direction of consideration to complete the matrix with many missing values:

- statistical models: infer missing entries from observed ones, with k<<m*n parameters
- low rank decomposition: find best approximation with low rank r. Parameter numbers: m*r+n*r=(m+n)r.

## Objective

The key problem of this slide is:

![img](http://latex.codecogs.com/svg.latex?%5Cmin_%7Brank%28B%29%3Dk%7D%5B%5CSigma_%7B%28i%2Cj%29%5Cin%20I%7D%28a_%7Bij%7D-b_%7Bij%7D%29%5E2%5D%2CI%20%3D%5C%7B%28i%2Cj%29%3Aobserved%5C%7D)

### Non-convex

A non-convex problem can be non-convex on two parts" domain or objective.

Finding the best approximation matrix B with the rank k is not a convex problem:

- the objective: minimize the frobenius norm is convex
- the searching space: matrix with rank k is not convex. The sum of two matrices with the same rank may have different rank.

Why we don't like non-convex problem?

- because non-convex problem usually has higher variance, or leads to local minima, not the global one, depending on the input data.

### Remark on SVD

- Is SVD the optimal solution of the low-rank approximation even it is a non-convex problem?
  - An answer from [mathoverflow](https://mathoverflow.net/questions/32533/is-all-non-convex-optimization-heuristic) "The classical case is the singular value decomposition (SVD) which is non-convex but yet solvable. This is because only the top eigenvector is the global and local optimum and all the other eigenvectors are saddle points (assuming eigen gap). "
- Why SVD is not enough? 
  - Because we have a large amount of unobserved entries! In another way, we want to find a matrix B according to the matrix A, but A is not good enough.



## Solutions

### Alternating method

This is a general method for non-convex problem with something convex.
- for fixed **U**, f is convex in **V**
- for fixed **V**, f is convex in **U**

#### Steps

- ![img](http://latex.codecogs.com/svg.latex?U%3A%3D%5Carg%5Cmin_%7BU%7Df%28U%2CV%29), ![img](http://latex.codecogs.com/svg.latex?u_i%20%3D%20%28%5CSigma_%7Bj%3A%28i%2Cj%29%5Cin%20I%7Dv_jv_j%5ET%2B%5Clambda%20I_k%29%5E%7B-1%7D%20%5CSigma_%7Bj%3A%28i%2Cj%29%5Cin%20I%7Da_%7Bij%7Dv_j)
- ![img](http://latex.codecogs.com/svg.latex?V%3A%3D%5Carg%5Cmin_%7BV%7Df%28U%2CV%29), ![img](http://latex.codecogs.com/svg.latex?v_j%20%3D%20%28%5CSigma_%7Bi%3A%28i%2Cj%29%5Cin%20I%7Du_iu_i%5ET%2B%5Clambda%20I_k%29%5E%7B-1%7D%20%5CSigma_%7Bi%3A%28i%2Cj%29%5Cin%20I%7Da_%7Bij%7Du_i)
- repeat until convergence.

Applied in CF problem:

- given low-dimensional representations for items/users: compute for each user/item **independently** the best representation
- we learn low-dimensional representations of users and items in the same space: R^K.

### Frobenius Norm Regularization

- Why do we regularize the norm? 
  - to penalize the matrices. Preventing entries of u_i, v_j from becoming too large. 
- What is the consideration/advantage to do this?
  - doing this is not for solving the non-convex problem but making the predicted result more general. Reducing overfitting.
- what is the disadvantage of doing this?
	- the objective is no longer convex.
	- alternating least squares can be used.

### Convex Relaxation

#### Nuclear Norm

- what is the definition of nuclear norm?
  
  - ![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_%2A%20%3D%20%5CSigma_%7Bi%3D1%7D%5Csigma_i) , ![img](http://latex.codecogs.com/svg.latex?%5Csigma_i) is the singular value of A.
  
- what is the property of nuclear norm?
	- compared to Frobenius norm: if we define ![img](http://latex.codecogs.com/svg.latex?%5Csigma%28A%29%3D%28%5Csigma_1%2C%5Csigma_2%2C%5Ccdots%2C%5Csigma_n%29), ![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_%2A%20%3Dtrace%28%5Csqrt%7BA%5E*A%7D%29%3D%5C%7C%5Csigma%28A%29%5C%7C_1),![img](http://latex.codecogs.com/svg.latex?%5C%7CA%5C%7C_F%3Dtrace%28A%5E*A%29%3D%5C%7C%5Csigma%28A%29%5C%7C_2)
	- compared to rank, it provides the **tightest** lower **convex  bound** of the rank:![img](http://latex.codecogs.com/svg.latex?rank%28B%29%5Cgeq%5C%7CB%5C%7C_%2A%2C%20%5Cforall%20%5C%7CB%5C%7C_2%5Cleq1) 
	  - Proof:![img](./imgs/2nn_4.png)
	  - does the condition mean we need to standardize **B** s.t. the 2-norm less than 1.
	- convexity. Proof ![img](./imgs/2nn_1.png) ![img](./imgs/2nn_2.png) ![img](./imgs/2nn_3.png)
	
- what is the purpose of using nuclear norm here?
  
  - to relax the rank constraint from a non-convex space to a convex space.
  
- what benefit does nuclear norm bring to us?

  - we can reconstruct the original non-convex problem. In fact, there are several ways to reconstruct the problem:

    - Exact reconstruction (by virtue of Boolean matrix G):![img](http://latex.codecogs.com/svg.latex?%5Cmin_B%5C%7CB%5C%7C_%2A%2C%20s.t.%20%5C%7CA-B%5C%7C_G%3D0)

    - Approximate reconstruction (directly relaxing the rank constraint, it enlarges the possible solution space)![img](http://latex.codecogs.com/svg.latex?%5Cmin_B%20%5C%7CA-B%5C%7C_G%2C s.t.%5C%7CB%5C%7C_%2A%2C%20%5Cleq r)

    -  Lagrange Formulation![img](http://latex.codecogs.com/svg.latex?%5Cmin_B%5B%5Cfrac%7B1%7D%7B2%5Ctau%7D%5C%7CA-B%5C%7C_G%5E2%2B%5C%7CB%5C%7C_%2A%5D)

      

### SVD Shrinkage Iterations

![img](http://latex.codecogs.com/svg.latex?B_%7Bt%2B1%7D%3DB_t%2B%5Ceta_t%5CPi%28A-shrink_%5Ctau%28B_t%29%29)

- what is the basic idea of this method?

  - It wants to 
    - find a matrix with low Frobenius norm and nuclear norm  ![img](http://latex.codecogs.com/svg.latex?B%5E%2A%3D%5Carg%5Cmin_B%5B%5Cfrac%7B1%7D%7B2%5Ctau%7D%5C%7CB%5C%7C_F%5E2%2B%5C%7CB%5C%7C_%2A%5D)
    - while keeping the observed ones being the same. Keep attention to that  ![img](http://latex.codecogs.com/svg.latex?s.t. %5CPi%28A-B%29%3D0)
  - include a shrinkage operator. ![img](http://latex.codecogs.com/svg.latex?B%5E%2A%3Dshrink_%7B%5Ctau%7D%28A%29%3A%3D%5Carg%5Cmin_B%5C%7B%5Cfrac%7B1%7D%7B2%7D%5C%7CA-B%5C%7C_F%5E2%2B%5Ctau%5C%7CB%5C%7C_%2A%5C%7D). The solution is ![img](http://latex.codecogs.com/svg.latex?B%5E%2A%3DUD_%7B%5Ctau%7DV%5ET%2C%20D_%7B%5Ctau%7D%3Ddiag%28%5Cmax%5C%7B0%2C%5Csigma_i-%5Ctau%5C%7D%29) This substracts a little from the big singular values and make the small ones as 0. 
  - iteratively update the B.
  - get the limitation of the shrinkage of B_t, which is B* we want.

- where is this method better than others?

  - it constraints the combination of the two norms: Frobenius norm and nuclear norm.

- how to guarantee the sparsity?

  - using the projection. Because the first B_1 only has entries on the observed locations, and each time we add a new matrix projected by the Pi, so all the B matrices are sparse and only have entries on the observed locations.

### Exact Matrix Recovery

This method is more **statistical**.

![img](http://latex.codecogs.com/svg.latex?%5Cmin_Xrank%28X%29%20, s.t.%20%5C%7CA-B%5C%7C_I%3D0)

The rank function is not convex, also the constraint is very stringent. Therefore, it is a difficult problem.

Solutions:

- using nuclear norm to replace the rank as objective,
  - which is not only convex,
  - but the largest convex function less than rank (the best)

- How many entries do we need to guess others? Certain number of known entries reveal the probability of get the right ideal of the whole matrix. The number is almost **linear** to **n**, assuming **m** is closed to **n**. 

  