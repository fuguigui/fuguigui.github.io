---
title: Neural Network
date: 2019-07-17
tags: [Machine Learning]
categories: [Learning Notes]
mathjax: true
---

# Composition

- layer
  - neurons/units
    - input
    - weight
    - activation function
    - output $x^{(l)}=\sigma^{(l)}(W^{(l)}x^{(l-1)})$​

- network depth: the feature hierarchy
- layer width: the number of features

## Activation function

### Sigmoid

change the linearity to non-linearity

### ReLU

change the linearity in some way

simple derivative

# Training

## Loss function

Define the training objective, can be chosen by the output type. $y^*$  is the target output, $y$ is the predicted output

- $y^*$​​ is categorical: **cross-entropy loss**: 
  $$
  l(y^*,y)=-y^* \log y - (1-y^*)\log(1-y)
  $$
  

- $y^*$ is numerical: **squared loss**: 
  $$
  l(y^*,y)=\frac{1}{2}(y^*-y)^2
  $$

### Regularization

to penalize the parameters

- L2 regularization: $L_{\lambda}(X;\theta)=L(X;\theta)+\frac{\lambda}{2}\|\theta\|_2^2$

## Backpropagation

### SGD

different from past SGD, here we also include a step size  $\eta$, because the steepest / original descent is too expensive for large data sets. 

In the past, it should be $\theta\leftarrow (1-\lambda)\theta - \nabla_\theta l(y_t^*, y(x_t, \theta))$

But with $\eta$​​, it becomes $\theta\leftarrow (1-\eta\lambda)\theta - \eta\nabla_\theta l(y_t^*, y(x_t, \theta))$​​

### Chain rule

$$
\frac{\partial x^{(l)}}{\partial x^{(l-n)}} = J^{(l)}\cdot  J^{(l-1)}\cdots  J^{(l-n+1)}\\
\nabla_{x^{(l)}}^T l = \nabla_y^T l\cdot J^{(L)}\cdots J^{(l+1)}
$$

### Weights influence

$$
\frac{\partial x_i^{(l)}}{\partial w_{ij}^{(l)}} = \sigma'([w_i^l]^Tx^{(l-1)})x_j^{(l-1)}
$$

is composed of two parts:

- sensitivity
- activation

# Comparison to Logistics Regression

Logistic Regression:

- linear

MLP: multi-layer perceptron:

- learn intermediate feature representation

- include non-linearity

  

# Convolutional Neural Network

## Receptive field

The creative point of convolutional neural network is how it chooses and organizes the input $x^{(l)}$

This variant in some way complicates the neural network

## Weight sharing

This simplifies the neural network. Neurons share the same weights.

Weights define a **filter mask**. A filter mask corresponds to a vector of $y$, in CNN, which is called channel.

## Building blocks

- convolutional layer
- pooling layer
- fully-connected layer

### Convolutional layer

[from Medium](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53) The objective of the Convolution Operation is to **extract the high-level features** such as edges, from the input image. ConvNets need not be limited to only one Convolutional Layer. Conventionally,

- the first ConvLayer is  responsible for capturing the Low-Level features such as edges, color,  gradient orientation, etc. 
- With added layers, the architecture adapts to the High-Level features as well, giving us a network which has the  wholesome understanding of images in the dataset, similar to how we would.

#### Formula

$$
F_{n,m}(x; w)=\sigma(b+\sum_{k=-2}^2\sum_{l=-2}^2w_{k,l}\cdot x_{n+k, m+l})
$$

### Pooling layer

 the Pooling layer is responsible 

- for reducing the spatial size of the Convolved Feature
- to **decrease the computational power required to process the data** through dimensionality reduction.
- useful for **extracting dominant features** which are rotational and positional invariant, thus maintaining the process of effectively training of the model.

#### Methods

- max pooling
- average pooling

### Fully-connected layer

Fully-Connected layer is a (usually) cheap way 

- of learning **non-linear** combinations of the high-level features as represented by the output of the convolutional layer. 
- it combines all the output from the previous layer. Different from the convolutional layer, which only uses part of the output from the previous layer.

# Variants

## Deeper Network

the number of layers grows from 10+ to 100+.

not only use the output from the previous layer but also the input of the previous layer

## Semantic Segmentation

add de-convolutional layers.