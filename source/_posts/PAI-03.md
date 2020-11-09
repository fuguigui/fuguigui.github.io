---
title: Bayesian Network 1 - Basic
date: 2020-01-23
tags: [probabilistic artificial intelligence, reinforcement learning]
categories: course notes
---
# Questions
- why to say the conditional independence is weaker than independence?
- what are the properties of conditional independence? How to compare them with independence?
- what is the definition of bayesian network? 
- what is the relation between conditional independence and bayesian network?
- what are the advantages of bayesian network?

# Keywords
conditional independence, naive bayes models, bayesian networks

# Conditional independence

Using independence in formula will greatly decrease the number of parameters from O(2^n)  to some smaller numbers, even O(n) 

## Definition

A and B, two random variables, are conditional independence given C iff for all a,b,c![c-indep](http://latex.codecogs.com/svg.latex?%5Ctext%7BPr%7D%28A%3Da%5Ccap%20B%3Db%7CC%3Dc%29%20%3D%20%5Ctext%7BPr%7D%28A%3Da%7CC%3Dc%29%5Ctext%7BPr%7D%28B%3Db%7CC%3Dc%29)

- If ![indep-orth](http://latex.codecogs.com/svg.latex?%5CPr%5BY%3Dy%7CZ%3Dz%5D%3E0%2C%20%5CPr%5BX%3Dx%7CY%3Dy%2CZ%3Dz%5D%3D%5CPr%5BX%3Dx%7CZ%3Dz%5D)

It is denoted as ![c-indep-denote](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%20%7C%20Z)

## Properties

Claim: all of the following properties should have non-conditional version

- symmetry: ![sym](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7CZ%20%5CLeftrightarrow%20%28Y%5Cperp%20X%29%7CZ)

  - The proof is trivial

- decomposition: Given Z, X is conditional independent from Y and W. We can conclude that X is conditional independent from Y and X is conditional independent from W specifically. ![dec](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28Y%2CW%29%29%7CZ%20%5CLeftrightarrow%20%28X%5Cperp%20Y%29%7CZ%20%5Cwedge%20%20%28X%5Cperp%20W%29%7CZ)

  - Proof (left to right): Since Y and W are symmetric, I only prove for Y (in discrete version). (The continuous version is similar) ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28Y%2CW%29%29%7CZ%5CLeftrightarrow%20%20P%28%28XYW%29%7CZ%29%3DP%28%28X%28YW%29%29%7CZ%29%20%3D%20P%28X%7CZ%29P%28%28YW%29%7CZ%29)

    ![decomp](http://latex.codecogs.com/svg.latex?%5CRightarrow%20P%28%28XY%29%7CZ%29%20%3D%20%5Csum_W%20P%28%28XYW%29%7CZ%29%20%3D%20%5Csum_W%20P%28X%7CZ%29P%28%28YW%29%7CZ%29%3D%20P%28X%7CZ%29%5Csum_W%20P%28%28YW%29%7CZ%29%20%3D%20P%28X%7CZ%29P%28Y%7CZ%29)

    ![decomp](http://latex.codecogs.com/svg.latex?%5CRightarrow%20%28X%5Cperp%20Y%29%7CZ)

  - Proof (right to left): I **don't know** how to prove it.(?????maybe it is not true??)

- Contraction: ![decomp](http://latex.codecogs.com/svg.latex?%28%28X%5Cperp%20Y%29%7CZ%29%5Cwedge%20%28X%5Cperp%20W%7C%28YZ%29%29%5CRightarrow%20%28X%5Cperp%20%28WY%29%29%7CZ)

- weak union: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20%28YW%29%29%7CZ%5CRightarrow%20%28X%5Cperp%20Y%29%7C%28ZW%29)

- intersection: ![decomp](http://latex.codecogs.com/svg.latex?%28X%5Cperp%20Y%29%7C%28WZ%29%5Cwedge%20%28X%5Cperp%20W%29%7C%28YZ%29%5CRightarrow%20%28X%5Cperp%28YW%29%29%7CZ)


### Application

Example: In a naive bayes model, where: Y is the cause variable, X1,...,Xn are the effects variables. ![nbmodel](./img/chap201.jpg)

We can give a conditional independence assertion: 

![nbmodel](http://latex.codecogs.com/svg.latex?X_A%5Cperp%20X_%7B%5Cbar%7BA%7D%7D%7CY%2C%5Cforall%20A%5Csubset%5C%7B1%2C2%2C%5Ccdots%2Cn%5C%7D)

Where ![nbmodel2](http://latex.codecogs.com/svg.latex?%20A%3D%5C%7Bi_1%2C%5Ccdots%2Ci_k%5C%7D%5Csubset%5C%7B1%2C2%2C%5Ccdots%2Cn%5C%7D%2C%20%5Cbar%7BA%7D%3D%5C%7B1%2C2%2C%5Ccdots%2Cn%5C%7D%5C%5CA%2C%20X_A%3D%5C%7BX_%7Bi_1%7D%2C%5Ccdots%2CX_%7Bi_k%7D%5C%7D)



In general, conditional independence can be used to transfer **joint parameterization** into **conditional parameterization**. In this way, we can greatly reduce the number of parameters. Formally,

![jointpara](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7B1%3An%7D%5D%3D%5Cprod_%7Bi%3D1%7D%5En%5CPr%5BX_i%7Cparents%28X_i%29%5D)

The number of parameters change: ![params](http://latex.codecogs.com/svg.latex?%5Cprod_%7Bi%3D1%7D%5En%20%7CX_i%7C%5Cto%20%5Csum_%7Bi%3D1%7D%5En%20%7CX_i%7C%5E%7B%5Cprod_%7BX_j%5Cin%20parents%28X_i%29%7D%7CX_j%7C%7D)



# Bayesian Network

- Bayesian: uses the Bayes' theorem, which specifies an event's probability given some conditions.
- network: represented by directed graph, as a network.

### Definition

A Bayesian Network is a *directed acyclic graph* in which:

- node:
  - each node corresponds to a random *variable*
  - (numeric parameters) each node has a *conditional* probability distribution P(x|parents(x))
- links:
  - a link from node X to node Y, X is said to be a *parent* of Y.
  - intuitive meaning: X is a *parent* of Y -> X has a *direct impact* on Y. Causes should be parents of effects.
- No directed cycles. (It is a DAG)

#### Semantics

Two ways to understand the semantics of Bayesian Network:

1. as a representation of the joint probability distribution
2. as an encoding of a collection of conditional independence statements.

#### Structures

![ind](http://latex.codecogs.com/svg.latex?X%5Cperp%20Z%7CY%2C%5Cneg%20X%5Cperp%20Z)

- x<-y->z: common cause
- x<-y<-z: indirect evidential effect
- x->y->z: indirect causal effect

![not-ind](http://latex.codecogs.com/svg.latex?%5Cneg%20X%5Cperp%20Z%7CY%2C%20X%5Cperp%20Z)

- x->y<-z: common effect

## Usage

### Build algorithm
1. Nodes: 
   1. determine the set of variables that are required to model the domain.
   2. Order them {X_1,...,X_n}. **Any order will work**. But the resulting network will be more compact if the variables are ordered such that causes precede effects.

2. Links: For each Xi do:
   1. find the *minimal parent subset A* from {1,2,...,i-1}, s.t. P(X_i|{1,2,...,i-1})=P(X_i|A) 
      - in some way, minimal means direct influence.  For example, some variable in {1,2,...,i-1} may have indirect influence on X_i, through a variable X_k. Then, if we include X_k, they are excluded.
   2. for each parent X_k in A, insert a link from X_k to X_i 
   3. write down the conditional probability table.



### Identification of CI(conditional independence)

Not independent (dependent) can transmit conditions. That means if we know some condition, all variables not independent of it are (partially) known.

Given a Bayesian network, how to find the independence relationship between variables from the network structures?

An important theorem says:

![d-sep](http://latex.codecogs.com/svg.latex?%5Ctext%7Bd-sep%7D%28X%3BY%7CZ%29%5CRightarrow%20%28X%5Cperp%20Y%29%7CZ)

converse does not hold in general, but for "almost" all distributions.

#### d-separation

 This term describes a structural relation between variables in a given network. We can connect this structural relation with independence relation.

Given observed variables O, any variables Xi and Xj for which there is **no active trail** are called *d-separated* by O.  Written as **d-sep(Xi;Xj|O) **

- active trail: an undirected path in BN structure G is called active trail for observed variables![active](http://latex.codecogs.com/svg.latex?O%5Csubset%5C%7BX_1%2CX_2%2C%5Ccdots%2CX_n%5C%7D), if for every consecutive triple of vars X,Y,Z on the path
  - ![active1](http://latex.codecogs.com/svg.latex?X%5Cto%20Y%5Cto%20Z%2CY%5Cnotin%20O)
  - ![active1](http://latex.codecogs.com/svg.latex?X%5Cgets%20Y%5Cgets%20Z%2CY%5Cnotin%20O)
  - ![active3](http://latex.codecogs.com/svg.latex?X%5Cgets%20Y%5Cto%20Z%2CY%5Cnotin%20O)
  - ![active4](http://latex.codecogs.com/svg.latex?X%5Cto%20Y%5Cgets%20Z), and Y or any of Y's descendants is observed.

- linear time algorithm for d-separation: find all nodes (active trail) reachable from X

  1. mark Z and its ancestors

  2. do breath-first search starting from X; stop if path is blocked

     