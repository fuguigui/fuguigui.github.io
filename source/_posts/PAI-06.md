---
title: Probabilistic Planning
date: 2020-01-28
tags: [probabilistic artificial intelligence, reinforcement learning]
categories: course notes
---

How do we use inference we keep tracking over time in order to act? 

# Markov Decision Process

An essential model studied in the context of decision making under uncertainty. The most basic model in making inference is Markov chain. MDP is the most basic model in making decisions, which is controlled Markov chain.

An MDP is specified by:

- a set of states X={1, 2, ..., n}
- a set of actions A={1,2,...,m}
- Transition probabilities: the next state not only depends on the current state, but also on the taken actions. P(x'|x,a)=Prob(Next state = x'|Action a in state x)
- A reward function r(x,a): objective: state and action pair. 
  - where does the reward come from? An engineering choice. Many works on how to define a good reward function. For instance, reverse reinforcement learning, where I have an ideal series of actions, I want to define a kind of reward to make this series come true.
  - This reward function could be deterministic or random. If it is random, we look its expected value.

The problem is how to choose actions to maximize rewards. 

## Planning in MDPs

Currently, assuming that r (reward function) and P(transition) are known.



**Policy** is a function from states to actions. It could be deterministic or stochastic. 

How to compare policies? According to the reward function. Several strategies to use the reward function

- sum up the rewards and fix a certain number of future steps. 
  - if not fixing a certain step number, the summation could diverge, cannot be compared.
  - This is a generalization of naive greedy. Question: what is the certain number? why others?
