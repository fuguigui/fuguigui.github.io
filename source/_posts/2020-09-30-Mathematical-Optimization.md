---
title: Concepts--Half-space, polyhedron 
date: 2020-09-30
tags: Mathematical Optimization
categories: course notes
---

![relax](./pic/093001.png)

- [ ] would this replacement add more freedom???
- [ ] what is the complexity of LP problem? the number of constraints? 
- [ ] if using LP to solve LR, how to deal with the enormous number of data points? The complexity of LR wouldn't change a lot with input growing but the LP would, I think.

Canonical Form
$$
\max c^Tx\\
Ax\leq b\\
x\geq 0
$$

- [x] Why make this form as canonical form??? In particular, the non-negative constraint.

  - it is quite convenient. It constraints us to half-closed space.

- [x] Does unbounded LP have unbounded feasible area?

  Yes.

**polyhedron**: the intersection of finitely many **half-spaces** in finite-dimensional Euclidean space

Exercise:
$$
A\text{ is } x, B \text{ is }y.\\
\max 0.72 x + 0.6 y\\
s.t. x + y\leq 20\\
x/16 + y/24\leq 1\\
18x+38y\leq30 (x+y)\\
0.5 x+y\leq 12\\
x\geq 0\\
y\geq 0
$$
# Concepts

## Dimension of a polyhedron

A detailed explanation of the dimension of a polyhedron, which is the smallest dimension of an affine subspace containing P:
$$
\text{dim}(P):=\min\{k\in\mathbb{Z}_{\geq 0}: \exists A\in\mathbb{R}^{n\times n} \text{ with rank}(A) = n-k \cap Ax=Ay, \forall x,y\in P\}
$$
An affine subspace is defined by:
$$
Ax=b
$$
So, an affine subspace containing P means:
$$
\exists A, b\forall x\in P, Ax=b
$$
we use 
$$
\forall x,y\in P, Ax=Ay
$$
to replace the above condition.

Since we want the affine subspace with the smallest dimension, we hope rank(A) as large as possible. In this way, we get the above condition.



## Faces

Empty is also a face.

An empty polyhedron has one face, which is empty.

An one-dimension polyhedron has one facet, which is empty.



### Exercise

shortest s-t path: the objective: maximize is to guarantee that we get the smallest path. Otherwise, the solution is not ideal. Think about the solution with values as 0. It is a feasible solution but obviously, is not what we want.