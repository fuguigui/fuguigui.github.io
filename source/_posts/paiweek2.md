---
title: Probability & Bayesian Networks
date: 2019-10-01
tags: [AI, Probability]
categories: Probabilistic AI
---

![decomp](http://latex.codecogs.com/svg.latex?

## Important points

- Probability Review
  - Joint distribution
    - sum rule (marginalization)
    - product rule (chain rule)
  - Conditional probability
    - conditional independence
- Bayesian Network

## Probability Review

First, please remember that probability is defined on the *sample space and set* !!! It is exactly a map, from sample space to numerical space. The bridge between sample space and numerical space is built by set operations and algebra operations.

It satisfies the following axioms:

1. P(\Omega) = 1
2. If A is a subset of \Omega, P(A)>=0
3. (**operation bridge**) If A1 and A2 are disjoint, P(A1 or A2) = P(A1)+P(A2)

### Conditional probability

#### Definition

![c-def](http://latex.codecogs.com/svg.latex?%5Ctext%7BPr%7D%28A%7CB%29%20%5Ctext%7BPr%7D%28B%29%3D%20%5Ctext%7BPr%7D%28AB%29)

In the following, i gonna use ![p-def](http://latex.codecogs.com/svg.latex?A%5Cperp%20B) to denote A and B are independent.

#### Properties

Referring to the conditional independence. To be proofed. The correctness isn't guaranteed.

- symmetry: ![sym](http://latex.codecogs.com/svg.latex?X%5Cperp%20Y%20%5CLeftrightarrow%20Y%5Cperp%20X)
- decomposition: Given Z, X is conditional independent from Y and W. We can conclude that X is conditional independent from Y and X is conditional independent from W specifically. ![dec](http://latex.codecogs.com/svg.latex?X%5Cperp%20%28Y%2CW%29%20%5CLeftrightarrow%20%28X%5Cperp%20Y%29%20%5Cwedge%20%20%28X%5Cperp%20W%29)
  - Proof (left to right): similar to the conditional independence version.
  - Proof (right to left): I **don't know** how to prove it.
- Contraction: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%5Cwedge%20%28X%5Cperp%20W%7CY%29%5CRightarrow%20X%5Cperp%20%28WY%29)
  - The proof is ![decomp](http://latex.codecogs.com/svg.latex?P%28XWY%29%20%3D%20P%28XW%7CY%29P%28Y%29%3DP%28X%7CY%29P%28W%7CY%29P%28Y%29%3DP%28X%29P%28WY%29)
    - ![cont](http://latex.codecogs.com/svg.latex?X%5Cperp%20W%7CY%5CRightarrow%20P%28XW%7CY%29%20%3D%20P%28X%7CY%29P%28W%7CY%29)
    - ![cont](http://latex.codecogs.com/svg.latex?X%5Cperp%20Y%5CRightarrow%20P%28X%7CY%29%3DP%28X%29)
  - That means if X is independent with W given a condition Y which is independent from X, X is independent from YW 
- weak union: ![decomp](http://latex.codecogs.com/svg.latex?X%5Cperp%20%28YW%29%5CRightarrow%20%28X%5Cperp%20Y%29%7CW)
  - The proof is ![decomp](http://latex.codecogs.com/svg.latex?P%28XY%7CW%29%3D%5Cfrac%7BP%28XYW%29%7D%7BP%28W%29%7D%3D%5Cfrac%7BP%28X%29P%28YW%29%7D%7BP%28W%29%7D%3DP%28X%29P%28Y%7CW%29%3DP%28X%7CW%29P%28Y%7CW%29)
    - ![weak](http://latex.codecogs.com/svg.latex?X%5Cperp%20%28YW%29%5CRightarrow%20P%28XYW%29%3DP%28X%29P%28YW%29)
    - ![weak](http://latex.codecogs.com/svg.latex?P%28XYW%29%3DP%28X%29P%28YW%29%5CRightarrow%20P%28XW%29%3D%5Csum_YP%28XYW%29%3D%5Csum_YP%28X%29P%28YW%29%3DP%28X%29P%28W%29)
  - That means we can separate part of variables as the condition and make the left independent. 
- intersection: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7CW%5Cwedge%20%28X%5Cperp%20W%29%7CY%5CRightarrow%20X%5Cperp%28YW%29)
  - The proof is ![decomp](http://latex.codecogs.com/svg.latex?P%28XYW%29%3DP%28XY%7CW%29P%28W%29%3DP%28X%7CW%29P%28Y%7CW%29P%28W%29%3DP%28X%29P%28YW%29)
    - ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7CW%5Cwedge%20%28X%5Cperp%20W%29%7CY%5CRightarrow%20P%28XYW%29%3DP%28XY%7CW%29P%28W%29%3DP%28X%7CW%29P%28YW%29%3DP%28X%7CY%29P%28YW%29%5CRightarrow%20P%28XW%29P%28Y%29%3DP%28XY%29P%28W%29%5CRightarrow%20%5Csum_YP%28XW%29P%28Y%29%3D%5Csum_YP%28XY%29P%28W%29%5CRightarrow%20P%28XW%29%3DP%28X%29P%28W%29)

### Operations in Probability

#### Operators

- and: ![op-and](http://latex.codecogs.com/svg.latex?%5Ccap%5Ctext%7B%20or%20%7D%2C%20%5Ctext%7B%20or%20missed%7D%2C%20e.g.%20P%28A%5Ccap%20B%29%20%3D%20P%28A%2CB%29%20%3D%20P%28AB%29%20)
- or: ![op-or](http://latex.codecogs.com/svg.latex?%5Ccup,%20e.g.%20P%28A%5Ccup%20B%29,P%28A%5Ccup%20B%29%20%3D%20P%28A%29%20%2B%20P%28B%29-P%28A%5Ccap%20B%29)
- conditional: ![op-cd](http://latex.codecogs.com/svg.latex?%7C,%20e.g.%20P%28A%7C%20B%29%3D%5Cfrac%7BP%28AB%29%7D%7BP%28B%29%7D)
  - this operator is a little bit tricky, cause if considering the sample space, "and" and "or" operators are in the original sample space but "conditional" operator **changes** the sample space !!!
- conditional independence: ![op-c-indep](http://latex.codecogs.com/svg.latex?%5Cperp,e.g.P%28%28A%5Cperp%20B%29%7CC%29%20%3D%20P%28A%7CC%29P%28B%7CC%29)

Priorities: (from lowest to highest): ![op-prior](http://latex.codecogs.com/svg.latex?%7C%20%3C%20%5Cperp%20%3C%20%5Ccap%2C%20%5Ccup)

For example, ![op-ex](http://latex.codecogs.com/svg.latex?P%28A%2CB%7CC%29%20%3D%20P%28%28A%2CB%29%7CC%29%5Cneq%20P%28A%2c%28B%7CC%29%29)

- "and" could be viewed as "multiply" (*) in algebra operations in some way

  - exchangeable: ![op-and](http://latex.codecogs.com/svg.latex?P%28A%5Ccap%20B%29%3D%20P%28B%5Ccap%20A%29)

- "conditional" could be viewed as "plus" (+), always the lowest
  - but it is **not exchangeable**, that means ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7C%20B%29%5Cneq%20P%28B%7CA%29)
  
  - it is **ordered**. (I am not sure which order it exactly is. Just from my own understanding, the order should be from right to left) which means ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7CB%7CC%29%20%3D%20P%28A%7C%28B%7CC%29%29%5Cneq%20P%28%28A%7CB%29%7CC%29)
  
    My explanation is as follows:
  
    - suppose ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7CB%7CC%29%20%3D%20P%28A%7C%28B%7CC%29%29) 
    
      We will have ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7C%28B%7CC%29%29%20%3D%20%5Cfrac%7BP%28A%2C%28B%7CC%29%29%7D%7BP%28B%7CC%29%7D%20%3D%20%5Cfrac%7BP%28%28AB%29%7CC%29%7D%7BP%28B%7CC%29%7D%3D%5Cfrac%7BP%28ABC%29P%28C%29%7D%7BP%28C%29P%28BC%29%7D%3D%5Cfrac%7BP%28ABC%29%7D%7BP%28BC%29%7D%3DP%28A%7C%28BC%29%29)
    
      - the second equality uses the "reducing rule", explained in the next block.
    
      - this directly gives us the conclusion that ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7CB%7CC%29%20%3DP%28A%7C%28BC%29%29)
    
      - generate it to the n cases, we will have 
    
        ![op-cd](http://latex.codecogs.com/svg.latex?P%28A_n%7CA_%7Bn-1%7D%7C%5Ccdots%7CA_1%29%20%3D%20P%28A_n%7C%28A_%7Bn-1%7D%28%5Ccdots%20%7CA_1%29%29%29%20%3D%20P%28A_n%7C%28A_%7Bn-1%7D%5Ccdots%20A_1%29%29)
    
        The proof is ignored here. Using induction will easily prove it.
    
    - suppose ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%7CB%7CC%29%20%3D%20P%28%28A%7CB%29%7CC%29)
    
      We will have ![op-cd](http://latex.codecogs.com/svg.latex?P%28%28A%7CB%29%7CC%29%3D%5Cfrac%7BP%28%28A%7CB%29%2CC%29%7D%7BP%28C%29%7D%3D%5Cfrac%7BP%28C%28A%7CB%29%29%7D%7BP%28C%29%7D%3D%5Cfrac%7BP%28%28AC%29%7CB%29%7D%7BP%28C%29%7D%3D%5Cfrac%7BP%28ABC%29%7D%7BP%28B%29P%28C%29%7D)
    
      - also, the third equality uses the "reducing rule"
      - well, I couldn't find error in the algebra calculation but from the definition of probability, using P(B)P(C) as the denominator, I don't know what the sample space becomes. Based on this aspect, I will refuse this assumption and take the former one.

##### Rules

This part is based on my own deduction, I am not sure if they are true. Please contact me if you have other ideas! I would greatly appreciate it.

- "reducing rule": ![rl-al](http://latex.codecogs.com/svg.latex?P%28A%28B%7CC%29%29%20%3D%20P%28%28AB%29%7CC%29)

  - just like the "conditional" operator, this rule is also tricky, cause A and B|C are in the different sample space. Here, I would reduce the larger sample space to the smaller sample space. That is reducing A to A|C actually.  By doing this, We will have 

    ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%28B%7CC%29%29%3DP%28%28A%7CC%29%28B%7CC%29%29%3DP%28%28AB%29%7CC%29)
  
- "association rule": ![rl-al](http://latex.codecogs.com/svg.latex?P%28A%28BC%29%29%20%3D%20P%28%28AB%29C%29%3DP%28ABC%29)

  - from the set view, the proof is trivial.

  

#### Anti-examples

I found some formulas look to be true but actually not. I will list them and give some brief explanation/proof.

- ![at1](http://latex.codecogs.com/svg.latex?P%28XY%29%20%3D%20P%28X%29P%28Y%29%2C%20P%28XW%29%3DP%28X%29P%28W%29%5CnRightarrow%20P%28%28XY%29W%29%3DP%28XY%29P%28W%29)

  - The key idea is Y and M may not be independent.

  - Otherwise, it means ![decomp](http://latex.codecogs.com/svg.latex?P%28%28XY%29W%29%20%3D%20P%28XY%29%20P%20%28W%29%20%3D%20P%28X%29P%28Y%29P%28W%29%20%3D%20P%28X%29P%28YW%29%20%5CRightarrow%20P%28Y%29P%28W%29%3DP%28YW%29)

    But Y and W are not necessarily independent.

    An anti-example: 

    | x    | 0    | 0    | 0    | 0    | 0    | 0    | 1    | 1    | 1    | 1    | 1    | 1    |
    | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
    | y    | 1    | 1    | 0    | 0    | 0    | 1    | 1    | 1    | 0    | 0    | 0    | 1    |
    | w    | 0    | 0    | 0    | 1    | 1    | 1    | 0    | 0    | 0    | 1    | 1    | 1    |

    ![decomp](http://latex.codecogs.com/svg.latex?%5Cforall%20i%5Cin%5C%7B0%2C1%5C%7D%2C%20j%5Cin%5C%7B0%2C1%5C%7D)

    - ![decomp](http://latex.codecogs.com/svg.latex?P%28X%3Di%2CW%3Dj%29%3D%5Cfrac%7B1%7D%7B4%7D%3D%5Cfrac%7B1%7D%7B2%7D%5Ccdot%5Cfrac%7B1%7D%7B2%7D%3DP%28X%3Di%29P%28W%3Dj%29)

    - ![decomp](http://latex.codecogs.com/svg.latex?P%28X%3Di%2CY%3Dj%29%3D%5Cfrac%7B1%7D%7B4%7D%3D%5Cfrac%7B1%7D%7B2%7D%5Ccdot%5Cfrac%7B1%7D%7B2%7D%3DP%28X%3Di%29P%28Y%3Dj%29)

    - ![decomp](http://latex.codecogs.com/svg.latex?P%28X%3D0%2CY%3D1%2CW%3D1%29%3D%5Cfrac%7B1%7D%7B12%7D%2C%20P%28X%3D0%2CY%3D1%29%20%3D%20%5Cfrac%7B1%7D%7B4%7D%2C%20P%28W%3D1%29%20%3D%20%5Cfrac%7B1%7D%7B2%7D)

- "transitivity rule": ![rl-al](http://latex.codecogs.com/svg.latex?P%28AB%29%3DP%28A%29P%28B%29%5CRightarrow%20%5Cforall%20C%5Csubset%20A%2C%20P%28CB%29%3DP%28C%29P%28B%29)

    - that means if set A and set B are independent from each other, any subset of A will also be independent from B.
    - It looks true, but not. For example, take C=AB, this is a subset of A, P(CB)=P(C)P(B)=P(AB)P(B)=P(ABB)=P(AB), => P(AB)P(B) = P(AB)=>P(B)=1. Of course not.

    

### Conditional Independence

#### Definition

A and B are conditional independence given C ![c-indep](http://latex.codecogs.com/svg.latex?%5Ctext%7BPr%7D%28A%5Ccap%20B%7CC%29%20%3D%20%5Ctext%7BPr%7D%28A%7CC%29%5Ctext%7BPr%7D%28B%7CC%29)

It is denoted as ![c-indep-denote](http://latex.codecogs.com/svg.latex?%28A%5Cperp%20B%29%20%7C%20C)

#### Properties

Claim: all of the following properties should have non-conditional version

- symmetry: ![sym](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7CZ%20%5CLeftrightarrow%20%28Y%5Cperp%20X%29%7CZ)
  
  - The proof is trivial
  
- decomposition: Given Z, X is conditional independent from Y and W. We can conclude that X is conditional independent from Y and X is conditional independent from W specifically. ![dec](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28Y%2CW%29%29%7CZ%20%5CLeftrightarrow%20%28X%5Cperp%20Y%29%7CZ%20%5Cwedge%20%20%28X%5Cperp%20W%29%7CZ)

  - Proof (left to right): Since Y and W are symmetric, I only prove for Y (in discrete version). (The continuous version is similar) ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28Y%2CW%29%29%7CZ%5CLeftrightarrow%20%20P%28%28XYW%29%7CZ%29%3DP%28%28X%28YW%29%29%7CZ%29%20%3D%20P%28X%7CZ%29P%28%28YW%29%7CZ%29)

    ![decomp](http://latex.codecogs.com/svg.latex?%5CRightarrow%20P%28%28XY%29%7CZ%29%20%3D%20%5Csum_W%20P%28%28XYW%29%7CZ%29%20%3D%20%5Csum_W%20P%28X%7CZ%29P%28%28YW%29%7CZ%29%3D%20P%28X%7CZ%29%5Csum_W%20P%28%28YW%29%7CZ%29%20%3D%20P%28X%7CZ%29P%28Y%7CZ%29)

    ![decomp](http://latex.codecogs.com/svg.latex?%5CRightarrow%20%28X%5Cperp%20Y%29%7CZ)

  - Proof (right to left): I **don't know** how to prove it.

- Contraction: ![decomp](http://latex.codecogs.com/svg.latex?%28%28X%5Cperp%20Y%29%7CZ%29%5Cwedge%20%28X%5Cperp%20W%7C%28YZ%29%29%5CRightarrow%20%28X%5Cperp%20%28WY%29%29%7CZ)

- weak union: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28YW%29%29%7CZ%5CRightarrow%20%28X%5Cperp%20Y%29%7C%28ZW%29)

- intersection: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7C%28WZ%29%5Cwedge%20%28X%5Cperp%20W%29%7C%28YZ%29%5CRightarrow%20%28X%5Cperp%28YW%29%29%7CZ)

  

## Bayesian Network

Basically from the book : Artificial Intelligence: A Modern Approach

Questions:

- how to specify a bayesian network
- how to imply CI (conditional independence) given a BN

### Definition

A Bayesian Network is a *directed graph* in which:

- node:
  - each node corresponds to a random variable
  - each node has a conditional probability distribution P(x|parents(x))
- links:
  - a link from node X to node Y, X is said to be a *parent* of Y.
- No directed cycles. (It is a DAG)

### Interpretation

- links: intuitive meaning: X is a *parent* of Y -> X has a *direct impact* on Y. Causes should be parents of effects.

### Semantics

Two ways to understand the semantics of Bayesian Network:

1. as a representation of the joint probability distribution
2. as an encoding of a collection of conditional independence statements.

### Constructing a BN

1. ordering the nodes such that *causes precede effects*. i.e. X_k's causes have index smaller than k.
2. For each Xi:
   1. find the minimal parent subset A from {1,2,...,i-1}, s.t. P(X_i|{1,2,...,i-1})=P(X_i|A)
   2. for each parent in A, insert a link from X_k to X_i 



### Active Trails

This definition is to answer the question *how to identify CI from a BN*

#### Basic structures: BNs with 3-nodes

- x<-y->z: common cause
- x<-y<-z: indirect evidential effect
- x->y->z: indirect causal effect
- x->y<-z: common effect

#### Definition

Active trail: 

- undirected, 
- for the first three paths, y is not observed
- for the last path, y or any its descendants are observed

### d-separation

Given observed variables O, any variables Xi and Xj for which there is**no active trail** are called *d-separated* by O.  Written as **d-sep(Xi;Xj|O) **

Theorem: ![d-sep](http://latex.codecogs.com/svg.latex?%5Ctext%7Bd-sep%7D%28X%3BY%7CZ%29%5CRightarrow%20%28X%5Cperp%20Y%29%7CZ)

- remark:

  - the converse generally does not hold

    .