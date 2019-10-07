---
title: Week 1 Reading - Approximation Algorithms
date: 2019-09-23
tags: [Algorithms, reading]
categories: Advanced Algorithms
---

# Advanced Algorithms (Vazirani)

## Questions

- what is the general principles in designing approximation algorithms?
  - lower bounding scheme
- how to we establish the approximation guarantee? It is NP-hard to compute the cost of the optimal solutions.



## Definitions

### Approximation algorithm

- for a NP-hard problem
- polynomial time
- "close" to the optimal solutions, within a guaranteed factor of the optimal values. 

## Typical Problems

### Vertex cover

settings:

- G = (V, E): undirected graph.
- cost function c from the vertex set to non-negative number: V-> Q^+

Objective: find a subset V' of V, s.t.:

- it is a vertex cover of G: every edge has at least one endpoint in V'
- the cost is minimum.

#### Algorithms:

the maximal matching algorithm gives 2-approximation.

### Maximum Matching

settings:

- G=(V,E): undirected
- matching definition: A subset of edges M of E is a matching if no two edges of M share an endpoint.

Objective: find a subset M of E, s.t.:

- |M| is maximum.

#### Algorithms:

the maximal matching algorithm.

### Set cover

settings:

- a universe U of n elemnets
- a collection of subsets of U, S={S_1,S_2,\cdots, S_k}
- a cost function c: S->Q^+

Objective: find a subset of S, s.t.:

- cover all elements of U
- the cost is minimum

#### Algorithms

- iteratively pick the most cost-effective set and remove the covered elements, until all elemenets are covered.

## Principles

### Lower bounding scheme

Define an algorithm giving the lower bound of the original problem.

Finding a good lower bound on OPT is a basic starting point in the design of an approximation algorithm for a minimization problem.

### Greedy Strategy

### Layering



# The Design of Advanced Algorithms (Williamson & David)

## Whats and Whys of Approximation Algorithms

For NP-hard problem, we can only choose 2 of 3:

1. find optimal solutions
2. in polynomial time
3. for any instance

## Definitions

- alpha-approximation.
  - this definition provides a **performance guarantee** of our algorithm.
  - we usually prefer solutions within a few percent of optimal rather than, say twice optimal.

- PTAS: polynomial-time approximation scheme.
  - this definition demonstrates whether there exist good approximation algorithms for problems of interest.

## Techniques

### Linear programming

- **decision variables**: represent decisions that need to be made.
- **constraints**: a number of linear inequalities and equalities.
- **feasible solution**: assignment: assign real numbers to variables such that all constraints are satisfied.
- **objective function**: a linear function of the decision variables, either being maximized or minimized.

**Integer program** cannot be solved in polynomial time.

**Linear program** is polynomial-time solvable.

That's why we usually use linear program as a relaxation for integer program.

Goodness:

- every feasible solution for the original integer program is feasible for the linear program
- the value of any feasible solution for the integer program has the same value in the linear program --> Obtain a lower bound or an upper bound of the original problem.
- a fractional solution to the linear program can be rounded to a solution to the integer program within in a certain factor.

### Greedy algorithm

**Process**: works by making a sequence of decisions: each decision is made to optimize that particular decision, even though this sequence of locally optimal decisions might not lead to a globally optimal solution.

**Advantage**: typically very easy to implement, even when they have no performance guarantee.

## Chapter 2 Greedy Algorithms and Local Search

- Greedy algorithms
  - constructed step by step
  - at each step: the decisions is locally the best possible.
- Local search algorithms
  - starts with an arbitrary feasible solution to the problem
  - at each step: checks if some small, local change results in an improved objective function.
  - for the running time: require extra restrictions on the local change to guarantee polynomial time.

Common: making a sequence of decisions that optimize some local choice.