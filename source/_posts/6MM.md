---
title: Data Clustering
date: 2019-07-15
categories: Computational Intelligence Lab
---

# Data Clustering

## Vector Quantization

assign vectors to some groups, denoted by the centroids. Methods: K-means, Mixture Model.

## K-Means

- k: the number of clusters
- means: the way we get the centroids is by averaging.

### Objective

![img](http://latex.codecogs.com/svg.latex?%5Cmin_%7BU%2CZ%7D%20J%28U%2CZ%29%3D%5CSigma_%7Bi%3D1%7D%5EN%5CSigma_%7Bj%3D1%7D%5EK%20z_%7Bij%7D%5C%7Cx_i-u_j%5C%7C%5E2%0A%3D%20%5C%7CX-UZ%5ET%5C%7C_F%5E2)



Here, we use the **squared** Euclidean distance, not the Euclidean distance.

### Algorithms

Alternating minimization

- update **Z**, given **U**: the closest U
- update **U**, given **Z**: the average of Z.

### Practical considerations

- Quadratic convergence rate.
- computational cost: O(nkd) per iteration.



### Variant

#### K-Means++ (initialization)

Since initialize **U** and **Z** matter a lot to the computation cost and final result, this variant try to find a better way to initialize **U**. 

[wiki](https://en.wikipedia.org/wiki/K-means%2B%2B)

The intuition behind this approach is that **spreading out** the *k*  initial cluster centers is a good thing: 

1. the first cluster center is chosen uniformly at random from the data points that are being clustered, 
2. after which each subsequent cluster center is chosen from the remaining data points with probability proportional to its squared distance from the point's closest existing cluster center.

#### Core set (sub-training dataset)

The idea is our dataset is too big to run the K-Means algorithm. Therefore, we need to extract a sub-sample dataset from it. This method solves how to select the sub-training dataset.

- how big should the sub-dataset be?

  - m is dependent on ![img](http://latex.codecogs.com/svg.latex?%5Cepsilon)-approximation guarantees (with probability ![img](http://latex.codecogs.com/svg.latex?%5Cdelta) for

    ![img](http://latex.codecogs.com/svg.latex?m%5Cpropto%20%5Cfrac%7Bdk%5Clog%20k%2B%5Clog%201%2F%5Cdelta%7D%7B%5Cepsilon%5E2%7D)

- which nodes should be selected?

  - randomly selected, according to the probability, which combines the uniform selection and the distance of nodes to centroids.

     ![img](http://latex.codecogs.com/svg.latex?p_i%20%3D%20%5Cfrac%7B1%7D%7B2N%7D%2B%5Cfrac%7BD_i%5E2%7D%7B2%5CSigma_%7Bj%3D1%7D%5END_j%5E2%7D)

  - why to weight the data points?[the original paper](https://las.inf.ethz.ch/files/bachem18scalable.pdf)

    - the weight is to guarantee the expectation of the quantization error on the sample dataset doesn't change.

      - the original vector quantization error is

       ![img](http://latex.codecogs.com/svg.latex?%5Cphi_%7B%5Cchi%7D%28Q%29%3D%5CSigma_%7Bx%5Cin%5Cchi%7Dd%28x%2CQ%29%5E2%3D%5CSigma_%7Bx%5Cin%5Cchi%7Dq%28x%29%5Cfrac%7Bd%28x%2CQ%29%5E2%7D%7Bq%28x%29%7D)

      The quantization error can hence be approximated by sampling m points from ![img](http://latex.codecogs.com/svg.latex?%5Cchi) using ![img](http://latex.codecogs.com/svg.latex?q%28x%29)and assigning them weights inversely proportional to ![img](http://latex.codecogs.com/svg.latex?q%28x%29).  Back to the original dataset, we can assign each data point with the selection probability ![img](http://latex.codecogs.com/svg.latex?%5cfrac%7B1%7D%7BN%7D), and weight 1, we get ![img](http://latex.codecogs.com/svg.latex?E%28%5Cphi_%7B%5Cchi%7D%28Q%29%29%3D%5cfrac%7B1%7D%7BN%7DE%28%5CSigma_%7Bx%5Cin%5Cchi%7Dd%28x%2CQ%29%5E2%29)

      - the error on the new dataset ![img](http://latex.codecogs.com/svg.latex?%5Czeta) is

        ![img](http://latex.codecogs.com/svg.latex?%5Cphi_%7B%5Czeta%7D%28Q%29%3D%5CSigma_%7Bx%5Cin%5Czeta%7Dp%28x%29w%28x%29d%28x%2CQ%29%5E2), 

        Set ![img](http://latex.codecogs.com/svg.latex?w%28x%29%3D%5Cfrac%7B1%7D%7Bmp%28x%29%7D)

# Mixture Model

## Idea

Probabilistic clustering. For each data point, it is not absolutely assigned to one cluster but with the probability belonging to one cluster.



## Gaussian Mixture Model

- mixture: several models mixed together
- gaussian: each model follows the multinomial Gaussian distribution.
- the categorical distribution of ![img](http://latex.codecogs.com/svg.latex?j) is given ahead and the same for all ![img](http://latex.codecogs.com/svg.latex?x)?? Or it is related to ![img](http://latex.codecogs.com/svg.latex?x_i), which means should be written as ![img](http://latex.codecogs.com/svg.latex?%5cpi_%7Bij%7D)?How should we know ![img](http://latex.codecogs.com/svg.latex?%5cpi)?
  - Yes. Here ![img](http://latex.codecogs.com/svg.latex?%5cpi_%7Bj%7D) means ![img](http://latex.codecogs.com/svg.latex?p%28z_i%3D1%29), this is known from our dataset as a whole, not meaning ![img](http://latex.codecogs.com/svg.latex?p%28z_%7Bij%7D%3D1%7Cx_i%29)
  - So, it only applies when we already know the categories for all ![img](http://latex.codecogs.com/svg.latex?x)

![img](http://latex.codecogs.com/svg.latex?p%28x%3B%5Ctheta%29%3D%5CSigma_%7Bj%3D1%7D%5EK%5Cpi_j%20p%28x%3B%5Cmu_j%2C%5CSigma_j%29)

​	 we can understand in this way: The model is composed of different models. For a data point ![img](http://latex.codecogs.com/svg.latex?x), its probability of generated by this "big" model, is decided by its probability of generated by each "small" model, and the proportion of the "small" model. For example, I know ![img](http://latex.codecogs.com/svg.latex?x) is generated by the ![img](http://latex.codecogs.com/svg.latex?j)-th model with probability 1 and 0 to other models, and ![img](http://latex.codecogs.com/svg.latex?j)-th model totally generates ![img](http://latex.codecogs.com/svg.latex?%5cpi_j%5ccdot N) number of points, (![img](http://latex.codecogs.com/svg.latex?N) is the size of the dataset), then the probability of  ![img](http://latex.codecogs.com/svg.latex?x) is generated by the whole model is ![img](http://latex.codecogs.com/svg.latex?1%5ccdot %5cpi_j)

### ML method

Maximum likelihood method usually 

- makes some general assumption about the distribution. Here: the Gaussian distribution.
- then try to obtain("infer") the specifics from the data available.

### Generation- EM algorithms

By **MLE**, we want to get the parameters of ![img](http://latex.codecogs.com/svg.latex?%5ctheta). Since the objective function has a summation within a log calculation, we should use E-M algorithm and use Jensen's inequality.

- what is the role of ![img](http://latex.codecogs.com/svg.latex?q_j)?
  - ![img](http://latex.codecogs.com/svg.latex?q_j) should exactly be ![img](http://latex.codecogs.com/svg.latex?q_%7Bij%7D), it is related to ![img](http://latex.codecogs.com/svg.latex?x_i). It means ![img](http://latex.codecogs.com/svg.latex?p%28z_%7Bij%7D%3D1%7Cx_i%29)

- why  we also estimate ![img](http://latex.codecogs.com/svg.latex?%5cpi_j)? I think it is given ahead, and has no effect on the solution of ![img](http://latex.codecogs.com/svg.latex?%5ctheta)
  - Yes, it has no **direct** effect on ![img](http://latex.codecogs.com/svg.latex?%5ctheta), but has a direct influence on ![img](http://latex.codecogs.com/svg.latex?qz_%7Bij%7D), thus indirectly influences ![img](http://latex.codecogs.com/svg.latex?%5ctheta). Therefore, in each step, we also need to optimize ![img](http://latex.codecogs.com/svg.latex?%5cpi_j)

### Inference

It is easy to get the inference of latent variable ![img](http://latex.codecogs.com/svg.latex?j) as ![img](http://latex.codecogs.com/svg.latex?q_%7Bij%7D)



# Model Selection

## Data fit

## Complexity

can be measured by the number of free parameters.

### AIC

![img](http://latex.codecogs.com/svg.latex?AIC%28%5Ctheta%7CX%29%3D-%5Clog%20p%28X%3B%5Ctheta%29%2B%5Ckappa%28%5Ctheta%29)

### BIC

![img](http://latex.codecogs.com/svg.latex?BIC%28%5Ctheta%7CX%29%3D-%5Clog%20p%28X%3B%5Ctheta%29%2B%5Cfrac%7B1%7D%7B2%7D%5Ckappa%28%5Ctheta%29%5Clog%20N)

BIC penalizes complexity more than AIC criterion

A single AIC (BIC) result is meaningless. One has to repeat the analysis for different Ks and compare the differences: the most suitable number of clusters corresponds to the smallest AIC (BIC) value.