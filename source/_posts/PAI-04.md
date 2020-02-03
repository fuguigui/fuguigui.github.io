---
title: Bayesian Network 2 - Inference
date: 2020-01-26
tags: [courses, Probability, AI]
categories: Probabilistic Artificial Intelligence
---

![active](http://latex.codecogs.com/svg.latex?

# Inference

## Typical queries

- What are these typical queries for?
- what kinds of typical queries?
  - conditional/marginal query: Given conditional variable(s)' value, what is the probability of the objective variable(s)? ![cond](http://latex.codecogs.com/svg.latex?%5CPr%5BX_i%7CX_S%3Dx_s%5D%3D%3F)
  - MPE(most probable explanation): a special case of MAP. given the values for some variables, compute most likely assignment to all remaining variables: ![mpe](http://latex.codecogs.com/svg.latex?%5Carg%5Cmax%20x_%7B%5Cbar%7Bs%7D%7D%20%5CPr%5BX_%7B%5Cbar%7Bs%7D%7D%3Dx_%7B%5Cbar%7Bs%7D%7D%7CX_s%3Dx_s%5D)
  - MAP (maximum a posterior): compute most likely assignment to some variables.  Two kinds of queries: 1. the arg giving the maximum, 2. just the maximum value![mpe](http://latex.codecogs.com/svg.latex?%5Carg%5Cmax_%7Bx_a%7D%5CPr%5BX_A%3Dx_a%7CX_B%3Dx_b%5D)

Formally, any probabilistic inference system is to compute the posterior probability distribution for a set of query variables, given some observed event (that is some assignment of values to a set of evidence variables).

Define three types of variables:

- evidence variables: the set of variables give the observed event, denoted as E
- query variables, denoted as X
- hidden variables: all the variables except for evidence or query variables, denoted as Y

## Methods

- exact: exploit structure(conditional independence) to efficiently perform exact inference. Variable elimination, factor graph

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

- product: the variables are the *union* of two factors' variables, the result is the product of two factors.![prod](http://latex.codecogs.com/svg.latex?f%28X_1%2CX_2%2C%5Ccdots%2CX_n%2CY_1%2CY_2%2C%5Ccdots%2CY_k%2CZ_1%2CZ_2%2C%5Ccdots%2CZ_m%29%3Df_1%28X_1%2CX_2%2C%5Ccdots%2CX_n%2CY_1%2CY_2%2C%5Ccdots%2CY_k%29%5Ccdot%20f_2%28Y_1%2CY_2%2C%5Ccdots%2CY_k%2CZ_1%2CZ_2%2C%5Ccdots%2CZ_m%29)

- marginalization: ![margin](http://latex.codecogs.com/svg.latex?f%28B%2CC%29%3D%5Csum_%7Ba%7Df%28A%3Da%2CB%2CC%29)

#### Algorithm

Case: marginal variables

Given BN and Query P(X|E)

1. choose an ordering of X_1,...,X_n for all elements in Y.
2. set up initial factors: f_i=P(X_i|Pa_i), Pa_i, the set of nodes who have edges pointing to X_i
3. For i=1:n, X_i\in Y
   1. collect and multiply all factors that include X_i
   2. generate new factor by marginalizing out X_i, ![gene](http://latex.codecogs.com/svg.latex?g%3D%5Csum_%7Bx_i%7D%5Cprod_j%20f_j)
   3. add g to the set of factors.
4. renormalize P(X,E) to get P(X|E)

Case: maximum arguments (Similar to the former algorithm)

Given BN and evidence E=e

1. choose an ordering of X_1,...,X_n for all elements in Y.

2. set up initial factors: f_i=P(X_i|Pa_i), Pa_i, the minimum parent  set subsetting to the set of ancestors of given variable X_i. Like the definition in the building BN algorithm

3. For i=1:n, X_i\in Y

   1. collect and multiply all factors that include X_i
   2. generate new factor by marginalizing out X_i, ![genemax](http://latex.codecogs.com/svg.latex?g%3D%5Cmax_%7Bx_i%7D%5Cprod_j%20f_j)
   3. add g to the set of factors.

4. For i = n:\-1:1, X_i\in Y

   ![argmax](http://latex.codecogs.com/svg.latex?%5Chat%7Bx_i%7D%3D%5Carg%5Cmax_%7Bx_i%7Dg_i%28x_i%2C%5Chat%7Bx%7D_%7Bi%2B1%3An%7D%29)

   


#### Order
**The order matters**! But how does it matter?

In general, the time and space requirements of variable elimination are dominated by the size of the largest factor constructed during the operation of the algorithm. It turns out to be intractable to determine the optimal ordering, but several good heuristics are available.

- eliminate whichever variable minimizes the size of the next factor to be constructed. In general, we can remove any leaf node that is not a query variable or an evidence variable. -> every variable that is not an ancestor of a query variable or evidence variable is irrelevant to the query.



#### Complexity

- polytree: linear in the size of the network. The size is defined as the number of  CPT entries.
  - if the number of parents of each node is bounded by a constant, then the complexity will also be linear in the number of nodes.

**polytree**: a DAG, dropping edge directions is a tree.

- multiply connected networks: has exponential time and space complexity in the worst case, even when the number of parents per node is bounded. (???? Example???)

Special case of the Algorithm for a polytree:

1. pick a root within Y
2. orient edges (in the undirected tree) towards root.
3. eliminate in topological order (bottom up, first all the children, then the parent)

All intermediate factors will contain at most 1+max|Pa_i| variables.



### Factor graphs

Why we need factor graph?

what advantages of it?

[from the wiki](https://en.wikipedia.org/wiki/Factor_graph)

#### Definition
a factor graph is a *bipartite graph* representing the *factorization* of a function. Given a function and its factorization:

![fact](http://latex.codecogs.com/svg.latex?g%28X_1%2CX_2%2C%5Ccdots%2CX_n%29%3D%5Cprod_%7Bj%7Df_j%28S_j%29%2C%20S_j%5Csubset%5C%7BX_1%2CX_2%2C%5Ccdots%2CX_n%5C%7D%2C%5Cforall%20j%5Cin%5Bm%5D)

The corresponding factor graph: G=(X,F,E)

- X: the set of variables. One variable node for each variable.
- F: f_1,f_2,...f_m, the set of factors. One factor node for each factor.
- E: *undirected edge* between a factor vertex f_j and a variable vertex X_i iff ![fact2](http://latex.codecogs.com/svg.latex?X_i%5Cin%20S_j)

#### Message passing

Messages are real-valued *functions*. It contains the "influence" that one variable (not a factor) exerts on others. There are two kinds of message: (1) message from a variable to a factor and (2) message from a factor to a variable. Messages are always denoted by variables, not factors. That is ![mess](http://latex.codecogs.com/svg.latex?%5Cmu_%7Bv%5Cto%20u%7D%28x_v%29%20%2C%20%5Cmu_%7Bu%5Cto%20v%7D%28x_v%29) All uses x_v.

- message from a variable v to a factor u: the product of all message this variable receives from its neighbors (factors) except for the one it sends to.

  ![mess1](http://latex.codecogs.com/svg.latex?%5Cmu_%7Bv%5Cto%20u%7D%28x_v%29%3D%5Cprod_%7Bu%27%5Cin%20N%28v%29%5Csetminus%5C%7Bu%5C%7D%7D%5Cmu_%7Bu%27%5Cto%20v%7D%28x_v%29)

  Upon convergence (if convergence happened), the estimated *marginal  distribution of each variable* is proportional to the product of all messages from adjoining factors (missing the normalization constant):

  ![conv1](http://latex.codecogs.com/svg.latex?%5CPr%5BX_v%3Dx_v%5D%5Cpropto%20%5Cprod_%7Bu%5Cin%20N%28v%29%7D%20%5Cmu_%7Bu%5Cto%20v%7D%28x_v%29)

  In other words, ![conv1](http://latex.codecogs.com/svg.latex?%5CPr%5BX_v%3Dx_v%5D%5Cpropto%20%5Cmu_%7Bu%5Cto%20v%7D%28x_v%29%5Cmu_%7Bv%5Cto%20u%7D%28x_v%29)

- message from a factor to a variable: 

  - the product of all message this factor receives from its neighbors (variables) except for the one it sends to 

  - weighted by the value of the factor, fixed the objective variable.

    ![mess2](http://latex.codecogs.com/svg.latex?%5Cmu_%7Bu%5Cto%20v%7D%28x_v%29%3D%5Csum_%7Bx_u%5Csim%20x_v%7Df_u%28x_u%29%5Cprod_%7Bv%27%5Cin%20N%28u%29%5Csetminus%5C%7Bv%5C%7D%7D%5Cmu_%7Bv%27%5Cto%20u%7D%28x_%7Bv%27%7D%29)

    Max version, to get the arg of max, also needs to store the arg information for each factor.
  
    ![max2](http://latex.codecogs.com/svg.latex?%5Cmu_%7Bu%5Cto%20v%7D%28x_v%29%3D%5Cmax_%7Bx_u%5Csim%20x_v%7Df_u%28x_u%29%5Cprod_%7Bv%27%5Cin%20N%28u%29%5Csetminus%5C%7Bv%5C%7D%7D%5Cmu_%7Bv%27%5Cto%20u%7D%28x_%7Bv%27%7D%29)
    
    Likewise, the estimated joint marginal distribution of the set of  variables belonging to *one factor* is proportional to the product of the factor and the messages from the variables:
  
  ![conv2](http://latex.codecogs.com/svg.latex?%5CPr%5BX_f%3Dx_f%5D%5Cpropto%20f_f%28x_f%29%5Cprod_%7Bv%5Cin%20N%28f%29%7D%20%5Cmu_%7Bv%5Cto%20f%7D%28x_v%29)

#### Algorithm

- Exact algorithm for trees:

**Sum-product (also called belief propagation) algorithm**. It computes the functions on edges. [A detailed introduction](https://www.doc.ic.ac.uk/~mpd37/teaching/ml_tutorials/2016-11-09-Svensson-BP.pdf) and [a demo](http://mlg.eng.cam.ac.uk/teaching/4f13/1920/factor%20graphs.pdf)

Given a factor graph, choose one node as root.

There are three phases.

1. Initialization. Void product is set as 1. 
2. message passing. Compute outgoing messages when incoming messages are available.
3. termination. A marginal distribution is the product of the incoming messages to the variable node.

- approximate algorithms for loops

1. initialize all messages as uniform distribution (instead of void product as 1)
2. until converged to
   1. pick some ordering on the factor graph edges (+directions, because of loops)
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

Given unnormalized distribution P with the form of the product of factors, the idea is to try to find a "simple" (tractable) distribution Q that approximates P well. Then, compute marginals under Q* instead of P

- ![vi1](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7B1%3An%7D%5D%3D%5Cfrac%7B1%7D%7BZ%7D%5Cprod_%7Bj%3D1%7D%5Em%20%5CPhi_j%28X_%7BA_j%7D%29)

- ![vi2](http://latex.codecogs.com/svg.latex?Q%5E%2A%5Cin%5Carg%5Cmin_%7BQ%5Cin%5Cmathcal%7BQ%7D%7DKL%28Q%7C%7CP%29)

- Q is chosen from a distribution family. A simple instance: independent distributions: ![independent](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BQ%7D%3D%5C%7BQ%3AQ%28X_%7B1%3An%7D%29%3D%5Cprod_%7Bi%3D1%7D%5En%20Q_i%28X_i%29%5C%7D)

    
  

A key question is when to do this? On the prior? Or on the posterior? 
Typically, the distribution of prior may be terrible, hard to catch, could be a lot configurations to capture, but once you have evidence, the "localized probability" (posterior) is somehow simplified. Then, the approximation of the posterior is hopeful. *Posterior often much better approximated by simple distribution*. That means the posterior only needs to capture features on a very very limited, much smaller space.

#### KL Divergence

![kldiverg](http://latex.codecogs.com/svg.latex?KL%28Q%7C%7CP%29%3D%5Csum_x%20Q%28x%29%5Clog%5Cfrac%7BQ%28x%29%7D%7BP%28x%29%7D%3D%5Csum_xQ%28x%29%5Clog%20Q%28x%29-%5Csum_xQ%28x%29%5Clog%20P%28x%29)

Not a distance: not satisfying: (1)symmetricity, (2) triangle inequality.

**Entropy** of a distribution:

![ent](http://latex.codecogs.com/svg.latex?H%28Q%29%3D-%5Csum_xQ%28x%29%5Clog%20Q%28x%29)

Entropy of a product distribution, where ![prod](http://latex.codecogs.com/svg.latex?Q%28X_%7B1%3An%7D%29%3D%5Cprod_i%20Q_i%28X_i%29%2CH%28Q%29%3D%5Csum_i%20H%28Q_i%29)

A proof ketch:

![calculation](http://latex.codecogs.com/svg.latex?H%28Q%29%3D-%5Csum_x%5Cprod_i%20Q_i%28X_i%29%5Clog%5Cprod_i%20Q_i%28X_i%29%3D-%5Csum_%7Bx_j%7DQ_j%28X_j%29%5Csum_%7Bx_k%2Ck%5Cneq%20j%7D%5Cprod_%7Bk%5Cneq%20j%7DQ_k%28X_k%29%5Csum_%7Bi%7D%5Clog%20Q_i%28X_i%29) 

![calculation](http://latex.codecogs.com/svg.latex?%3D-%5Csum_%7Bx_j%7DQ_j%28X_j%29%5Clog%20Q_j%28X_j%29%5Ccdot%201+%5Csum_%7Bx_j%7DQ_j%28X_j%29H%28Q_%7B-j%7D%29%3DH%28Q_j%29%2BH%28Q_%7B-j%7D%29)



#### ELBO

evidence lower bound

![obj](http://latex.codecogs.com/svg.latex?%5Carg%5Cmin_%7BQ%5Cin%5Cmathcal%7BQ%7D%7DKL%28Q%7C%7CP%29%3D%5Carg%5Cmax_%7BQ%5Cin%5Cmathcal%7BQ%7D%7D%5Csum_%7Bi%3D1%7D%5EnH%28Q_i%29%2B%5Csum_%7Bi%3D1%7D%5Em%5Csum_%7Bx_%7BA_i%7D%7D%5Cprod_%7Bj%5Cin%20A_i%7D%20Q_j%28x_j%29%5Clog%5CPhi%28x_%7BA_i%7D%29)

Want to optimize this objective according to Q. Methods:

- gradient-based
- mean-field algorithm, also called coordinate ascent. The idea is to optimize one variable at a time.

**Mean-filed algorithm**

1. initialize. (Start with a guess, e.g. uniform distribution for each variable) Q^{(0)}
2. Operate until convergence:
   1. cycle throughout variables and update one each time. 

### Marginal as Expectations

Before, there are all *deterministic* algorithms, now we turn to randomized algorithms. Using randomness to get a trade-off between computation time and accuracy.  The core idea is treat marginals as expectations and approximate expectation by sampling. 

#### Convergence

The law of large numbers (valid on independent samples) gives a hope of convergence.
![exp](http://latex.codecogs.com/svg.latex?%5Cmathbb%7BE%7D_P%5Bf%28X%29%5D%3D%5Csum_x%20P%28x%29f%28x%29%2C%20%5Cint%20P%28x%29f%28x%29dx)

examples: 

- marginals: ![marg](http://latex.codecogs.com/svg.latex?%5CPr%5BX_i%3Dx%5D%3D%5Cmathbb%7BE%7D_P%5B%5BX_i%3Dx%5D%5D)

#### Sampling methods

How to sample from structured models (e.g. a Bayesian network)?

- forward sampling (Monte Carlo sampling), using the structure to sample step by step. Proceed according to the topological order.
  - algorithm:
    1. sort variables in topological ordering X1,...,Xn
    2. For i = 1 to n do
       1. sample xi according to the probability P(Xi|X1=x1,...,Xi-1=xi-1)
  - estimation of marginal: the mean of count of Xi = xi
  - conditional: the joint divided by the marginal. one count divides another. (**Rejection sampling**) Also, this algorithm can be *problematic* if the denominator event is very rare, could be 0 after sampling. 
- sampling directly from posterior distribution to avoid the rare events effect (the case that the denominator almost 0) MCMC: ingenious idea: using some dependent variables to sample.

#### Complexity

How many samples do we need?

- Hoeffding's inequality: bound the relation between an estimation's distance from the true expectation and its probability. 

We have two kinds of errors. Suppose C=1,

- absolute error: ![error-ab](http://latex.codecogs.com/svg.latex?%5CPr%5B%5Chat%7BP%7D%28x%29%5Cnotin%5BP%28x%29-%5Cepsilon%2C%20P%28x%29%2B%5Cepsilon%5D%5D%5Cleq%202%5Cexp%28-2N%5Cepsilon%5E2%29)

- relative error: ![error_rela](http://latex.codecogs.com/svg.latex?%5CPr%5B%5Chat%7BP%7D%28x%29%5Cnotin%20P%28x%29%281%5Cpm%5Cepsilon%29%5D%5Cleq%202%5Cexp%28-NP%28x%29%5Cepsilon%5E2%2F3%29)

  

### MCMC

The idea of MCMC is to directly sample from posterior distribution. Forward sampling couldn't do this. Assuming that given evidences appear later than some variables according to the topological order, if we do forward sampling, we start from root (parents) and may get only few of these samples with observed variables as the observed values. Forward sampling uses likelihood to sample, while MCMC uses posterior to sample.

#### Settings

Given unnormalized distribution Q(x), we want to sample from the normalized version P(X) = Q(X)/Z. For example, given P(A,B,C) but we want to sample from P(A|B,C)=P(A,B,C)/Z, Z is an unknown constant.

The solution is to create Markov chain from Q(X), which has the stationary distribution P(X) which we want to sample from.

An important statement: an

- *ergodic* 
- *Markov Chain* 
has a 
- unique 
- and positive stationary distribution \pi(X)>0, such that for all x, ![mcmc](http://latex.codecogs.com/svg.latex?%5Clim_%7Bt%5Cto%5Cinfty%7D%5CPr%5BX_t%3Dx%5D%3D%5Cpi%28x%29)

#### Detailed Balance

Detailed balance equation: ![dbe](http://latex.codecogs.com/svg.latex?Q%28x%29P%28x%27%7Cx%29%3DQ%28x%27%29P%28x%7Cx%27%29)

In the our application, it builds the transfer from likelihood probability to posterior by the replacement in conditional distribution. The equation is ![dbe1](http://latex.codecogs.com/svg.latex?Q%28X_t%3Dx%29P%28X_%7Bt%2B1%7D%3Dx%27%7CX_t%3Dx%29%3DQ%28X_%7Bt%2B1%7D%3Dx%27%29P%28X_t%3Dx%7CX_%7Bt%2B1%7D%3Dx%27%29)

#### Algorithm

a framework

1. provide a proposal distribution R(X'|X), this is chosen by ourselves. (Performance of the algorithm will strongly depend on R). 

   What does the performance mean? Convergence? Complexity?

   - bad proposal may cause alpha never been accepted. The state seldom moves.

2. acceptance distribution:

   1. suppose X_t=x
   2. accept the transition from R with probability ![prob](http://latex.codecogs.com/svg.latex?%5Calpha%3D%5Cmin%5C%7B1%2C%5Cfrac%7BQ%28x%27%29R%28x%7Cx%27%29%7D%7BQ%28x%29R%28x%27%7Cx%29%7D%5C%7D) 

Remarks:

- in the case that R(x|x') is symmetric, alpha is decided by Q(x')/Q(x). It means it will definitely transit to a value with higher probability (Q(x')>Q(x)). For a value with lower probability, it has some chance to transit to that (Q(x')/Q(x)).

Theorem: the stationary distribution is Z^{-1}Q(X).

How to design the proposal? Why this proposal needs to satisfy the detailed balance equation?

##### Gibbs sampling

Algorithm:

1. start with initial assignment x(0) to all variables
2. fix observed variables X_E to their observed values x_E
3. For t = 1 to infty do:
   1. set x^{(t)}=x^{(t-1)}
   2. For each variable X_i (not in E)
      1. set v_i=values of all x^{(t)} except x_i
      2. sample x^{(t)}_i from P(X_i|v_i)

In Gibbs sampling, we still use an unknown conditional distribution P(X_i|v_i). Good news is this special type of conditional distribution, which conditions on everything except one variable, could be calculated efficiently.  Because when calculating the denominator, we focus on a small space where only the aimed variable takes different values. The space is |V|. The whole space's size is |V|^n, hard to calculate.

Advantage:

- can do update parallel. 



Algorithm via Gibbs sampling

1. use Gibbs sampling to obtain samples, X^{(1)},...X^{(T)}

2. ignore the first t0 samples in "burn in" stage, and approximate

   ![gb-exp](http://latex.codecogs.com/svg.latex?%5Cmathbb%7BE%7D_%7Bx_E%7D%5Bf%28X%29%5D%5Csimeq%5Cfrac%7B1%7D%7BT-t_0%7D%5Csum_%7B%5Ctau%3Dt_0%2B1%7D%5ET%20f%28X%5E%7B%28%5Ctau%29%7D%29)

#### Convergence

We want to use MCMC to answer the question, what is the expectation of some function f(X) given the probability distribution. In the former, the law of large numbers works on the independent samples. Here, all of our samples are x^{(t)}, sample at time t depends on sample at time t-1. How to calculate E_p[f(X)] now?

Hopefully, we have **ergodic theorem**. This is a strong law of large numbers for Markov chain. 

Ergodic theorem: 

- suppose X1,X2,...,XN,...is an ergodic Markov chain 
- over a finite state space D, 
- with stationary distribution pi. 
- f is a function on D.

Then, we have

![ergodic](http://latex.codecogs.com/svg.latex?%5Clim_%7BN%5Cto%5Cinfty%7D%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bi%3D1%7D%5EN%20f%28x_i%29%3D%5Csum_%7Bx%5Cin%20D%7D%5Cpi%28x%29f%28x%29%3D%5Cmathbb%7BE%7D_%7Bx%5Csim%5Cpi%7D%5Bf%28x%29%5D)

- how about the convergence rate?
  - Establishing convergence rates generally very difficult. (No Hoeffding for it)

# Summary

| methods               | Deterministic? | applicable cases              | advantages                                   | disadvantages                                     |
| --------------------- | -------------- | ----------------------------- | -------------------------------------------- | ------------------------------------------------- |
| variable elimination  | Yes            | tree-structured bayes net     | exact marginals                              | not scalable to general models                    |
| belief propagation    | Yes            | tree-structed, loopy networks | efficiently compute all marginals            | may not converge on loopy networks                |
| variational inference | Yes            | tree, loopy                   | fast, converge, good if having a lot of data | but the convergence may not be the exact solution |
| gibbs sampling        | No             | tree, loopy                   | converges to exact marginals                 | may take a long time                              |

