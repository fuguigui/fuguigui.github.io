---
title: PAI3 Bayesian Network 2Inference
date: 2020-01-26
tags: [AI, Reinforcement Learning, Bayesian]
categories: [Learning Notes]
mathjax: true
---

# Inference

## Typical queries

- What are these typical queries for?
- what kinds of typical queries?
  - conditional/marginal query: Given conditional variable $s$' value, what is the probability of the objective variable $s$? $\Pr[X_i|X_S=x_s]=?$​
  - MPE, most probable explanation: a special case of MAP. given the values for some variables, compute most likely assignment to all remaining variables: $\arg\max x_{\bar{s}} \Pr[X_{\bar{s}}=x_{\bar{s}}|X_s=x_s]$
  - MAP, maximum a posterior: compute most likely assignment to some variables.  Two kinds of queries: 1. the arg giving the maximum, 2. just the maximum value $\arg\max_{x_a}\Pr[X_A=x_a|X_B=x_b]$

Formally, any probabilistic inference system is to compute the posterior probability distribution for a set of query variables, given some observed event, that is some assignment of values to a set of evidence variables.

Define three types of variables:

- evidence variables: the set of variables give the observed event, denoted as $E$
- query variables, denoted as $X$
- hidden variables: all the variables except for evidence or query variables, denoted as $Y$

## Methods

- exact: exploit structure/conditional independence to efficiently perform exact inference. Variable elimination, factor graph

- approximation: loopy factor graph

### Variable elimination

It is called variable elimination because it eliminates one by one variables which are irrelevant for the query.

It lies on some **basic operations** on a class of functions known as **factors**.

It uses an algorithmic technique called **dynamic programming**.

The general idea:

- push sums through products as far as possible
- create new factor by summing out variables, intermediate solutions are distributions on fewer variables.


#### Factor
**Factor** is a function over a set of variables. It maps each instantiation of these variables to a real number. For instance, CPT(conditional probability table)s  are factors. In other words, a factor is a matrix indexed by the values of its argument variables. It has three operations:

- product: the variables are the *union* of two factors' variables, the result is the product of two factors.
  $$
  f(X_1,X_2,\cdots,X_n,Y_1,Y_2,\cdots,Y_k,Z_1,Z_2,\cdots,Z_m)
  $$

  $$
  =f_1(X_1,X_2,\cdots,X_n,Y_1,Y_2,\cdots,Y_k)\cdot f_2(Y_1,Y_2,\cdots,Y_k,Z_1,Z_2,\cdots,Z_m)
  $$

- marginalization: $f(B,C)=\sum_{a}f(A=a,B,C)$

#### Algorithm

Case: marginal variables

Given BN and Query $P(X|E)$

1. choose an ordering of $X_1,\cdots,X_n$ for all elements in $Y$.
2. set up initial factors: $f_i=P(X_i|Pa_i), Pa_i,$ the set of nodes who have edges pointing to $X_i$
3. For $i=1:n, X_i\in Y$
   1. collect and multiply all factors that include $X_i$
   2. generate new factor by marginalizing out $X_i$, $g=\sum_{x_i}\prod_j f_j$
   3. add $g$ to the set of factors.
4. renormalize $P(X,E)$ to get $P(X|E)$

Case: maximum arguments, similar to the former algorithm

Given BN and evidence $E=e$

1. choose an ordering of $X_1,\cdots,X_n$​ for all elements in $Y$.

2. set up initial factors: $f_i=P(X_i|Pa_i), Pa_i,$​ the minimum parent  set subsetting to the set of ancestors of given variable $X_i$. Like the definition in the building BN algorithm

3. For $i=1:n, X_i\in Y$

   1. collect and multiply all factors that include $X_i$
   2. generate new factor by marginalizing out $X_i$,  $g=\max_{x_i}\prod_j f_j$
   3. add g to the set of factors.

4. For $i = n:\-1:1, X_i\in Y$

   $$
   \hat{x_i}=\arg\max_{x_i}g_i(x_i,\hat{x}_{i+1:n})
   $$


#### Order
**The order matters**! But how does it matter?

In general, the time and space requirements of variable elimination are dominated by the size of the largest factor constructed during the operation of the algorithm. It turns out to be intractable to determine the optimal ordering, but several good heuristics are available.

- eliminate whichever variable minimizes the size of the next factor to be constructed. In general, we can remove any leaf node that is not a query variable or an evidence variable.$\to$ every variable that is not an ancestor of a query variable or evidence variable is irrelevant to the query.

#### Complexity

- polytree: linear in the size of the network. The size is defined as the number of  CPT entries.
  - if the number of parents of each node is bounded by a constant, then the complexity will also be linear in the number of nodes.

**polytree**: a DAG, dropping edge directions is a tree.

- multiply connected networks: has exponential time and space complexity in the worst case, even when the number of parents per node is bounded. ???? Example???

