---
title: Representation of Polyhedra
date: 2020-10-22
tags: Mathematical Optimization
categories: course notes
---

# Concepts

## Convex hull

For $X\subseteq\mathbb{R}^n$, a convex hull is the smallest convex set that contains $X$.

## cone

cone is a set which is closed under conical combination.

Polyhedral cone:  the cone is a polyhedron:

- Finite intersection of half-spaces
- convex

# Properties

## Proposition 1.36

(The formula of a polyhedral con)
$$
C=\{x\in\mathbb{R}^n:Ax\leq 0\}
$$
Proof from formula to polyhedral con is easy.

From polyhedral con to the formula:

Since it is a polyhedron, we have $C=\{x\in\mathbb{R}^n: Ax\leq b\}$

$0\in C\Rightarrow A\cdot 0 = 0\leq b\Rightarrow b\geq 0$

For any non-redundant constraint, $\exists y\in C, s.t. a^\top y=\beta$

Since it is a cone $\Rightarrow 2y\in C\Rightarrow a^\top 2y = 2\beta$

Since $2y$ in the polyhedron, we have $2\beta\leq \beta\Rightarrow \beta\leq 0$

## Proposition 1.38

$$
P = Q+C:=\{q+c, q\in Q, c\in C\}
$$

- C: the polyhedral cone is unique
- Q: the polyhedron may not be unique.

## Proposition 1.40

It says a shift of a polyhedron is still a polyhedron.

- [x] To prove proposition 1.40, why don't we directly use that $Ax\leq b$, so that $ABx\leq Bb$? The latter inequality is a definition of polyhedron

  The reason is that: $P=\{x\in\mathbb{R}^n: Ax\leq b\}$, and we have a map $\phi: \mathbb{R}^n \to\mathbb{R}^m$ and it is expressed by $\phi(x)=Bx$.

  Since $B\in\mathbb{R}^{m\times n}$, what we are interested in is $\{Bx: x\in\mathbb{R}^n, Ax\leq b\}=\phi(P)$, not $ABx\leq Bb$.

  The problem is that B is not necessarily full-rank. You can really go from lower dimensional space.

  - [ ] :question: and then?

For the proof plan, suppose we already have proposition 1.38.

The plan is: we gonna show that:

- the affine map of a polytope, $\phi(Q)$ , is a polytope
- the affine map of a polyhedral cone, $\phi(C)$, is a polyhedral cone.

Q is a polytope $\Leftrightarrow Q = conv(vertices(Q))$ and $vertices(Q)$ is finite.

So, we gonna show the vertices set of $\phi(Q)$ is finite. We can use the expression $conv(X)=\sum_{i=1}^K \lambda_i x_i$ and the linearity of $\phi$

## Proposition 1.42

- [x] what is the dominant of $P$?
  $$
  dom(P) = P + \mathbb{R}_{\geq 0}^n
  $$

Since $P$ is a polyhedron, it can be written as $P = Q+C$, so $dom(P) = Q + C + \mathbb{R}_{\geq 0}^n$

If we can show $C + \mathbb{R}_{\geq 0}^n$ is a polyhedral cone, then we are done.

We can define a new generator, by the union of the generators of $C$ and $\mathbb{R}_{\geq 0}^n$, according to Proposition 1.37



