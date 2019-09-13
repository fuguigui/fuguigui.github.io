---
title: Neural Network
date: 2019-07-17
tags: [courses, machine learning, data analysis]
categories: Computational Intelligence
---

# Neural Network

## Composition

- layer
  - neurons/units
    - input
    - weight
    - activation function
    - output

![img](http://latex.codecogs.com/svg.latex?x%5E%7B%28l%29%7D%3D%5Csigma%5E%7B%28l%29%7D%28W%5E%7B%28l%29%7Dx%5E%7B%28l-1%29%7D%29)

- network depth: the feature hierarchy
- layer width: the number of features



### Activation function

#### Sigmoid

change the linearity to non-linearity

#### ReLU

change the linearity in some way

simple derivative

## Training

### Loss function

Define the training objective, can be chosen by the output type. ![img](http://latex.codecogs.com/svg.latex?y%2A) is the target output, ![img](http://latex.codecogs.com/svg.latex?y) is the predicted output

- ![img](http://latex.codecogs.com/svg.latex?y%2A) is categorical: **cross-entropy loss**: 

  ![img](http://latex.codecogs.com/svg.latex?l%28y%2A%2Cy%29%3D-y%2A%5Clog%20y%20-%20%281-y%2A%29%5Clog%281-y%29)

- ![img](http://latex.codecogs.com/svg.latex?y%2A) is numerical: **squared loss**: ![img](http://latex.codecogs.com/svg.latex?l%28y%2A%2Cy%29%3D%5Cfrac%7B1%7D%7B2%7D%28y%2A-y%29%5E2)

#### Regularization

to penalize the parameters

- L2 regularization:![img](http://latex.codecogs.com/svg.latex?L_%7B%5Clambda%7D%28X%3B%5Ctheta%29%20%3D%20L%28X%3B%5Ctheta%29%2B%5Cfrac%7B%5Clambda%7D%7B2%7D%5C%7C%5Ctheta%5C%7C_2%5E2)

### Backpropagation

#### SGD

different from past SGD, here we also include a step size  ![img](http://latex.codecogs.com/svg.latex?%5ceta), because the steepest (original) descent is too expensive for large data sets. 

In the past, it should be

![img](http://latex.codecogs.com/svg.latex?%5Ctheta%5Cleftarrow%20%281-%5Clambda%29%5Ctheta-%5Cnabla_%7B%5Ctheta%7Dl%28y_t%5E%2A%2Cy%28x_t%2C%5Ctheta%29%29)

But with ![img](http://latex.codecogs.com/svg.latex?%5ceta), it becomes

![img](http://latex.codecogs.com/svg.latex?%5Ctheta%5Cleftarrow%20%281-%5ceta%5Clambda%29%5Ctheta-%5ceta%5Cnabla_%7B%5Ctheta%7Dl%28y_t%5E%2A%2Cy%28x_t%2C%5Ctheta%29%29)

#### Chain rule

![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Cpartial%20x%5E%7B%28l%29%7D%7D%7B%5Cpartial%20x%5E%7B%28l-n%29%7D%7D%20%3D%20J%5E%7B%28l%29%7D%5Ccdot%20J%5E%7B%28l-1%29%7D%5Ccdots%20J%5E%7B%28l-n%2B1%29%7D)

![img](http://latex.codecogs.com/svg.latex?%5Cnabla%5ET_%7Bx%5E%7B%28l%29%7D%7Dl%3D%5Cnabla%5ET_y%20l%5Ccdot%20J%5E%7B%28L%29%7D%5Ccdots J%5E%7B%28l%2B1%29%7D)

#### Weights influence

![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Cpartial%20x_i%5E%7B%28l%29%7D%7D%7B%5Cpartial%20w_%7Bij%7D%5E%7B%28l%29%7D%7D%20%3D%20%5Csigma%27%28%5Bw_i%5E%7Bl%7D%5D%5ETx%5E%7B%28l-1%29%7D%29x_j%5E%7B%28l-1%29%7D)

is composed of two parts:

- sensitivity
- activation

## Comparison to Logistics Regression

Logistic Regression:

- linear

MLP (multi-layer perceptron):

- learn intermediate feature representation

- include non-linearity

  

# Convolutional Neural Network

## Receptive field

The creative point of convolutional neural network is how it chooses and organizes the input  ![img](http://latex.codecogs.com/svg.latex?x%5E%7B%28l%29%7D)

This variant in some way complicates the neural network

## Weight sharing

This simplifies the neural network. Neurons share the same weights.

Weights define a **filter mask**. A filter mask corresponds to a vector of ![img](http://latex.codecogs.com/svg.latex?y), in CNN, which is called channel.

## Building blocks

- convolutional layer
- pooling layer
- fully-connected layer

### Convolutional layer

[from Medium](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53) The objective of the Convolution Operation is to **extract the high-level features** such as edges, from the input image. ConvNets need not be limited to only one Convolutional Layer. Conventionally,

- the first ConvLayer is  responsible for capturing the Low-Level features such as edges, color,  gradient orientation, etc. 
- With added layers, the architecture adapts to the High-Level features as well, giving us a network which has the  wholesome understanding of images in the dataset, similar to how we would.

#### Formula

![img](http://latex.codecogs.com/svg.latex?F_%7Bn%2Cm%7D%28x%3Bw%29%3D%5Csigma%28b%2B%5CSigma_%7Bk%3D-2%7D%5E2%5CSIgma_%7Bl%3D-2%7D%5E2w_%7Bk%2Cl%7D%5Ccdot%20x_%7Bn%2Bk%2Cm%2Bl%7D%5C%29)

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