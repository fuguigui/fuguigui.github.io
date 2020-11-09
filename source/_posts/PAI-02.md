---
title: Probability review
date: 2020-01-23
tags: [probabilistic artificial intelligence, reinforcement learning]
categories: course notes
---

# Probability Review

## Basic concepts

- Probability space: three elements: atomic events space Omega (can be continuous), sigma-algebra (closed under complement and unions) and probability measure. ![img](http://latex.codecogs.com/svg.latex?%5C%7B%5COmega%2C%20F%2C%20P%5C%7D%2C%20F%20%5Csubseteq%202%5E%7B%5COmega%7D%2C%20P%3AF%5Cto%20%5B0%2C1%5D)
- probability axioms: 
  - normalization (the probability of the whole set is 1) ![norm](http://latex.codecogs.com/svg.latex?%5Ctext%7BPr%7D%28%5COmega%29%3D1)
  - non-negativity (all the probability is larger than 0) ![nonnge](http://latex.codecogs.com/svg.latex?%5CPr%5BA%5D%5Cgeq%200%2C%5Cforall%20A%5Cin%20F)
  - ("add operation") sigma-additivity (the probability of the union of disjoint sets are the sum of the probability of each set), ![add-union](http://latex.codecogs.com/svg.latex?%5CPr%5B%5Cbigcup_%7Bi%3D1%7DA_i%5D%3D%5Csum_%7Bi%3D1%7D%5CPr%5BA_i%5D%2C%5Cforall%20A_i%5Cin%20F%20and%20A_i%5Ccap%20A_j%3D%5Cphi%2C%20i%5Cneq%20j)
    - this axiom builds the relation between the "add" operation in the algebra and the "union" operation in the set theory.
  - From the three axioms, we can deduct:
    - ("minus" operation) ![minus](http://latex.codecogs.com/svg.latex?%5CPr%5B%5COmega-A%5D%3D1-%5CPr%5BA%5D)
    - ("empty") ![empty](http://latex.codecogs.com/svg.latex?%5CPr%5B%5Cphi%5D%3D0)
    - ("minus" operation) ![minus-two](http://latex.codecogs.com/svg.latex?%5CPr%5BA-B%5D%3D%5CPr%5BA%5D-%5CPr%5BA%5Ccap%20B%5D) since A-B and A\cap B are disjoint. (All minus operations are from add operations)
    - ("inclusion-exclusion principle") ![in-ex](http://latex.codecogs.com/svg.latex?%5CPr%5BA%5Ccup%20B%5D%3D%5CPr%5BA%5D%2B%5CPr%5BB%5D-%5CPr%5BA%5Ccap%20B%5D)
    - ("inequality") ![inequality](http://latex.codecogs.com/svg.latex?A%5Csubset%20B%5CRightarrow%5CPr%5BA%5D%5Cleq%5CPr%5BB%5D)

## Derivatives

Based on the basic definitions, we can define more advanced concepts, such as independent events, conditional probability

### Independence

- independent events: two random events A, A' are independent iff ![img](http://latex.codecogs.com/svg.latex?P%28A%5Ccap%20A%27%29%3DP%28A%29P%28A%27%29)
  - event is a subset of the sample space Omega.
  - the left side means the probability of the joint of two sets
  - the right side means the multiplication of two probabilities
  - this definition connects the calculation of sets with the calculation of numbers. In some way, map the "joint" operation to the "multiplication" operation. 
  
- random variable: **A random variable is a mapping.** Denote it as X. Given a set D, ![img](http://latex.codecogs.com/svg.latex?X%3A%5COmega%5Cto%20D). How to define probability for the element(s) in D? Based on the corresponding elements in Omega. That is ![img](http://latex.codecogs.com/svg.latex?x%5Cin%20D%2C%20%5CPr%5BX%3Dx%5D%3D%5CPr%5B%5C%7Bw%3A%20X%28w%29%3Dx%2C%20w%5Cin%5COmega%5C%7D%5D)
  - the left side is the probability definition on a new set D
  - the right side is the corresponding probability on the on the original whole set Omega.
  - I would call the left side is a symbol for representation while right side is the essence. 
  - ??? Does it mean all elements in Omega must have a corresponding element in D? 
    - Yes!!! X is defined on the **whole** Omega. That implies ![rv-def](http://latex.codecogs.com/svg.latex?%5Csum_%7Bx%7D%20%5CPr%5BX%3Dx%5D%3D1)
  - ??? Does it mean X must be deterministic???
  -  How to interpret that in continuous settings? In continuous distribution, no "good" definition of the probability of a point.
    - From [this wiki](https://en.wikipedia.org/wiki/Elementary_event), "in a continuous distribution, individual elementary events must all have a probability  of zero because there are infinitely many of them— then non-zero  probabilities can only be assigned to non-elementary events."
    - I think the definition may comes from a more advanced concept [Borel-algebra](https://en.wikipedia.org/wiki/Borel_set). Also in the first chapter in this book [Foundations of Modern Probability](https://books.google.ch/books?id=uL7UBwAAQBAJ&pg=PA1&hl=zh-CN&source=gbs_toc_r&cad=4#v=onepage&q&f=false), it says something about the Borel-algebra, which is beyond my understanding. But it convinces me that this kind definition of probability works.
    
  
- independent random variable: two random variables are independent iff ![ind-rv](http://latex.codecogs.com/svg.latex?P%28xy%29%3DP%28x%29P%28y%29%2C%5Cforall%20x%5Cin%20X%28%5Ccdot%29%2C%20y%5Cin%20Y%28%5Ccdot%29)

  - different from independent events, which could be represented easily be the set (Vien graph), independent random variables are not straightforward in the Vien, because both of them work on the whole sample space. But we can show the Vien graph of their specific values.

  -  use ![p-def](http://latex.codecogs.com/svg.latex?X%5Cperp%20Y) to denote X and Y are independent. This notation is very descriptive. Taking the algebra  view, X and Y are "orthogonal" to each other. For instance, supposing the sample space Omega is a cube. X maps it to the z axis, and Y maps it to the y axis. Then for any pair of (x,y), P(xy) (shown in the Vien graph is a rectangle in surface y-z) is exactly P(x)P(y), which are two orthogonal rectangles with an overlapped rectangle.

    
### Joint distributions
Instead of random variable, using random vector of several variables. ![img](http://latex.codecogs.com/svg.latex?X%3D%5BX_1%28w%29%2CX_2%28w%29%2C%5Ccdots%2C%20X_n%28w%29%5D) and ![img](http://latex.codecogs.com/svg.latex?%5CPr%5BX%3Dv_n%5D%3D%5CPr%5BX_1%28w%29%3Dv_1%2CX_2%28w%29%3Dv_2%2C%5Ccdots%2C%20X_n%28w%29%3Dv_n%5D)

- Interpreted from the set theory view: supposing the Omega has high dimension and we use a n-dimension lense to inspect it to find the aimed subspace.

- note that each item is a random variable, that means ![rv-def](http://latex.codecogs.com/svg.latex?%5Csum_%7Bx%7D%20%5CPr%5BX_i%3Dx%5D%3D1).Also, this is the precondition of the  sum rule. Otherwise, supposing ![rv-def](http://latex.codecogs.com/svg.latex?%5Csum_%7Bx%7D%20%5CPr%5BX_i%3Dx%5D%3C1), we cannot have ![rv-def](http://latex.codecogs.com/svg.latex?%5Csum_%7Bx%7D%20%5CPr%5BX_i%3Dx%2CY%5D%3D%5CPr%5BY%5D)

  
### Conditional probability
Define a new operator "|" in the probability theory
![cond-prop](http://latex.codecogs.com/svg.latex?%5CPr%5BA%7CB%5D%3D%5Cfrac%7B%5CPr%5BA%5Ccap%20B%5D%7D%7B%5CPr%5BB%5D%7D%2C%5CPr%5BB%5D%3E0)

- Observation of the left-side (set theory view): it actually changes the event space to the set of B from the original set Omega.
- calculation view: introduce the division in the probability theory.

## Sum rule and product rule

- sum rule (marginalization): an application of the add-operation of disjoint sets

  ![sum-rule](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7B1%3Ai-1%7D%EF%BC%8CX_%7Bi%2B1%3An%7D%5D%3D%5Csum_%7Bx_i%7D%5CPr%5BX_%7B1%3Ai-1%7D%2Cx_i%2CX_%7Bi%2B1%3An%7D%5D)

- product rule(chain rule): an application of the definition of conditional probability.

  ![chain-rule](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7B1%3An%7D%5D%3D%5CPr%5BX_n%7CX_%7B1%3An-1%7D%5D%5Ccdots%5CPr%5BX_3%7CX_1X_2%5D%5CPr%5BX_2%7CX_1%5D%5CPr%5BX_1%5D)

- Application: Bayes' Rule: from the prior P(X) and likelihood P(Y|X) to compute the posterior P(X|Y)

  ![bayes-rule](http://latex.codecogs.com/svg.latex?%5CPr%5BX%7CY%5D%3D%5Cfrac%7B%5CPr%5BX%5D%5CPr%5BY%7CX%5D%7D%7B%5Csum_x%5CPr%5BX%5D%5CPr%5BY%7CX%5D%7D)

  - the nominator: product rule
  - the denominator: sum rule, then product rule

## Operations in Probability

### Operators

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

### Rules

This part is based on my own deduction, I am not sure if they are true. Please contact me if you have other ideas! I would greatly appreciate it.

- "reducing rule": ![rl-al](http://latex.codecogs.com/svg.latex?P%28A%28B%7CC%29%29%20%3D%20P%28%28AB%29%7CC%29)

  - just like the "conditional" operator, this rule is also tricky, cause A and B|C are in the different sample space. Here, I would reduce the larger sample space to the smaller sample space. That is reducing A to A|C actually.  By doing this, We will have 

    ![op-cd](http://latex.codecogs.com/svg.latex?P%28A%28B%7CC%29%29%3DP%28%28A%7CC%29%28B%7CC%29%29%3DP%28%28AB%29%7CC%29)

- "association rule": ![rl-al](http://latex.codecogs.com/svg.latex?P%28A%28BC%29%29%20%3D%20P%28%28AB%29C%29%3DP%28ABC%29)

  - from the set view, the proof is trivial.

  



# Probabilistic propositional logic

So far, we define the "probability" from the set theory view and so for the operations, which could be interpreted as the set operations.

How to express uncertainty about logical propositions?

The probability of a proposition is the probability mass of all **models** of it.

- model: all **events** that make the proposition true
- event: encode assignment to all propositional symbols.

In the paper [probability, logic and probability logic](https://faculty.arts.ubc.ca/pbartha/p520w07/comp_logic.pdf), there is a definition of probability theory from the logic theory view.

S: a collection of sentences of a language, closed under finite truth-functional combinations, with the following axiomatization:

- normalization ![norm-log](http://latex.codecogs.com/svg.latex?%5Ctext%7BPr%7D%28T%29%3D1), If T is a tautology
- non-negativity: ![nonnge-log](http://latex.codecogs.com/svg.latex?%5CPr%5BA%5D%5Cgeq%200%2C%5Cforall%20A%5Cin%20S)
- ("add operation") ![add-log](http://latex.codecogs.com/svg.latex?%5CPr%5BA%5Clor%20B%5D%3D%5CPr%5BA%5D%2B%5CPr%5BB%5D) for all A and B in S, such that A and B are logically incompatible.



### Examples given in the lecture

The method is:

1. degrade the logical operations (e.g. imply) to the most basic three operations (and, or and not)
2. go to the set theory, find the corresponding set of the logical expression and calculate the probability of the set

|           | Toothache | Toothache | no toothache | no toothache |
| --------- | --------- | --------- | ------------ | ------------ |
| -         | catch     | no catch  | catch        | no catch     |
| cavity    | 0.108     | 0.012     | 0.072        | 0.008        |
| no cavity | 0.016     | 0.064     | 0.144        | 0.576        |

P(toothache)? find the corresponding set (the left two columns) P(toothache)=0.108+0.016+0.012+0.064=0.2

P(toothache=>cavity)?

1. degrade into and, or and not: ![log](http://latex.codecogs.com/svg.latex?A%5CRightarrow%20B%5CLeftrightarrow%20%5Clnot%20A%5Cvee%20B)

2. not toothache correspondents to the right two columnes, cavity correspondents to the first row. Then take the union of them.

   = 0.108+0.012+0.072+0.008+0.144+0.576=0.92