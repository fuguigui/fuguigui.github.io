---
title: Matrix Completion
date: 2019-07-05
tags: [computational intelligence]
categories: course notes
mathjax: true
---


# Matrix Completion

There are two direction of consideration to complete the matrix with many missing values:

- statistical models: infer missing entries from observed ones, with k<<m*n parameters
- low rank decomposition: find best approximation with low rank r. Parameter numbers: m*r+n*r=(m+n)r.

## Objective

The key problem of this slide is:
$$
\min_{rank(B)=k}[\sum_{(i,j)\in I}(a_{ij}-b_{ij})^2], I = \{(i,j): \text{observed}\}
$$

### Non-convex

A non-convex problem can be non-convex on two parts: domain or objective.

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
- for fixed $U$, $f$ is convex in $V$
- for fixed $V$, $f$ is convex in $U$

#### Steps

- $U:=\arg\min_U f(U,V)$
- $u_i=(\sum_{j:(i,j)\in I}v_jv_j^\top+\lambda I_k)^{-1}\sum_{j:(i,j)\in I}a_{ij}v_j$
- $V:=\arg\min_V f(U,V)$
- $v_j = (\sum_{i:(i,j)\in I} u_i u_i^\top+\lambda I_k)^{-1}\sum_{i:(i,j)\in I} a_{ij} u_i$
- repeat until convergence.

Applied in CF problem:

- given low-dimensional representations for items/users: compute for each user/item **independently** the best representation
- we learn low-dimensional representations of users and items in the same space: R^K.

### Frobenius Norm Regularization

- Why do we regularize the norm? 
  - to penalize the matrices. Preventing entries of $u_i, v_j$ from becoming too large. 
- What is the consideration/advantage to do this?
  - doing this is not for solving the non-convex problem but making the predicted result more general. Reducing overfitting.
- what is the disadvantage of doing this?
	- the objective is no longer convex.
	- alternating least squares can be used.

### Convex Relaxation

#### Nuclear Norm

- what is the definition of nuclear norm?
  
  - $\|A\|_* = \sum_i\sigma_i$, $\sigma_i$ is the singular value of $A$.
  
- what is the property of nuclear norm?
	- compared to Frobenius norm: if we define $\sigma(A)=(\sigma_1,\sigma_2,\cdots,\sigma_n)$, $\|A\|_* =\text{trace}(\sqrt{A^*A})=\|\sigma(A)\|_1$, $\|A\|_F = \text{trace}(A^*A)=\|\sigma(A)\|_2$
	- compared to rank, it provides the **tightest** lower **convex  bound** of the rank: $\text{trace}(B)\geq \|B\|_*, \forall \|B\|_2\leq 1$ 
	  - Proof:![img](./imgs/2nn_4.png)
	  - does the condition mean we need to standardize **B** s.t. the 2-norm less than 1.
	- convexity. Proof ![img](./imgs/2nn_1.png) ![img](./imgs/2nn_2.png) ![img](./imgs/2nn_3.png)
	
- what is the purpose of using nuclear norm here?
  
  - to relax the rank constraint from a non-convex space to a convex space.
  
- what benefit does nuclear norm bring to us?

  - we can reconstruct the original non-convex problem. In fact, there are several ways to reconstruct the problem:

    - Exact reconstruction (by virtue of Boolean matrix $G$): 
  $$
      \min_B\|B\|_*, s.t. \|A-B\|_G=0
  $$
  
- Approximate reconstruction (directly relaxing the rank constraint, it enlarges the possible solution space)
      $$
      \min_B\|A-B\|_G, s.t. \|B\|_*\leq r
      $$
  
    - Lagrange Formulation
      $$
      \min_B[\frac{1}{2\tau}\|A-B\|_G^2 + \|B\|_*]
      $$
  

### SVD Shrinkage Iterations

$$
B_{t+1} = B_t+\eta_t\Pi(A-\text{shrink}_\tau(B_t))
$$

- what is the basic idea of this method?

  - It wants to 
    - find a matrix with low Frobenius norm and nuclear norm  $B^*=\arg\min_B[\frac{1}{2\tau}\|B\|_F^2+\|B\|_*]$
    - while keeping the observed ones being the same. Keep attention to that  $s.t. \Pi(A-B)=0$
    
  - include a shrinkage operator
    $$
    B^* = \text{shrink}_\tau(A):=\arg\min_B\{\frac{1}{2}\|A-B\|_F^2+\tau\|B\|_*\}
    $$

  -  The solution is  $B^*=UD_\tau V^\top, D_\tau=\text{diag}(\max\{0,\sigma_i-\tau\})$
    This substracts a little from the big singular values and make the small ones as 0. 
    
  - iteratively update the $B$.
  
- get the limitation of the shrinkage of $B_t$, which is $B^*$ we want.
  
- where is this method better than others?

  - it constraints the combination of the two norms: Frobenius norm and nuclear norm.

- how to guarantee the sparsity?

  - using the projection. Because the first $B_1$ only has entries on the observed locations, and each time we add a new matrix projected by the Pi, so all the $B$ matrices are sparse and only have entries on the observed locations.

### Exact Matrix Recovery

This method is more **statistical**.

$$
\min_X\text{rank}(X), s.t. \|A-B\|_I = 0
$$
The rank function is not convex, also the constraint is very stringent. Therefore, it is a difficult problem.

Solutions:

- using nuclear norm to replace the rank as objective,
  - which is not only convex,
  - but the largest convex function less than rank (the best)

- How many entries do we need to guess others? Certain number of known entries reveal the probability of get the right ideal of the whole matrix. The number is almost **linear** to $n$, assuming $m$ is closed to $n$. 

  