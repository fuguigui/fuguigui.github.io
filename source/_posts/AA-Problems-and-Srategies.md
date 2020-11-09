---
title: Problems and Srategies
date: 2019-09-24
tags: [advanced algorithms]
categories: course notes
---

# Vertex cover

settings:

- G = (V, E): undirected graph.
- cost function c from the vertex set to non-negative number: V-> Q^+

Objective: find a subset V' of V, s.t.:

- it is a vertex cover of G: every edge has at least one endpoint in V'
- the cost is minimum.

## Algorithms:

the maximal matching algorithm gives 2-approximation.

# Maximum Matching

settings:

- G=(V,E): undirected
- matching definition: A subset of edges M of E is a matching if no two edges of M share an endpoint.

Objective: find a subset M of E, s.t.:

- |M| is maximum.

## Algorithms:

the maximal matching algorithm.

# Scheduling Jobs

## V1.0: On a single machine (NP-hard)

Settings:

machine:

- process at most one job at a time.
- must process a job until its completion once it has begun processing

Jobs: totally n jobs, for job i

- time length: p_i units of time required to complete
- starting time: r_i: job may begin no earlier than r_i
- due time: d_i: a specified due date.

Objective:

minimize the maximum lateness: L_{max}=max_j L_j

- L_j:=C_j-d_j
- C_j: actual completion time

### Heuristic strategy

Begin from the tasks with the earliest due date.

## V2.0: on identical parallel machines

Settings:

machine:

- process at most one job at a time
- must process a job until its completion once it has begun processing
- all the same, totally m machines.

Jobs: totally n jobs.

- required completion time length: p_i

Objective:

minimize the time of the latest finished job

### local search procedure:

start from random arrangements, and if possible, rearrange the latest job

### list scheduling algorithm (greedy)

whenever a machine becomes idle, assign one of the remaining jobs to it.

# Set cover

settings:

- a ground set E of n elements: E={e1,e2,...,en}
- a collection of subsets of E, S={S_1,S_2,\cdots, S_k},S_j is a subset of E.
- a cost function c: S->Q^+

Objective: find a subset of S, s.t.:

- cover all elements of U
- the cost is minimum

## Algorithms

- From integer program to linear program.
  - solve a linear programming relaxation for the set cover problem
  - round the fractional solution to integral solution.
- (randomized rounding) the fractional value is interpreted as the probability that xj should be set to 1.
- (Greedy) iteratively 
  - pick the most cost-effective set (the set that minimizes the ratio of its weight to the number of currently uncovered elements it contains) 
  - remove the covered elements, 
  - until all elements are covered.

## Applications

### To Vertex cover problem

Settings:

- E: the set of edges
- S_i: the set of edges linked to vertex i.
- c: \sum_j e_{ij}

Objective:

- cover all edges
- the cost is minimum



# K-center

Settings:

- a set V of n nodes: v1,v2,\cdots, vn
- a distance evaluation: d: V\*V->Q\*
  - assume d_{ii}=0
  - symmetry
  - triangle inequality
- a distance definition from a point to a set:
  - d(v_i, S) = min_{v_j\in S} d(v_i, v_j)

Objective: Find a subset S of V, s.t.:

- |S| = k
- minimize the maximum distance of nodes in the set V to S.

## Algorithms

- Greedy:

  1. randomly choose one node in V and add it into S
  2. while |S|<k:
     1. j<-arg max d(j,S)
     2. add j into S

  2-approximation.



# Knapsack

Settings:

- a set S of n objects: {a1,a2,\cdots, an}
- each object has a size and profit (both positive integer):
  - size(ai)\in Z^+
  - profit(ai)\in Z^+
- a "knapsack capacity" B\in Z^+

Objective: find a subset of objects 

- whose total size is bounded by B
- the total profit is maximized.

## Algorithms

### Greedy

sort the objects by decreasing ratio of profit to size, and then greedily pick objects in this order.

- can be made to perform **arbitrarily badly**: 

# Bin packing

Settings:

- n items: with sizes a1, \cdots, an\in (0,1]
- bins: unit-sized, all with the size 1

Objective: find a packing

- minimize the number of binds used.

## Algorithms

### First-Fit (2-approximation)

This algorithm considers items in an arbitrary order.

In the i-th step, it has a list of partially packed bins, say B1, \cdots, Bk. It attempts to put the next item, ai, in one of these bins, in this order:

- if ai does not fit into any of these bins, it opens a new bin B_{k+1} and puts a_i in it

  