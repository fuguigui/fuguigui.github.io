---
title: Word Embeddings
date: 2019-07-10
categories: Computational Intelligence Lab
---

#  Word Embeddings

## Motivation

Using **vector** as the symbol to represent words, which can capture their meaning in some way.

- the advantage of vector representation?
  
- maybe capture the meaning closeness of words. e.g. similar words have similar vectors.
  
- how do we know if the embedding is good?
  - usually, we use the prediction ability to evaluate the goodness. 
  
  - skip-ram model (distributional context model): a model of prediction. Given a context word, learn the probability distribution of all the vocabularies. Our objective is to maximize: ![img](http://latex.codecogs.com/svg.latex?L%28%5Ctheta%3Bw%29%3D%5CSigma_%7Bt%3D1%7D%5ET%5CSigma_%7B%5CDelta%5Cin%20I%7D%5Clog p_%7B%5Ctheta%7D%28w%5E%7Bt%2B%5CDelta%7D%7Cw%5E%7B%28t%29%7D%29)
  
    the context is a point, the predicted words are a window.
  
  - CBOW (continuous bag-of-word model): Contrary to the skip-ram model, given the context words, learn the probability of this word. The context is a window, the predicted word is a point.

## Models

### Latent Vector Model

![img](http://latex.codecogs.com/svg.latex?w%5Cmapsto%28%5Cvec%7Bx%7D_w%2Cb_w%29%5Cin%20R%5E%7Bd%2B1%7D)

represent a word by a vector and a scalar bias.

- what is the function of bias?
  - to make sure that some words are more likely than other words **under all conditioning**. Reflected by the definition of probabilistic:
    ![img](http://latex.codecogs.com/svg.latex?%5Clog%20p_%7B%5Ctheta%7D%28w%7Cw%27%29%3D%3Cx_w%2Cx_%7Bw%27%7D%3E%2Bb_w%2Bconst%28w'%29)
    -  the const is to guarantee the sum conditioned on w' is 1.
  - it is not necessary but very helpful to have the bias item.
**Inner product** is a natural way to evaluate the similarity of vectors
#### Objective
 ![img](http://latex.codecogs.com/svg.latex?L%28%5Ctheta%3Bw%29%3D%5CSigma_%7Bt%3D1%7D%5ET%5CSigma_%7B%5CDelta%5Cin%20I%7D%5B%5Clog b_%7Bw%28t%2B%5CDelta%29%7D%2B%5Clog%5Clangle x_%7Bw%28t%2B%5CDelta%29%7D%2Cx_%7Bw%28t%29%7D%5Crangle-%5Clog%5CSigma_%7Bv%5Cin%20V%7D%5Cexp%5B%5Clangle x_v%2Cx_%7Bw%28t%29%7D%5Crangle%2Bb_v%5D%5D)

#### Modification

- why to distinguish the main vocabulary and context vocabulary? 

  - if we don't distinguish, a word as to be predicted and as the context will have the same representation. But the reality isn't like that. If we use different representation when a word has different roles, it will add the expression ability of the model.

  - symmetric could be a problem. For instance, according to the bayesian rule:

    ![img](http://latex.codecogs.com/svg.latex?p%28w%2Cw%27%29%3Dp%28w%7Cw%27%29p%28w%27%29%3Dp%28w%27%7Cw%29p%28w%29) if w and w' have different probability, the two conditional probabilities also should be different. In other words, these two are not symmetric.

- how to deal with the exponential item?

  - see the below models

#### Negative sampling

This is a method of sampling. It is natural to use sampling when the object is exponentially big. One classic sampling method is MCMC.

- sampling: In the maximum likelihood estimation, each time for one context word, we need to update all the other words vectors, represented by the exponential items. This is very time-consuming. Instead, we only update a few words' vectors. These few words are selected by sampling. 

- negative: all the words not the one we expected are called negative words. 

  - how to know which word is our expected word? the word itself as the context must be a positive word.

- how to update the word vectors?

- what is the relationship between Negative sampling and PMI?

### Global vectors for word representation (GloVe)

  The key idea of GloVe is: 

  1. extract a count matrix from the given dataset. 

     - Each row is a word in the vocabulary
     - Each column is a context
     - Each entry n_ij is the appearance times of the word i in the context j

  2. construct another matrix from the vector representation of vocabularies and contexts.

     - each entry is the inner product of the vocabulary vector and the context vector.

  3. update vectors to make these two matrices as close as possible.

     (optional) using weight function to weight items with different counts.

  In this way, the vector representation problem is transferred as a matrix factorization problem.

  ![img](http://latex.codecogs.com/svg.latex?%5Cmin_%7BX%2CY%7D%5C%7CM-X%5ETY%5C%7C%5E2_F)

- how does GloVe solve the exponential problem?

  - it uses the **unnormalized** "probability" instead of the normalized ones to avoid the exponential item. Because the exponential item works for normalization. 

## Methods

### SGD (Stochastic gradient descent)

- The objective function: ![img](http://latex.codecogs.com/svg.latex?f%28x%29%3D%5CSigma_%7Bi%7Df_i%28x%29)

- Traditional gradient descent way:

  ![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Cpartial%20f%7D%7B%5Cpartial%20x%7D%3D%5CSigma_%7Bi%7D%5Cfrac%7B%5Cpartial%20f_i%7D%7B%5Cpartial%20x%7D)
  
- Stochastic way:

  ![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Cpartial%20f%7D%7B%5Cpartial%20x%7D%3D%5Cfrac%7B%5Cpartial%20f_%7B%5Cgamma%7D%7D%7B%5Cpartial%20x%7D,%5Cgamma%5Csim Uniform%281,n%29)

  Key Idea: 

  - derivative
  - only part of objective
  - stochastic each time

# Left Questions

- the relationship of Negative Sampling and PMI?