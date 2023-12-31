---
title: PAI1 Foundantion
date: 2020-01-23
tags: [AI, Reinforcement Learning, Probability Theory]
categories: [Learning Notes]
mathjax: true
---

## Fundamental Settings

Use Agent-environment model to represent all tasks.

### Agent

has a set $A$​ and a function $f$.

- $A$: the elements in $A$ are called actions.
- $f$: map sequence of percepts to action $f: P\to A$

### Environment

has a set $P$, a function $f$, a measure function $R$.

- $P$: the elements in $P$ are called percepts or environment states.
- $f$: map sequence of actions to percept  $f:A\to P$
- $R$: evaluates any given sequence of environment states: $R:S^\ast \to \mathbb{R}$ 

We want to design an agent to be rational, doing the right thing. How to define "right"?  Based on the evaluation of **environment** states. 

**A rational agent** is defined as: for each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given

- the evidence provided by the percept sequence 
- and whatever built-in knowledge the agent has.
- ? how to show this formally? How to mathematically evaluate the performance measurement related to agent's built-in knowledge

# Probability Review

## Basic concepts

- Probability space: three elements: atomic events space Omega, can be continuous, sigma-algebra which is closed under complement and unions and probability measure. $\{\Omega, F, P\}, F \subseteq 2^{\Omega}, P:F\to [0,1]$ 
- probability axioms: 
  - normalization, making the probability of the whole set is 1: $\text{Pr}(\Omega)=1$ 
  
  - non-negativity, all the probability is larger than 0: $\Pr[A]\geq 0,\forall A\in F$ 
  
  - "add operation" sigma-additivity, the probability of the union of disjoint sets are the sum of the probability of each set:
    $$
    \Pr[\bigcup_{i=1}A_i]=\sum_{i=1}\Pr[A_i],\forall A_i\in F \text{ and } A_i\cap A_j=\phi, i\neq j
    $$
    
    - this axiom builds the relation between the "add" operation in the algebra and the "union" operation in the set theory.
    
  - From the three axioms, we can deduct:
    - "minus" operation: $\Pr[\Omega-A]=1-\Pr[A]$ 
    - "empty": $\Pr[\phi]=0$ 
    - "minus" operation: $5CPr[A-B]=\Pr[A]-\Pr[A\cap B]$ since $A-B$ and $A\cap B$​ are disjoint. All minus operations are from add operations
    - "inclusion-exclusion principle" $\Pr[A\cup B]=\Pr[A]+\Pr[B]-\Pr[A\cap B]$​
    - "inequality": $A\subset B\Rightarrow\Pr[A]\leq\Pr[B]$ 

## Derivatives

Based on the basic definitions, we can define more advanced concepts, such as independent events, conditional probability

### Independence

