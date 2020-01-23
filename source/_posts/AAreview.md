---
title: Advanced algorithms
date: 2020-01-22
tags: [courses, algorithms]
categories: Advanced algorithms
---



# Preface

In 2019 fall semester, I took part in the course [Advanced algorithms](https://people.inf.ethz.ch/gmohsen/AA19/), opened by [Mohsen Ghaffari](https://people.inf.ethz.ch/gmohsen/), ETH Zurich. Mohsen is a genius professor and he gives us a lot of help on this course outside the lectures. Honestly, this course is challenging for me but I have indeed learned a lot from that. Lecture notes, tutorials, graded homework and the programming projects all bring me a lot of fun. The feeling of taking this course is like playing a series of puzzles, both struggling and addictive. Eureka is a gift for brain storming. Thanks for professor Mohsen, the tutors and my colleagues David, Karolis and Rok. They make me enjoy this period very much. 

This review generally gives a summary of important methodologies of each part of this course, covering few details. Although technical details matter a lot of an algorithm, methodologies work as a guide to create concrete algorithms.  From my experience, the order of creating an algorithm is contrary to the presentation in the notes, which prove each claim first then give the last conclusion. The real birth process of an algorithm could be: I want something, how to get it? If it satisfies these, I gonna make the algorithm. How to satisfy these conditions, we design this, this that... It is motivated by some methodology. 

The order of this review follows the lecture notes, and parts are divided basically following the notes. Helpful references are also included.

There are totally four parts:

- approximation algorithms
- streaming and sketching algorithms
- graph sparsification
- online algorithms and competitive analysis



# Approximation algorithms

## Greedy algorithms

Mohsen's summary:

from the first lecture, you should take away some idea about 

- how to approach computational problems using natural "greedy" ideas and more importantly, 
- how to analyze the performance of your idea by comparing it to the best possible solution, even though we don't know the latter. This often involves 
- understanding some structure of the problem and lower bounding the performance of OPT, by basic observations, 
- and relating your algorithm's performance to these simple bounds on OPT.

For me, I learn to know

- the meaning of "approximation algorithm" when dealing with hard problems, e.g. NP-hard problems. 
- how to design the algorithm: usually very intuitive. Greedy approach means local optimal, the best strategy for each step.
- how to analyze the approximation ratio of the designed algorithm:
  - formally write the objective function
  - build inequality (lower bound/upper bound) from the designed algorithm and the property of question for OPT and the designed algorithm
  - relates c(ALG) to c(OPT)

## Approximation schemes

Mohsen's summary: 

from the PTAS, FPTAS lectures, you take away the idea that,

-  sometimes, by slightly changing the parameters of the problem (e.g., by rounding the input data), you can 
  - turn the problem into a much easier one---computationally---
  - while still not sacrificing too much in the quality of the
    solution. 
-  Designing such schemes usually involves understanding what kind of instances are easier computationally (e.g., by building a corresponding Dynamic Programming) as well as
-  getting a feeling for the parameters that can be changed without introducing too much error --- for instance, recall the intuitive discussion in the class about soft constraints and hard constraints.

For me, I learn to:

- the framework of PTAS(FPTAS): transfer from an exact solution of a special problem to an approximate algorithm of a more general problem.

  1. design an exact solution of a special case
  2. analyze the time complexity, find which part is computational expensive, beyond polynomial in other words
  3. round the general problem to the special case to deal with expensive parts, may involve classifying the instances and process each of them by different approaches.

- Creative points lie on the round strategy. That is how to round to trade off the computational complexity and the precision. For instance, to deal with the minimum makespan problem, inspired by the 4/3-approximation algorithm, we get a general method:

  - split the whole jobs as short jobs and long jobs
    - for long jobs: we compute the optimal schedule for them
    - for short jobs: we extend that partial schedule by using FirstFit algorithm.
    - In this way, we trade off between the number of long jobs and the quality of solutions.
  - bound the "polynomial" property on m: the key idea is that we didn't really need the schedule for the long jobs to be optimal. We use the optimality of the schedule for the long jobs only when the last job to finish was a long job. If we have found a schedule for the long jobs that have makespan at most 1+1/k times the optimal value, that clearly would have been sufficient.

## Randomized approximation schemes

Mohsen's summary:

from the FPRAS lectures, you should 

- recall the idea of Monte Cato sampling for estimating various problems. 
- Moreover, you can get an understanding of why this natural and frequently used idea, on its own, might be insufficient for some problems, when the quantity that is to be estimated is only a tiny fraction of the whole (sampling) space. 
- Then, we saw some schemes that allow us to view the target quantity as a simple function of other quantities that can be computed/estimated more efficiently. For instance, for DNF counting, the number of satisfying assignments for each clause is easy to compute and we saw how to estimate the total number of satisfying assignments of the whole formula by, roughly speaking, estimating for each clause the fraction of satisfying assignments for which this clause can take the credit (i.e., is the first satisfied clause), weighted by the number of satisfying assignments for each clause.

For me, I learn:

- the sampling trick when the whole space is "much larger" (e.g. in exponential level) than the goal space
  - redefine the whole space with some properties.
  - sample step by step 
- express the objective by newly defined random variable to make the expectation of this expression as our objective.
- design sampling times to satisfy the approximation requirement.

## Rounding ILPs

Mohsen's summary:

from the LP rounding lecture, you should take away the idea of 

- how to formulate some optimization problems as a linear programs (with a linear objective function and linear constraints
  on your variables) 
- and how you can transform a fractional solution of this LP to an integral solutions by, e.g., randomized rounding. 
- You should also start building an intuition for when just
  a deterministic rounding suffices-e.g., think about the minimum-weight vertex cover problem, and
- moreover, how to use probabilistic analysis to argue about the performance of your randomly rounded integral solutions, in comparison with the corresponding fractional solution.

For me, I also learn:

- the way to round back the integer solution to the original problem, by virtue of probability interpretation of the solution.

## Tree Embedding

Mohsen's summary:

from the tree imbedding lectures you can take away 

- the existence of a tree that "approximates" distances in a general graph (actually, a random tree or formally a probability distribution over a collection of trees) and 
- how this enables us to build approximation algorithm for some graph problems, by 
  - first transforming the input graph to a tree---via tree embeddings---, 
  - then solving the problem on that tree, 
  - and finally projecting the solution back to the original graph.
- We then have to argue about the quality of the solution. Often, we just lose an O(logn) factor in the approximation, because of how much the tree stretches the distances, in expectation.

The summary is enough for me.

# Streaming and sketching algorithms

In this chapter, we discuss the algorithms under the "streaming data" situation, where exact and deterministic algorithms may not work. I have learnt how to save memory space by randomization and  how to build proper data summary with limited size to approximate the objective, and some tricks to increase the success probability. 

## Streaming algorithms

The general framework for the streaming algorithms:

- randomize the "original" expression (the  trick is called "**random projection**" )and update summary when some condition satisfied. For instance
  - in estimating the zero-th moment, each number is uniformly hashed to an unary expression
  - in estimating the first moment, we take a uniform distributed variable for each element (not number)
  - in estimating the second moment, each number is mapped randomly to a predefined label {+1,-1}.
- design a summary whose expectation is our objective
- make the probability of being away from the objective is smaller than a "big enough" number (e.g. any little constant is enough)
- increase the success probability by some trick:
  - mean trick: to reduce the variance. If we can limit the probability of the variable being away from the expectation by using Chebyshev inequality (involving variance in the proof process), we can make it very low. Make new X' as the mean of T independent X. If we want the probability less than \delta, T = O(1/\sqrt{\delta})
  - median trick: if one single event's success probability is larger than 0.5, we can take the median of T independent events, whose success probability could be as large as we want. T = O(log 1/\delta)
- A useful trick: logarithmic trick.  For some upper bound M, consider the sequence (1+\epsilon), (1+\epsilon)^2, \cdots, (1+\epsilon)^{log_{1+\epsilon}M}. We can approximate by log M turns of experiment instead of M turns.

## Graph sketching

In this lecture, we learn to use limited-memory algorithms to store graph information presented by an edge stream and to keep some desired properties.

From the survey [Graph Stream Algorithms: A Surve](http://alpha.luc.ac.be/~lucg5503/sr/mcg.pdf), the algorithms of graph streaming data must do the following things:

- process the input stream in the order it arrives
- use a limited amount memory

In other words, these algorithms should solve the following questions:

- how to trade-off size and accuracy when constructing data summaries
- how to quickly update these summaries

A classic algorithm is called "semi-streaming model", which uses O(n polylogn) memory to store the graph.

The common framework is to 

1. find a trivial algorithm on the offline whole graph
2. represent the graph using suitable trick and apply the algorithm on newly representations.

# Graph Sparsification

The learning objective is to sparsify a graph with fewer edges but still keep the distance between pairs of nodes not very long. Different approaches are applied corresponding to different constraint of lengths.

- multiplicative constraint: use a family of graph with girth larger than a constant k. Such a graph has the property of trade-off between girth and edge numbers. The approach is to build such a graph from the original graph.
- additive constraint: classify the nodes as light nodes and heavy nodes according to their degree. 
  - Light nodes could not have too many edges 
  - For heavy nodes, we can do some sampling on nodes and add BFS tree (promise the distance between the root and other nodes) rooted at sampled nodes. Since they are heavy, they have a large probability of having a sampled neighbor. 

# Online algorithms and competitive analysis

In this chapter, we learn to design algorithms with incomplete information, how to compare its performance with algorithms with complete information and how to design adversary against an online algorithm to prove the competitive ratio.  There are several important concepts/ideas in this chapter, basically refered from [on-line algorithms versus off-line algorithms: how much is it worth to know the future?](https://www.icsi.berkeley.edu/pubs/techreports/TR-92-044.pdf).

## Online VS offline algorithms:

- online: 
  - receive a sequence of requests
  - perform an immediate action in response to each request
- offline:
  - receive the entire sequence of requests in advance
  - take an action in response to each request, but the choice of each action can be based on the entire sequence of requests.

Deterministic online algorithms: suppose there is a function f from the sequence of requests to an action, such that the function is deterministic. Then the algorithm A(r=r1r2...rt) = f(r1)f(r1r2)...f(r1r2...rt) is a deterministic online algorithm.

## Competitive analysis

It is a method to analyze the performance of an online algorithm compared with the optimal offline algorithm

Bounds the ratio between the worst case behavior of the algorithm on a problem instance and the behavior of the optimal algorithm (not necessarily well defined) on the same problem instance.

- competitive ratio of an algorithm: the worst case ratio of its cost divided by the optimal cost over all possible inputs
- competitive ratio of an online problem: the best competitive ratio achieved by an online algorithm

### Potential function

It is used to do competitive analysis. Where does it come from? We want to compare the performance between the designed algorithm and the optimal. We want to quantify the potential of the OPT to outperform the online algorithm.

There are two cases for the performances of OPT and the online algorithm:

- at a step in which the algorithm's actions sequence coincide with the optimal action sequence, the two will have the same cost
- when these two take different actions, the OPT has the potential to outperform the online algorithm. So, it is usually defined related to the difference between the OPT and the online one.

Potential function is defined to measure the extent to which the two sequences differ, and hence the potential for OPT to outperform the online algorithm.

## Adversary constructions

It is a main approach to prove the lower bounds on the competitive ratio achievable of an online deterministic algorithm for a given problem. The framework:

- given any deterministic online algorithm as input
- produce an infinite family R of request sequences such that as the length of the sequence r\in R increases, the ratio c(A(r))/c(OPT(r)) eventually exceeds every number less than k (a constant, the lower bound of the competitive ratio)

There are three types of adversary. They can be used to test if a randomized online algorithm could be better than a deterministic online algorithm. In other words, to test if randomization could bring any advantage. These three have the same power to a deterministic algorithm but may have different power to a randomized algorithm. So, when speaking the competitivity of a randomized algorithm, we should point out which kind of adversary is used.

Three types of adversary:

- oblivious
- adaptive
- fully adaptive

Two important theorems:

- if there is a C-competitive randomized algorithm against fully adaptive adversary, then there is a C-competitive deterministic algorithm. In other words, there is no advantage to randomize when playing against fully adaptive adversaries.
- If there is a C-competitive randomized algorithm against oblivious adversaries and a D-competitive randomized algorithm against adaptive adversaries, then there is a  CD-competitive deterministic algorithm. It follows that the competitive ratio achievable by a randomized algorithm against adaptive online adversary is at least the square root of the best competitive ratio achievable by a deterministic algorithm.