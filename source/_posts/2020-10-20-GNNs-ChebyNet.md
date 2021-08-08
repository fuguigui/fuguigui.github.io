---
title: ChebyNet
date: 2020-10-20
categories: [Learning Notes]
tags: [Graph, Deep Learning]
mathjax: true
---



# ChebyNet

[arXiV](https://arxiv.org/pdf/1606.09375.pdf)

A saying is that: using Chebyshev polynomials to calculate will save a large amount of computation.

In the paper, there is a detailed explanation.

The goal is to compute
$$
y = U g_{\theta}(\Lambda)U^{\top}x\\
g_{\theta}(\Lambda) = \sum_{k=0}^{K-1}\theta_k\Lambda^k
$$
The parameters' dimensions are
$$
\Lambda = diag([\lambda_0,\cdots,\lambda_{n-1}])\in\mathbb{R}^{n\times n}\\
U\in\mathbb{R}^{n\times n}
$$

- $Ug_{\theta}(\Lambda)U^\top$ computation cost: $O(n^2)$, because of the multiplication with the Fourier basis $U$.

- So, A solution to this problem is to parametrize $g_{\theta}(L)$ as a polynomial function that can be computed recursively from $L$, as $K$ multiplications by a sparse $L$ costs $\mathcal{O}(K|E|) \ll \mathcal{O}(n^2)$.

  - one choice: Chebyshev expansion: $T_k(x)=2xT_{k-1}(x)-T_{k-2}(x)$

  - another choice: Krylov subspace: $\mathcal{K}_K(L,x)=span\{x, Lx,\cdots, L^{K-1}x\}$. (I agree! why not use this?)

    The author says this: (it) seems attractive because of the coefficients’ independence. It is however *more convoluted* and thus left as a future work.

So far, I know there is some disadvantage of that choice, but still not very clear about this disadvantage.



- [ ] In this paper [Wavelets on Graphs via Spectral Graph Theory](https://arxiv.org/pdf/0912.3848.pdf), maybe I can find a detailed explanation of the advantage of Chebyshev polynomial application. I am not very willing to read this now.
- [ ] In this paper [breakdowns and stagnation in iterative methods](file:///Users/fuguirong/Downloads/Leyk1997_Article_BreakdownsAndStagnationInItera.pdf), I may find an analysis of Krylov subspace.