- independent events: two random events A, A' are independent iff $P(A\cap A')=P(A)P(A')$ 
  - event is a subset of the sample space Omega.
  - the left side means the probability of the joint of two sets
  - the right side means the multiplication of two probabilities
  - this definition connects the calculation of sets with the calculation of numbers. In some way, map the "joint" operation to the "multiplication" operation. 
  
- random variable: **A random variable is a mapping.** Denote it as $X$. Given a set  D, $X: \Omega\to D$ . How to define probability for the element(s) in $D$? Based on the corresponding elements in $\Omega$​​. That is 
  $$
  x\in D, \Pr[X=x]=\Pr[\{w: X(w)=x, w\in\Omega\}]
  $$
  
  - the left side is the probability definition on a new set $D$
  - the right side is the corresponding probability on the on the original whole set $\Omega $​.
  - I would call the left side is a symbol for representation while right side is the essence. 
  - ??? Does it mean all elements in $\Omega $​ must have a corresponding element in $D$​? 
    - Yes!!! $X$ is defined on the **whole** $\Omega $. That implies $\sum_{x} \Pr[X=x]=1$
  - ??? Does it mean X must be deterministic???
  -  How to interpret that in continuous settings? In continuous distribution, no "good" definition of the probability of a point.
    - From [this wiki](https://en.wikipedia.org/wiki/Elementary_event), "in a continuous distribution, individual elementary events must all have a probability  of zero because there are infinitely many of them— then non-zero  probabilities can only be assigned to non-elementary events."
    - I think the definition may comes from a more advanced concept [Borel-algebra](https://en.wikipedia.org/wiki/Borel_set). Also in the first chapter in this book [Foundations of Modern Probability](https://books.google.ch/books?id=uL7UBwAAQBAJ&pg=PA1&hl=zh-CN&source=gbs_toc_r&cad=4#v=onepage&q&f=false), it says something about the Borel-algebra, which is beyond my understanding. But it convinces me that this kind definition of probability works.
    
  
- independent random variable: two random variables are independent iff $P(xy)=P(x)P(y),\forall x\in X(\cdot), y\in Y(\cdot)$ 

  - different from independent events, which could be represented easily be the set (Vien graph), independent random variables are not straightforward in the Vien, because both of them work on the whole sample space. But we can show the Vien graph of their specific values.

  -  use $X\perp Y$​ to denote $X$​ and $Y$​ are independent. This notation is very descriptive. Taking the algebra  view, $X$​ and $Y$​ are "orthogonal" to each other. For instance, supposing the sample space Omega is a cube. $X$ maps it to the $z$ axis, and $Y$ maps it to the $y$ axis. Then for any pair of $(x,y), P(xy)$ , shown in the Vien graph is a rectangle in surface y-z,  is exactly $P(x)P(y)$, which are two orthogonal rectangles with an overlapped rectangle.

    
### Joint distributions
Instead of random variable, using random vector of several variables. 

$X=[X_1(w),X_2(w),\cdots, X_n(w)]$ and $\Pr[X=v_n]=\Pr[X_1(w)=v_1,X_2(w)=v_2,\cdots, X_n(w)=v_n]$

- Interpreted from the set theory view: supposing the Omega has high dimension and we use a n-dimension lense to inspect it to find the aimed subspace.

- note that each item is a random variable, that means $\sum_{x} \Pr[X_i=x]<1$, we cannot have $\sum_{x} \Pr[X_i=x,Y]=\Pr[Y]$

  
### Conditional probability
Define a new operator "|" in the probability theory
$$
\Pr[A|B]=\frac{\Pr[A\cap B]}{\Pr[B]},\Pr[B]>0
$$

- Observation of the left-side (set theory view): it actually changes the event space to the set of $B$ from the original set $\Omega.$
- calculation view: introduce the division in the probability theory.

## Sum rule and product rule

- sum rule marginalization: an application of the add-operation of disjoint sets
  $$
  \Pr[X_{1: i-1}，X_{i+1: n}]=\sum_{x_i}\Pr[X_{1:i-1},x_i,X_{i+1: n}]
  $$

- product rule chain rule: an application of the definition of conditional probability.
  $$
  \Pr[X_{1:n}]=\Pr[X_n|X_{1:n-1}]\cdots\Pr[X_3|X_1X_2]\Pr[X_2|X_1]\Pr[X_1]
  $$

- Application: Bayes' Rule: from the prior $P(X)$ and likelihood $P(Y|X)$ to compute the posterior $P(X|Y)$

  $$
  \Pr[X|Y]=\frac{\Pr[X]\Pr[Y|X]}{\sum_x\Pr[X]\Pr[Y|X]}
  $$
  
  
  - the nominator: product rule
  - the denominator: sum rule, then product rule

## Operations in Probability

### Operators

- and: $\cap$​  or $,$  or missed, e.g. $P(A\cap B) = P(A,B) = P(AB) $​
- or: $\cup$, e.g. $P(A\cup B),P(A\cup B) = P(A) + P(B)-P(A\cap B)$​
- conditional: $|$, e.g. $P(A| B)=\frac{P(AB)}{P(B)}$
  - this operator is a little bit tricky, cause if considering the sample space, "and" and "or" operators are in the original sample space but "conditional" operator **changes** the sample space !!!
- conditional independence: $\perp$,e.g.$P((A\perp B)|C) = P(A|C)P(B|C)$

Priorities from lowest to highest: $| < \perp < \cap, \cup$

For example, $P(A,B|C) = P((A,B)|C)\neq P(A,(B|C))$

- "and" could be viewed as "multiply" $\ast$​ in algebra operations in some way

  - exchangeable: $P(A\cap B)= P(B\cap A)$

- "conditional" could be viewed as "plus" $+$, always the lowest

  - but it is **not exchangeable**, that means $P(A| B)\neq P(B|A)$

  - it is **ordered**. I am not sure which order it exactly is. Just from my own understanding, the order should be from right to left,  which means $P(A|B|C) = P(A|(B|C))\neq P((A|B)|C)$

    My explanation is as follows:

    - suppose$P(A|B|C) = P(A|(B|C))$ We will have 
      $$
      P(A|(B|C)) = \frac{P(A,(B|C))}{P(B|C)} = \frac{P((AB)|C)}{P(B|C)}
      $$
    
      $$
      =\frac{P(ABC)P(C)}{P(C)P(BC)}=\frac{P(ABC)}{P(BC)}=P(A|(BC))
      $$
    
      
    
      - the second equality uses the "reducing rule", explained in the next block.
    
      - this directly gives us the conclusion that $P(A|B|C) =P(A|(BC))$
    
      - generate it to the n cases, we will have 
    
        $$
        P(A_n|A_{n-1}|\cdots|A_1) = P(A_n|(A_{n-1}(\cdots |A_1))) 
        $$
        
        $$
        = P(A_n|(A_{n-1}\cdots A_1))
        $$
        
        
        
        The proof is ignored here. Using induction will easily prove it.
    
    - suppose $P(A|B|C) = P((A|B)|C)$​ We will have 
      $$
      P((A|B)|C)=\frac{P((A|B),C)}{P(C)}=\frac{P(C(A|B))}{P(C)}
      $$
      
      $$
      =\frac{P((AC)|B)}{P(C)}=\frac{P(ABC)}{P(B)P(C)}
      $$
      
      
      
      - also, the third equality uses the "reducing rule"
      - well, I couldn't find error in the algebra calculation but from the definition of probability, using $P(B)P(C)$ as the denominator, I don't know what the sample space becomes. Based on this aspect, I will refuse this assumption and take the former one.

### Rules

This part is based on my own deduction, I am not sure if they are true. Please contact me if you have other ideas! I would greatly appreciate it.

- "reducing rule": $P(A(B|C)) = P((AB)|C)$

  - just like the "conditional" operator, this rule is also tricky, cause $A$ and $B|C$ are in the different sample space. Here, I would reduce the larger sample space to the smaller sample space. That is reducing $A$ to $A|C$ actually.  By doing this, We will have 

    $$
    P(A(B|C))=P((A|C)(B|C))=P((AB)|C)
    $$
    

- "association rule": $P(A(BC)) = P((AB)C)=P(ABC)$

  - from the set view, the proof is trivial.


# Probabilistic propositional logic

So far, we define the "probability" from the set theory view and so for the operations, which could be interpreted as the set operations.

How to express uncertainty about logical propositions?

The probability of a proposition is the probability mass of all **models** of it.

- model: all **events** that make the proposition true
- event: encode assignment to all propositional symbols.

In the paper [probability, logic and probability logic](https://faculty.arts.ubc.ca/pbartha/p520w07/comp_logic.pdf), there is a definition of probability theory from the logic theory view.

S: a collection of sentences of a language, closed under finite truth-functional combinations, with the following axiomatization:

- normalization $\text{Pr}(T)=1$, If $T$ is a tautology
- non-negativity: $\Pr[A]\geq 0,\forall A\in S$
- "add operation":  $\Pr[A\lor B]=\Pr[A]+\Pr[B]$ for all $A$ and $B$ in $S$, such that $A$ and $B$ are logically incompatible.

### Examples given in the lecture

The method is:

1. degrade the logical operations, e.g. imply,  to the most basic three operations: and, or and not
2. go to the set theory, find the corresponding set of the logical expression and calculate the probability of the set

|           | Toothache | Toothache | no toothache | no toothache |
| --------- | --------- | --------- | ------------ | ------------ |
| -         | catch     | no catch  | catch        | no catch     |
| cavity    | 0.108     | 0.012     | 0.072        | 0.008        |
| no cavity | 0.016     | 0.064     | 0.144        | 0.576        |

$P(toothache)$​​​? find the corresponding set  the left two columns $P(toothache)=0.108+0.016+0.012+0.064=0.2$

$P(toothache=>cavity)$?

1. degrade into and, or and not: $A\Rightarrow B\Leftrightarrow \lnot A\vee B$

2. not toothache correspondents to the right two columnes, cavity correspondents to the first row. Then take the union of them.

   $= 0.108+0.012+0.072+0.008+0.144+0.576=0.92$