---
title: Week 2 Reading - Approximation Algorithms
date: 2019-09-27
tags: [Algorithms, reading]
categories: Advanced algorithms
---

# Advanced Algorithms (Vazirani)

## Chapter 8 Knapsack

Key idea: formalize the case that some NP-hard optimization problem allow approximability to any required degree.

We give three bounds:

1. solution's objective is bounded by an error parameter to the optimal solution (1+-\epsilon)*opt
2. the running time is bounded by **polynomial time** in the **size** of instance I.
3. the running time is bounded by **polynomial time** in 1/(\epsilon)

We give three definitions, satisfying different bounds respectively:

- AS: approximation scheme: 1
- PTAS (polynomial time approximation scheme): 1,2
- FPTAS (fully polynomial time approximation scheme): 1,2,3

FPTAS is the "**best**"

### Size of a problem instance

an instance I consists of:

- objects, such as sets or graphs
- numbers, such as cost, profit, size

Written them in binary. 

- |I|=the number of bits needed to write I.
- |I_u|=the number of bits needed to write all numbers occurring in I.



### Pseudo-polynomial time

**Pseudo-polynomial time algorithm**: if the runtime is some polynomial *in the numeric value of the input*, rather than in the number of bits required to represent it.

This is a weakening of the notion of *efficient algorithm*.

running time on I bounded by

- a polynomial in |I_u|

  or in other words

- a polynomial in the length of the input (the number of bits required to represent it) 

- and the numeric value of the input (the largest integer present in the input).

All known pseudo-polynomial time algorithms for NP-hard problems are based on **dynamic programming**.

#### Examples

- a pseudo-polynomial time algorithm but **not** polynomial time algorithm: Prime-testing algorithm runs in time O(n^4), but it's not a polynomial-time algorithm  because as a function of the number of bits x required to write out the input, the runtime is O(2^\{4x\}).
-  in many cases pseudo polynomial time algorithms are perfectly fine, because the size of the numbers won't be too large. For example, [counting sort](http://en.wikipedia.org/wiki/Counting_sort) has runtime O(n + U), where U is the largest number in the array. This is pseudo polynomial time (because the numeric value of U requires O(log U) bits to write out, so the runtime is exponential in the input size). 
  If we artificially constrain U so that U isn't too large (say, if we let U be 2), then the runtime is O(n), which actually is polynomial time. 

#### Properties
Any NP-hard problem admitting an FPTAS must admit a pseudo-polynomial time algorithm.

**Claim**: All known pseudo-polynomial  time algorithms for NP-hard problems are based on **dynamic programming**.



### Strong NP-hardness and existence of FPTAS's

Assuming P!=NP

**Claim** Very few of the known NP-hard problems admit an FPTAS.

#### Strongly NP-hard

A problem is strongly NP-hard if every problem in NP can be polynomially reduced to it in such a way that numbers in the reduced instance are always written in unary.

Claims:

- most known NP-hard problems are strongly NP-hard
- A strongly NP-hard problem cannot have a pseudo-polynomial time algorithm.

## Chapter 9 Bin Packing

