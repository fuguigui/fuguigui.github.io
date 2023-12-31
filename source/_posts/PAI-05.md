---
title: PAI5 Probabilistic Planning
date: 2020-01-28
tags: [AI, Reinforcement Learning, Markov]
categories: [Learning Notes]
mathjax: true
---

How do we use inference we keep tracking over time in order to act? 

# Markov Decision Process

An essential model studied in the context of decision making under uncertainty. The most basic model in making inference is Markov chain. MDP is the most basic model in making decisions, which is controlled Markov chain.

An MDP is specified by:

- a set of states $X=\{1, 2, \cdots, n\}$
- a set of actions $A=\{1,2,\cdots,m\}$
- Transition probabilities: the next state not only depends on the current state, but also on the taken actions. $P(x'|x,a)=\Pr(\text{Next state} = x'|\text{Action } a \text{ in state } x)$​
- A reward function $r(x,a)$: objective: state and action pair. 
  - where does the reward come from? An engineering choice. Many works on how to define a good reward function. For instance, reverse reinforcement learning, where I have an ideal series of actions, I want to define a kind of reward to make this series come true.
  - This reward function could be deterministic or random. If it is random, we look its expected value.

The problem is how to choose actions to maximize rewards. 

## Planning in MDPs

Currently, assuming that $r$ reward function and $P$ transition are known.

**Policy** is a function from states to actions. It could be deterministic or stochastic. 

How to compare policies? According to the reward function. Several strategies to use the reward function

- sum up the rewards and fix a certain number of future steps. 
  - if not fixing a certain step number, the summation could diverge, cannot be compared.
  - This is a generalization of naive greedy. Question: what is the certain number? why others?
  
- count more on the current step than those in future. Discount calculation. How to make discounts?
  $$
  \mathbb{E}[r(X_0,\pi(X_0))+\gamma r(X_1,\pi(X_1))+\gamma^2r(X_2,\pi(X_2))+\cdots]
  $$
  

Assuming that, we have a strategy to score policy, then, what is the optimal policy?

### Value function

Given the starting state x, try to evaluate a policy from a long term view:

$$
V^{\pi}(x)=J(\pi|X_0=x)=\mathbb{E}[\sum_{t=0}^{\infty}\gamma^t r(X_t,\pi(X_t))|X_0=x]
$$
This is a function on the state. Each state is mapped to one value.

It has **recursive relation**. What is recursion? 