- count more on the current step than those in future. Discount calculation. How to make discounts?
  - ![value](http://latex.codecogs.com/svg.latex?%5Cmathbb%7BE%7D%5Br%28X_0%2C%5Cpi%28X_0%29%29%2B%5Cgamma%20r%28X_1%2C%5Cpi%28X_1%29%29%2B%5Cgamma%5E2r%28X_2%2C%5Cpi%28X_2%29%29%2B%5Ccdots%5D)

Assuming that, we have a strategy to score policy, then, what is the optimal policy?

### Value function

Given the starting state x, try to evaluate a policy from a long term view:

![valuefunc](http://latex.codecogs.com/svg.latex?V%5E%7B%5Cpi%7D%28x%29%3DJ%28%5Cpi%7CX_0%3Dx%29%3D%5Cmathbb%7BE%7D%5B%5Csum_%7Bt%3D0%7D%5E%7B%5Cinfty%7D%5Cgamma%5Et%20r%28X_t%2C%5Cpi%28X_t%29%29%7CX_0%3Dx%5D)

This is a function on the state. Each state is mapped to one value.

It has **recursive relation**. What is recursion? 

![recurs](http://latex.codecogs.com/svg.latex?V%5E%7B%5Cpi%7D%28x%29%3Dr%28x%2C%5Cpi%28x%29%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2C%5Cpi%28x%29%5DV%5E%7B%5Cpi%7D%28x%27%29)

#### Solution

How to solve this function?

Supposing there are finite states with n states. Then, this is a group of n linear functions with n unknown variables, which have a unique solution when gamma<1. 

- for any given start state, we can have the value of the policy
- if the start state is random, we can take the expectation of the policy's value.

### Policy finding

So far, we know given a policy how to evaluate it. Then, how to find the best policy?

- A simple algorithm

  1. for every policy pi compute ![policyfunc](http://latex.codecogs.com/svg.latex?J%28%5Cpi%29%3D%5Csum_x%5CPr%5BX_0%3Dx%5DV%5E%7B%5Cpi%7D%28x%29)
  2. pick the policy with the maximum expected value.

  The complexity is linear to the number of polices, which is the number of actions exponential to the number of states. ![complexity](http://latex.codecogs.com/svg.latex?O%28m%5En%29)

- A more efficient algorithm. Consider a new setting, we have been told the value of each state, which action to take? An intuition is greedy maximizing the trade-off between the immediate action and the future states. In this way, we achieve a "best" policy. (best under the meaning of our maximize standard)

   ![best-action](http://latex.codecogs.com/svg.latex?a%5E%2A%5Cin%5Carg_a%5Cmax%20r%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV%28x%27%29)

  - the **greedy police** is with respect to the value of state.

**Theorem** (Bellman): A policy is optimal <=> The greedy policy w.r.t its induced value function is itself.

How to use this theorem to find optimal policy?

#### Policy iteration

This is an intuitive way to perform the cycle in the theorem

1. Start with an arbitrary (e.g. random) policy pi
2. until converge, do:
   1. compute value function ![valuefunc](http://latex.codecogs.com/svg.latex?V%5E%7B%5Cpi%7D%28x%29)
   2. compute greedy policy ![valuefunc](http://latex.codecogs.com/svg.latex?%5Cpi_G%20%20w.r.tV%5E%7B%5Cpi%7D)
   3. set ![valuefunc](http://latex.codecogs.com/svg.latex?%5Cpi%5Cgets%5Cpi_G)

Remark: it only converges to values, not actions. Because there could be several policies with an optimal value.

Guarantee:

- monotonically improve
- converge to an exact optimal policy 
  - in polynomial iterations ![complexity](http://latex.codecogs.com/svg.latex?O%28n%5E2m%2F%281-%5Cgamma%29%29)
  - in every iteration, the complexity is ![complexity](http://latex.codecogs.com/svg.latex?O%28n%5E2%5Ccdot%20%28n+m%29%29)
    - needs to solve a linear system in the number of states (n^2) for each state (n), which is expensive ![complexity](http://latex.codecogs.com/svg.latex?O%28n%5E2%5Ccdot%20n%29)
    - Also, for each state (n), for each action (m), needs to calculate the value (n), which is ![complexity](http://latex.codecogs.com/svg.latex?O%28n%5E2%5Ccdot%20m%29)
    - this complexity can be reduced if a state just reaches some of other states. (called sparse MDPS)



#### Value iteration

An alternative approach is to find the value of optimal policy first and then use the greedy policy. For the optimal policy pi* it holds

![optpolicy](http://latex.codecogs.com/svg.latex?V%5E%2A%28x%29%3D%5Cmax_a%20r%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV%5E%2A%28x%27%29)

we can compute pi* using dynamic programming:

define Vt(x): the maximum expected reward when starting in state x and world ends in t time steps

- ![policy0](http://latex.codecogs.com/svg.latex?V_0%28x%29%3D%5Cmax_ar%28x%2Ca%29)

- ![policy1](http://latex.codecogs.com/svg.latex?V_1%28x%29%3D%5Cmax_ar%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV_0%28x%27%29)

- ![policy2](http://latex.codecogs.com/svg.latex?V_%7Bt%2B1%7D%28x%29%3D%5Cmax_ar%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV_t%28x%27%29)

Iterate until convergence.

Algorithm:

1. initialize ![policy0](http://latex.codecogs.com/svg.latex?V_0%28x%29%3D%5Cmax_ar%28x%2Ca%29)
2. For t=1 to infty:
   1. For each x, a, let ![policy0](http://latex.codecogs.com/svg.latex?Q_t%28x%2Ca%29%3Dr%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV_%7Bt-1%7D%28x%27%29)
   2. For each x let Vt(x)=max_a Qt(x,a)
   3. Break if ![breakcond](http://latex.codecogs.com/svg.latex?%7C%7CV_t-V_%7Bt-1%7D%7C%7C_%7B%5Cinfty%7D%3D%5Cmax_x%7CV_t%28x%29-V_%7Bt-1%7D%28x%29%7C%5Cleq%5Cepsilon)

Guarantee:

- converge to epsilon-optimal policy. The proof is Bellman update (using Vt to update Vt+1) is a contraction. ![bellupdate](http://latex.codecogs.com/svg.latex?B%3A%5Cmathbb%7BR%7D%5En%5Cto%5Cmathbb%7BR%7D%5En%2C%20B%3AV%5Cto%20BV.%20%28BV%29%28x%29%3D%5Cmax_ar%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV%28x%27%29)

  There is a theorem: ![bellupdate](http://latex.codecogs.com/svg.latex?%5Cforall%20V%2CV%27%5Cin%5Cmathbb%7BR%7D%5En%2C%20%7C%7CBV-BV%27%7C%7C_%7B%5Cinfty%7D%5Cleq%5Cgamma%7C%7CV-V%27%7C%7C_%7B%5Cinfty%7D)

  A contraction has two important properties:

  - existence of a unique fixed point
  - convergence to the fixed point

  Convergence rate:

  - number of iterations: depends a lot on initial  policy and epsilon.
  - in each iteration:  the complexity is ![complexity](http://latex.codecogs.com/svg.latex?O%28n%5E2%5Ccdot%20m%29)

- **no** monotonical property.

#### Summary

In practice, which one is better depends on the application. Can combine both of them. Like take one greedy policy in some step of policy iteration as the initial policy for value iteration.



# POMDP

Partially observed markov decision process = controlled HMM = Belief-state MDP

We have a series of observations Y1,...,Yt. The key idea is the last belief Xt, including all information from the previous observations. In other words, if we have Xt, we can forget all of Y1,...,Yt. Instead of solving markov decision process over the state space, we are going to solve MDP over the belief of state space. 

Key idea: interpret POMDP as an MDP with enlarged state space: new states correspond to beliefs P(Xt|y1:t) in the original POMDP.

Once we have the newest observation Yt+1, applying Bayesian filtering, a deterministic function, to update on the observations

## Settings

- the set of state space: X1,X2,...,Xn
- the set of action space: A1,A2,...Am
- the set of observation space: Y1,Y2,...Yk
- Transition probability: P(Xt+1|Xt,At)
- observation probability: P(Yt|Xt)

Consider at time t, 

- we have the belief Bt=bt, where bt is an instance of n-dimension probability vector.
- Suppose we take the action at 
- and observe yt+1.

In this way,

 ![belief](http://latex.codecogs.com/svg.latex?b_%7Bt%2B1%7D%28x%29%3D%5CPr%5BX_%7Bt%2B1%7D%3Dx%7Cy_%7B1%3At%2B1%7D%2Ca_%7B1%3At%7D%5D)

![belief](http://latex.codecogs.com/svg.latex?%3D%5Cfrac%7B1%7D%7BZ%7D%5Csum_%7Bx%27%7D%5CPr%5BX_t%3Dx%27%7Cy_%7B1%3At%7D%2Ca_%7B1%3At-1%7D%5D%5CPr%5BX_%7Bt%2B1%7D%3Dx%7CX_t%3Dx%27%2Ca_t%5D%5CPr%5BY_%7Bt%2B1%7D%3Dy_%7Bt%2B1%7D%7Cx%5D)

That means ![belief](http://latex.codecogs.com/svg.latex?b_%7Bt%2B1%7D%3Df%28b_t%2Ca_t%2Cy_%7Bt%2B1%7D%29)

## Transformation

Transform into a new MDP problem:

- states: beliefs over states for original POMDP: ![belief-state](http://latex.codecogs.com/svg.latex?%5Cmathcal%7BB%7D%3D%5C%7Bb%3A%5C%7B1%2C2%2C%5Ccdots%2Cn%5C%7D%5Cto%5B0%2C1%5D%2C%5Csum_xb%28x%29%3D1%5C%7D)

- actions: same as original MDP

- Transition model: 

  - stochastic observations: the observation of the next time only depends on our belief of the current time and the action we take. ![belief](http://latex.codecogs.com/svg.latex?%5CPr%5BY_%7Bt%2B1%7D%3Dy%7Cb_t%2Ca_t%5D%3D%5Csum_x%20b_t%28x%29%5CPr%5BY_%7Bt%2B1%7D%3Dy%7CX_t%3Dx%2Ca_t%5D)
  - state update (Bayesian filtering): Given bt, at, yt+1: ![belief](http://latex.codecogs.com/svg.latex?b_%7Bt%2B1%7D%28x%27%29%3D%5Cfrac%7B1%7D%7BZ%7D%5Csum_x%20b_t%28x%29%5CPr%5BX_%7Bt%2B1%7D%3Dx%27%7CX_t%3Dx%2Ca_t%5D%5CPr%5By_%7Bt%2B1%7D%7Cx%27%5D)

- Reward function: ![belief](http://latex.codecogs.com/svg.latex?r%28b_t%2Ca_t%29%3D%5Csum_x%20b_t%28x%29r%28x%2Ca_t%29)

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