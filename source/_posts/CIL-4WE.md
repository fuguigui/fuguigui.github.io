---
title: CIL4 Word Embeddings
date: 2019-07-10
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
---

# Motivation

Using **vector** as the symbol to represent words, which can capture their meaning in some way.

- the advantage of vector representation?
  
- maybe capture the meaning closeness of words. e.g. similar words have similar vectors.
  
- how do we know if the embedding is good?
  - usually, we use the prediction ability to evaluate the goodness. 
  
  - skip-ram model (distributional context model): a model of prediction. Given a context word, learn the probability distribution of all the vocabularies. Our objective is to maximize: 
    $$
    L(\theta;w) = \sum_{t=1}^T \sum_{\Delta\in I}\log p_\theta(w^{t+\Delta}|w^{(t)})
    $$
    the context is a point, the predicted words are a window.
  
  - CBOW (continuous bag-of-word model): Contrary to the skip-ram model, given the context words, learn the probability of this word. The context is a window, the predicted word is a point.

# Models

## Latent Vector Model

$$
w\mapsto (\vec{x_w},b_w)\in\mathbb{R}^{d+1}
$$

represent a word by a vector and a scalar bias.

- what is the function of bias?
  - to make sure that some words are more likely than other words **under all conditioning**. Reflected by the definition of probabilistic:
    $$
    \log p_\theta(w|w')=<x_w, x_{w'}> + b_w+\text{const}(w')
    $$
  
    -  the const is to guarantee the sum conditioned on $w'$ is 1.
    
  - it is not necessary but very helpful to have the bias item.
  **Inner product** is a natural way to evaluate the similarity of vectors
### Objective
$$
L(\theta;w)=\sum_{t=1}^T \sum_{\Delta\in I}[\log b_{w(t+\Delta)}+\log <x_{w(t+\Delta)},x_{w(t)}>-\log\sum_{v\in V}\exp[<x_v, x_{w(t)}> + b_v]]
$$

### Modification

- why to distinguish the main vocabulary and context vocabulary? 

  - if we don't distinguish, a word as to be predicted and as the context will have the same representation. But the reality isn't like that. If we use different representation when a word has different roles, it will add the expression ability of the model.

  - symmetric could be a problem. For instance, according to the bayesian rule: 

    $$
    p(w,w')=p(w|w')p(w')=p(w'|w)p(w)
    $$
    if $w$ and $w'$ have different probability, the two conditional probabilities also should be different. In other words, these two are not symmetric.

- how to deal with the exponential item?

  - see the below models

### Negative sampling

This is a method of sampling. It is natural to use sampling when the object is exponentially big. One classic sampling method is MCMC.

- sampling: In the maximum likelihood estimation, each time for one context word, we need to update all the other words vectors, represented by the exponential items. This is very time-consuming. Instead, we only update a few words' vectors. These few words are selected by sampling. 

- negative: all the words not the one we expected are called negative words. 

  - how to know which word is our expected word? the word itself as the context must be a positive word.

- how to update the word vectors?

- what is the relationship between Negative sampling and PMI?

## Glove: Global vectors for word representation

  The key idea of GloVe is: 

  1. extract a count matrix from the given dataset. 

     - Each row is a word in the vocabulary
     - Each column is a context
     - Each entry $n_{ij}$ is the appearance times of the word i in the context $j$

  2. construct another matrix from the vector representation of vocabularies and contexts.

     - each entry is the inner product of the vocabulary vector and the context vector.

  3. update vectors to make these two matrices as close as possible.

     optional: using weight function to weight items with different counts.

  In this way, the vector representation problem is transferred as a matrix factorization problem.

$$
\min_{X,Y}=\|M-X^\top Y\|_F^2
$$

- how does GloVe solve the exponential problem?

  - it uses the **unnormalized** "probability" instead of the normalized ones to avoid the exponential item. Because the exponential item works for normalization. 

# Methods

## SGD: Stochastic gradient descent

- The objective function: $f(x)=\sum_i f_i(x)$

- Traditional gradient descent way:

  $$
  \frac{\partial f}{\partial x}=\sum_i \frac{\partial f_i}{\partial x}
  $$
  
- Stochastic way:

  $$
  \frac{\partial f}{\partial x}=\frac{\partial f_\gamma}{\partial x}, \gamma\sim\text{Uniform}(1,n)
  $$
  Key Idea: 
  
  - derivative
  - only part of objective
  - stochastic each time

# Left Questions

- the relationship of Negative Sampling and PMI?