Special case of the Algorithm for a polytree:

1. pick a root within Y
2. orient edges in the undirected tree towards root.
3. eliminate in topological order bottom up: first all the children, then the parent

All intermediate factors will contain at most $1+\max|Pa_i|$ variables.

### Factor graphs

Why we need factor graph?

what advantages of it?

[from the wiki](https://en.wikipedia.org/wiki/Factor_graph)

#### Definition
a factor graph is a *bipartite graph* representing the *factorization* of a function. Given a function and its factorization:

$$
g(X_1,X_2,\cdots,X_n)=\prod_{j}f_j(S_j), S_j\subset\{X_1,X_2,\cdots,X_n\},\forall j\in[m]
$$


The corresponding factor graph: $G=(X,F,E)$

- $X$: the set of variables. One variable node for each variable.
- $F: f_1,f_2,...f_m$, the set of factors. One factor node for each factor.
- $E$: *undirected edge* between a factor vertex $f_j$ and a variable vertex $X_i$ iff $X_i\in S_j$

#### Message passing

Messages are real-valued *functions*. It contains the "influence" that one variable ,not a factor, exerts on others. There are two kinds of message: 

1. message from a variable to a factor and

2. message from a factor to a variable. 

   Messages are always denoted by variables, not factors. That is $\mu_{v\to u}(x_v) , \mu_{u\to v}(x_v)$.  All uses $x_v$.

- message from a variable v to a factor u: the product of all message this variable receives from its neighbors/factors except for the one it sends to.

  $$
  \mu_{v\to u}(x_v)=\prod_{u'\in N(v)\setminus\{u\}}\mu_{u'\to v}(x_v)
  $$
  

  Upon convergence if convergence happened, the estimated *marginal  distribution of each variable* is proportional to the product of all messages from adjoining factors missing the normalization constant:
  $$
  \Pr[X_v=x_v]\propto \prod_{u\in N(v)} \mu_{u\to v}(x_v)
  $$
  

  In other words, 
  $$
  \Pr[X_v=x_v]\propto \mu_{u\to v}(x_v)\mu_{v\to u}(x_v)
  $$

- message from a factor to a variable: 

  - the product of all message this factor receives from its neighbors variables except for the one it sends to 

  - weighted by the value of the factor, fixed the objective variable.

    $$
    \mu_{u\to v}(x_v)=\sum_{x_u\sim x_v}f_u(x_u)\prod_{v'\in N(u)\setminus\{v\}}\mu_{v'\to u}(x_{v'})
    $$
    
    
    Max version, to get the arg of max, also needs to store the arg information for each factor.
    
    $$
    \mu_{u\to v}(x_v)=\max_{x_u\sim x_v}f_u(x_u)\prod_{v'\in N(u)\setminus\{v\}}\mu_{v'\to u}(x_{v'})
    $$
    
    
    Likewise, the estimated joint marginal distribution of the set of  variables belonging to *one factor* is proportional to the product of the factor and the messages from the variables:
  
  $$
  \Pr[X_f=x_f]\propto f_f(x_f)\prod_{v\in N(f)} \mu_{v\to f}(x_v)
  $$

#### Algorithm

- Exact algorithm for trees:

**Sum-product (also called belief propagation) algorithm**. It computes the functions on edges. [A detailed introduction](https://www.doc.ic.ac.uk/~mpd37/teaching/ml_tutorials/2016-11-09-Svensson-BP.pdf) and [a demo](http://mlg.eng.cam.ac.uk/teaching/4f13/1920/factor%20graphs.pdf)

Given a factor graph, choose one node as root.

There are three phases.

1. Initialization. Void product is set as 1. 
2. message passing. Compute outgoing messages when incoming messages are available.
3. termination. A marginal distribution is the product of the incoming messages to the variable node.

- approximate algorithms for loops

1. initialize all messages as uniform distribution, instead of void product as 1
2. until converged to
   1. pick some ordering on the factor graph edges +directions, because of loops
   2. update messages according to this ordering
   3. break once all messages change by at most epsilon



Remarks:

- Does loopy BP always converge?

  - No. The more "deterministic" of the initial distribution, the less likely that this algorithm will converge.

- If it converges, is it closed to the true value?

  - This convergence may **not** always equal to the true marginals. It is often **overconfident**, having a posterior bigger than the true posterior.

#### Application

Factor graph can be used to compute **marginal** probability with low computation complexity.

### Variational inference

Compared to factor graphs, this method always converges.

#### Settings

Given unnormalized distribution P with the form of the product of factors, the idea is to try to find a "simple" (tractable distribution Q that approximates P well. Then, compute marginals under Q* instead of P

- $\Pr[X_{1:n}]=\frac{1}{Z}\prod_{j=1}^m \Phi_j(X_{A_j})$

- $Q^*\in\arg\min_{Q\in\mathcal{Q}}KL(Q||P)$

- Q is chosen from a distribution family. A simple instance: independent distributions: $\mathcal{Q}=\{Q:Q(X_{1:n})=\prod_{i=1}^n Q_i(X_i)\}$


A key question is when to do this? On the prior? Or on the posterior? 
Typically, the distribution of prior may be terrible, hard to catch, could be a lot configurations to capture, but once you have evidence, the "localized probability" posterior is somehow simplified. Then, the approximation of the posterior is hopeful. *Posterior often much better approximated by simple distribution*. That means the posterior only needs to capture features on a very very limited, much smaller space.

#### KL Divergence

$$
KL(Q||P)=\sum_x Q(x)\log\frac{Q(x)}{P(x)}=\sum_xQ(x)\log Q(x)-\sum_xQ(x)\log P(x)
$$

Not a distance: not satisfying: (1)symmetricity, (2) triangle inequality.

**Entropy** of a distribution:

$$
H(Q)=-\sum_xQ(x)\log Q(x)
$$
Entropy of a product distribution, where $Q(X_{1:n})=\prod_i Q_i(X_i),H(Q)=\sum_i H(Q_i)$

A proof ketch:

$$
 H(Q)=-\sum_x\prod_i Q_i(X_i)\log\prod_i Q_i(X_i)
$$

$$
=-\sum_{x_j}Q_j(X_j)\sum_{x_k,k\neq j}\prod_{k\neq j}Q_k(X_k)\sum_{i}\log Q_i(X_i)
$$

$$
=-\sum_{x_j}Q_j(X_j)\log Q_j(X_j)\cdot 1+\sum_{x_j}Q_j(X_j)H(Q_{-j})
$$

$$
=H(Q_j)+H(Q_{-j})
$$

#### ELBO

evidence lower bound

$$
\arg\min_{Q\in\mathcal{Q}}KL(Q||P)=\arg\max_{Q\in\mathcal{Q}}\sum_{i=1}^nH(Q_i)+\sum_{i=1}^m\sum_{x_{A_i}}\prod_{j\in A_i} Q_j(x_j)\log\Phi(x_{A_i})
$$
Want to optimize this objective according to Q. Methods:

- gradient-based
- mean-field algorithm, also called coordinate ascent. The idea is to optimize one variable at a time.

**Mean-filed algorithm**

1. initialize. Start with a guess, e.g. uniform distribution for each variable: $Q^{(0)}$
2. Operate until convergence:
   1. cycle throughout variables and update one each time. 

### Marginal as Expectations

Before, there are all *deterministic* algorithms, now we turn to randomized algorithms. Using randomness to get a trade-off between computation time and accuracy.  The core idea is treat marginals as expectations and approximate expectation by sampling. 

#### Convergence

The law of large numbers, valid on independent samples, gives a hope of convergence.
$$
\mathbb{E}_P[f(X)]=\sum_x P(x)f(x), \int P(x)f(x)\mathrm{d}x
$$
examples: 

- marginals: $\Pr[X_i=x]=\mathbb{E}_P[[X_i=x]]$

#### Sampling methods

How to sample from structured models, e.g. a Bayesian network?

- forward sampling Monte Carlo sampling, using the structure to sample step by step. Proceed according to the topological order.
  - algorithm:
    1. sort variables in topological ordering $X_1,\cdots,X_n$
    2. For $i = 1$ to n do
       1. sample $x_i$ according to the probability $P(X_i|X_1=x_1,...,X_{i-1}=x_{i-1})$
  - estimation of marginal: the mean of count of $X_i = x_i$
  - conditional: the joint divided by the marginal. one count divides another. **Rejection sampling**. Also, this algorithm can be *problematic* if the denominator event is very rare, could be 0 after sampling. 
- sampling directly from posterior distribution to avoid the rare events effect, the case that the denominator almost 0, MCMC: ingenious idea: using some dependent variables to sample.

#### Complexity

How many samples do we need?

- Hoeffding's inequality: bound the relation between an estimation's distance from the true expectation and its probability. 

We have two kinds of errors. Suppose $C=1$,

- absolute error: $\Pr[\hat{P}(x)\notin[P(x)-\epsilon, P(x)+\epsilon]]\leq 2\exp(-2N\epsilon^2)$

- relative error: $\Pr[\hat{P}(x)\notin P(x)(1\pm\epsilon)]\leq 2\exp(-NP(x)\epsilon^2/3)$


### MCMC

The idea of MCMC is to directly sample from posterior distribution. Forward sampling couldn't do this. Assuming that given evidences appear later than some variables according to the topological order, if we do forward sampling, we start from root/parents and may get only few of these samples with observed variables as the observed values. Forward sampling uses likelihood to sample, while MCMC uses posterior to sample.

#### Settings

Given unnormalized distribution $Q(x)$, we want to sample from the normalized version $P(X) = Q(X)/Z$. For example, given $P(A,B,C)$ but we want to sample from $P(A|B,C)=P(A,B,C)/Z$, $Z$ is an unknown constant.

The solution is to create Markov chain from $Q(X)$, which has the stationary distribution $P(X)$ which we want to sample from.

An important statement: an

- *ergodic* 
- *Markov Chain* 
has a 
- unique 
- and positive stationary distribution $\pi(X)>0$, such that for all $x$,$\lim_{t\to\infty}\Pr[X_t=x]=\pi(x)$

#### Detailed Balance

Detailed balance equation: $Q(x)P(x'|x)=Q(x')P(x|x')$ 

In the our application, it builds the transfer from likelihood probability to posterior by the replacement in conditional distribution. The equation is 
$$
Q(X_t=x)P(X_{t+1}=x'|X_t=x)=Q(X_{t+1}=x')P(X_t=x|X_{t+1}=x')
$$


#### Algorithm

a framework

1. provide a proposal distribution $R(X'|X)$​, this is chosen by ourselves. Performance of the algorithm will strongly depend on $R$. 

   What does the performance mean? Convergence? Complexity?

   - bad proposal may cause alpha never been accepted. The state seldom moves.

2. acceptance distribution:

   1. suppose $X_t=x$
   2. accept the transition from R with probability $\alpha=\min\{1,\frac{Q(x')R(x|x')}{Q(x)R(x'|x)}\}$

Remarks:

- in the case that $R(x|x')$ is symmetric, alpha is decided by $Q(x')/Q(x)$. It means it will definitely transit to a value with higher probability $Q(x')>Q(x)$​. For a value with lower probability, it has some chance to transit to that $Q(x')/Q(x)$.

Theorem: the stationary distribution is $Z^{-1}Q(X).$

How to design the proposal? Why this proposal needs to satisfy the detailed balance equation?

##### Gibbs sampling

Algorithm:

1. start with initial assignment $x(0)$ to all variables
2. fix observed variables $X_E$ to their observed values $x_E$
3. For t = 1 to infty do:
   1. set $x^{(t)}=x^{(t-1)}$
   2. For each variable $X_i$ (not in E)
      1. set $v_i$=values of all $x^{(t)}$ except $x_i$
      2. sample $x^{(t)}_i$ from $P(X_i|v_i)$

In Gibbs sampling, we still use an unknown conditional distribution $P(X_i|v_i)$. Good news is this special type of conditional distribution, which conditions on everything except one variable, could be calculated efficiently.  Because when calculating the denominator, we focus on a small space where only the aimed variable takes different values. The space is $|V|$. The whole space's size is $|V|^n$, hard to calculate.

Advantage:

- can do update parallel. 

Algorithm via Gibbs sampling

1. use Gibbs sampling to obtain samples, $X^{(1)},\cdots, X^{(T)}$

2. ignore the first $t_0$ samples in "burn in" stage, and approximate

   $$
   \mathbb{E}_{x_E}[f(X)]\simeq\frac{1}{T-t_0}\sum_{\tau=t_0+1}^T f(X^{(\tau)}
   $$

#### Convergence

We want to use MCMC to answer the question, what is the expectation of some function $f(X)$ given the probability distribution. In the former, the law of large numbers works on the independent samples. Here, all of our samples are $x^{(t)}$​, sample at time t depends on sample at time $t-1$. How to calculate $E_p[f(X)]$ now?

Hopefully, we have **ergodic theorem**. This is a strong law of large numbers for Markov chain. 

Ergodic theorem: 

- suppose $X_1,X_2,\cdots,X_N, \cdots$ is an ergodic Markov chain 
- over a finite state space D, 
- with stationary distribution $\pi$. 
- $f$ is a function on D.

Then, we have

$$
\lim_{N\to\infty}\frac{1}{N}\sum_{i=1}^N f(x_i)=\sum_{x\in D}\pi(x)f(x)=\mathbb{E}_{x\sim\pi}[f(x)]
$$


- how about the convergence rate?
  - Establishing convergence rates generally very difficult. No Hoeffding for it

# Summary

| methods               | Deterministic? | applicable cases              | advantages                                   | disadvantages                                     |
| --------------------- | -------------- | ----------------------------- | -------------------------------------------- | ------------------------------------------------- |
| variable elimination  | Yes            | tree-structured bayes net     | exact marginals                              | not scalable to general models                    |
| belief propagation    | Yes            | tree-structed, loopy networks | efficiently compute all marginals            | may not converge on loopy networks                |
| variational inference | Yes            | tree, loopy                   | fast, converge, good if having a lot of data | but the convergence may not be the exact solution |
| gibbs sampling        | No             | tree, loopy                   | converges to exact marginals                 | may take a long time                              |

