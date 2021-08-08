---
title: Weekly summary
date: 2020-10-18
tags: [Thesis, Life]
categories: [Diary]
---

本周总结：

初步了解了higher-order graph的一些模型，重点了解用simplicial complex进行建模的理论基础和实际案例。

主要入手的几篇论文：

- simplicial neural network：这篇文章在simplicial complex上定义了laplacian，进而根据卷积定理定义了convolution操作，融入到现有的神经网络框架中。
- hodgenet：这篇文章关注于edge层面的hodge laplacian，把hodge laplacian融入到现有的网络框架中，解决edge相关的一些问题。 
- random walk ... normalized hodge laplacian：使用Hodge laplacian在边上定义random walk，来解决与边相关的一系列问题。

十分推荐在理解simplicial complex的理论性质时，参考论文：HODGE LAPLACIANS ON GRAPHS。这篇论文的第二三四章主要讲解hodge的一些理论基础。由浅入深，其中第二章很容易理解，其他两章在第二章的基础上也可以理解。第五章讲证明，可以跳过。



此外，推荐一篇hyper场景下的综述文章：Networks beyond pairwise interactions: structure and dynamics。可以用作闲着的时候的读物。



个人感受，simplicial complex模型的局限性比较大，对item的要求严格，而且理论性质多应用于同度（k）的边之间，不适用于higher-order不一致的情况，而我认为后者可能才是主流。

解决的想法：使用hypergraph这个模型。对于higher-order的理论研究非常多，能不能遵循graph中的模型发展路径，将其类比迁移到hypergraph上？

下周的计划：

Graph Paper：

- normal graph, Goal: have a general idea of GNNs, implement all the following GNNs using DGL
  - [x] How Powerful are Graph Neural Networks? (10.23)
  - [x] GCN
  - [x] ChebyNet (10.20)
  
  **Unfinished**
  
  - [ ] MoNet (10.21)
  - [ ] GraphSage (10.22)
  - [ ] GAT (10.22)
- higher-order graph
  - Datasets: How to get the datasets we want? How to design tasks? See other papers.
    - [x] [Amazon dataset](https://snap.stanford.edu/data/com-Amazon.html) to see how to get the triangle values? (10.20)
    - [x] Other papers tasks. (10.20, 10.21)
  - [x] :star2: A. R. Benson, D. F. Gleich, and J. Leskovec, Higher-order organization of complex networks, Science, 353 (2016), pp. 163–166. (10.20):star2:
  - [x] Learning with hypergraphs: Clustering, classification, and embedding. Dengyong Zhou, Jiayuan Huang, and Bernhard Scho ̈lkopf.  NIPS2007. 
  - [x] :star2: Random walks on hypergraphs. Timoteo Carletti, Federico Battiston, Giulia Cencetti, and Duccio Fanelli. Phys. Rev. E, 101(2):022308, 2020. (10.21)
  
  **Unfinished**
  
  - [ ] :star2: Simultaneous group and individual centralities. **Phillip Bonacich**. Soc. Netw., 13(2):155–168, 1991.
  - [ ] Weisfeiler and Leman Go Neural: Higher-order Graph Neural Networks
  - [ ] :star: Random walks and diffusion on networks, Physics Reports, N. Masuda, M. A. Porter, and R. Lambiotte, 2017.
  
  - [ ] :star2:High-ordered random walks and generalized Laplacians on hypergraphs.  Linyuan Lu and Xing Peng. In International Workshop on Algorithms and Models for the Web-Graph, pages 14–25. Springer, 2011.
  - [ ] http://www.geometry.caltech.edu/pubs/dGDT16.pdf
  - [ ] Discrete Connection and Covariant Derivative for Vector Field Analysis and Design (10.21)

Optimization:

- [x] September all lecture video and notes 
  - [x] 10.20: 9.24
  - [x] 10.21: 9.28
  - [x] 10.22: 10.1

Video:

- [x] Game of thrones: Season 6 
  - [x] Episode 5
- [x] Deutsch lehrnen: 2-4

**Unfinished**

Misc

- [ ] https://archwalker.github.io/blog/2019/11/10/GNN-Go-Through-Main-Models.html

Video:

- [ ] Game of thrones: Season 6 
  - [x] Episode 5
  - [ ] Episode 6
  - [ ] Episode 7

Game:

- [ ] 隐形的守护者