$$
V^{\pi}(x)=r(x,\pi(x))+\gamma\sum_{x'}\Pr[x'|x,\pi(x)]V^{\pi}(x')
$$

#### Solution

How to solve this function?

Supposing there are finite states with n states. Then, this is a group of n linear functions with n unknown variables, which have a unique solution when $\gamma<1$. 

- for any given start state, we can have the value of the policy
- if the start state is random, we can take the expectation of the policy's value.

### Policy finding

So far, we know given a policy how to evaluate it. Then, how to find the best policy?

- A simple algorithm

  1. for every policy pi compute $J(\pi)=\sum_x\Pr[X_0=x]V^{\pi}(x)$  
  2. pick the policy with the maximum expected value.

  The complexity is linear to the number of polices, which is the number of actions exponential to the number of states $O(m^n)$

- A more efficient algorithm. Consider a new setting, we have been told the value of each state, which action to take? An intuition is greedy maximizing the trade-off between the immediate action and the future states. In this way, we achieve a "best" policy. Best under the meaning of our maximize standard.

   $$
  a^*\in\arg_a\max r(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V(x')
  $$
  
  
  - the **greedy police** is with respect to the value of state.

**Theorem** Bellman: A policy is optimal $\Leftrightarrow$ The greedy policy w.r.t its induced value function is itself.

How to use this theorem to find optimal policy?

#### Policy iteration

This is an intuitive way to perform the cycle in the theorem

1. Start with an arbitrary, e.g. random policy $\pi$
2. until converge, do:
   1. compute value function $V^{\pi}(x)$
   2. compute greedy policy  $\pi_G  w.r.tV^{\pi}$
   3. set $ \pi\gets\pi_G$

Remark: it only converges to values, not actions. Because there could be several policies with an optimal value.

Guarantee:

- monotonically improve
- converge to an exact optimal policy 
  - in polynomial iterations $O(n^2m/(1-\gamma))$
  - in every iteration, the complexity is $  O(n^2\cdot (n+m))$
    - needs to solve a linear system in the number of states $n^2$ for each state n, which is expensive  $O(n^2\cdot n)$ 
    - Also, for each state $n$, for each action $m$, needs to calculate the value $n$, which is $O(n^2\cdot m)$​
    - this complexity can be reduced if a state just reaches some of other states, called sparse MDPS



#### Value iteration

An alternative approach is to find the value of optimal policy first and then use the greedy policy. For the optimal policy $\pi\ast i_t$ holds

$$
V^*(x)=\max_a r(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V^*(x')
$$


we can compute $\pi\ast$ using dynamic programming:

define $V_t(x)$: the maximum expected reward when starting in state $x$ and world ends in $t$ time steps

- $V_0(x)=\max_ar(x,a)$

- $V_1(x)=\max_ar(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V_0(x')$

- $V_{t+1}(x)=\max_ar(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V_t(x')$

Iterate until convergence.

Algorithm:

1. initialize $V_0(x)=\max_ar(x,a)$
2. For $t=1, \cdots, \infty$:
   1. For each $x, a$, let $Q_t(x,a)=r(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V_{t-1}(x')$
   2. For each $x$ let $V_t(x)=\max_a Q_t(x,a)$
   3. Break if $ ||V_t-V_{t-1}||_{\infty}=\max_x|V_t(x)-V_{t-1}(x)|\leq\epsilon$

Guarantee:

- converge to $\epsilon$​-optimal policy. The proof is Bellman update, using $V_t$ to update $V_{t+1}$,  is a contraction.
  $$
  B:\mathbb{R}^n\to\mathbb{R}^n, B:V\to BV. (BV)(x)=\max_ar(x,a)+\gamma\sum_{x'}\Pr[x'|x,a]V(x')
  $$
  There is a theorem: 
  $$
  \forall V,V'\in\mathbb{R}^n, ||BV-BV'||_{\infty}\leq\gamma||V-V'||_{\infty}
  $$
  A contraction has two important properties:

  - existence of a unique fixed point
  - convergence to the fixed point

  Convergence rate:

  - number of iterations: depends a lot on initial  policy and epsilon.
  - in each iteration:  the complexity is $O(n^2\cdot m)$

- **no** monotonical property.

#### Summary

In practice, which one is better depends on the application. Can combine both of them. Like take one greedy policy in some step of policy iteration as the initial policy for value iteration.



# POMDP

Partially observed markov decision process = controlled HMM = Belief-state MDP

We have a series of observations $Y_1,\cdots,Y_t$. The key idea is the last belief $X_t$, including all information from the previous observations. In other words, if we have $X_t$, we can forget all of $Y_1,\cdots,Y_t$. Instead of solving markov decision process over the state space, we are going to solve MDP over the belief of state space. 

Key idea: interpret POMDP as an MDP with enlarged state space: new states correspond to beliefs $P(X_t|y_{1:t})$ in the original POMDP.

Once we have the newest observation $Y_{t+1}$, applying Bayesian filtering, a deterministic function, to update on the observations

## Settings

- the set of state space: $X_1,X_2,\cdots,X_n$
- the set of action space: $A_1,A_2,\cdots A_m$
- the set of observation space: $Y_1,Y_2,\cdots Y_k$
- Transition probability: $P(X_{t+1}|X_t,A_t)$
- observation probability: $P(Y_t|X_t)$

Consider at time $t$, 

- we have the belief $B_t=b_t$​, where $b_t$​ is an instance of $n$-dimension probability vector.
- Suppose we take the action at 
- and observe $y_{t+1}$.

In this way,

$$
b_{t+1}(x)=\Pr[X_{t+1}=x|y_{1:t+1},a_{1:t}]
$$

$$
=\frac{1}{Z}\sum_{x'}\Pr[X_t=x'|y_{1:t},a_{1:t-1}]\Pr[X_{t+1}=x|X_t=x',a_t]\Pr[Y_{t+1}=y_{t+1}|x]
$$

That means $  b_{t+1}=f(b_t,a_t,y_{t+1})$

## Transformation

Transform into a new MDP problem:

- states: beliefs over states for original POMDP: $\mathcal{B}=\{b:\{1,2,\cdots,n\}\to[0,1],\sum_xb(x)=1\}$

- actions: same as original MDP

- Transition model: 

  - stochastic observations: the observation of the next time only depends on our belief of the current time and the action we take. $\Pr[Y_{t+1}=y|b_t,a_t]=\sum_x b_t(x)\Pr[Y_{t+1}=y|X_t=x,a_t]$
  - state update Bayesian filtering: Given $b_t$, $a_t$, $y_{t+1}$: $b_{t+1}(x')=\frac{1}{Z}\sum_x b_t(x)\Pr[X_{t+1}=x'|X_t=x,a_t]\Pr[y_{t+1}|x']$​

- Reward function: $r(b_t,a_t)=\sum_x b_t(x)r(x,a_t)$

  If we can solve the new MDP, we can solve the original MDP.

## Approximate solutions

The belief state space could be exponential. We need some approximate solutions.

The key idea: most belief states never reached, we want to limit the belief space, approaches like:

- discretize the belief space by sampling
- dimension reduction

### Policy gradient method

Limit the policy space to some parametric family. Then transform the problem into an optimization problem.



## Planning

- use the inference model to do planning
- condition not only on the past, but also on the future, find the action to lead me to that future. 