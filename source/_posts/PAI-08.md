---
title: Reinforcement Learning
date: 2020-01-29
tags: [probabilistic artificial intelligence, reinforcement learning]
categories: course notes
---

The data is assumed to be drawn from some distributions. In reinforcement learning, we learn by interacting with environment. For example, one agent could perform some actions, and these actions will give him different rewards. He wants to learn how to take actions. One big issue is that the reward is based on a sequence of actions, not only one action. Considering the case that a sequence of actions leads to some reward, how to figure out exactly which action is responsible for this reward. Generally, it is impossible. We need some assumptions.

# Background

RL = planning in unknown MDPs. 

- not know the transition probability
- not know the rewards
- may not even know the states.

In this way, how to come up a policy to react?

A central object is the value function. In order to solve MDPs, we need to solve the value function.

Difference from supervised learning:

- in RL, the data we get depends on what we did. The data is not i.i.d.
- what we do will affect what we learn
- have rewards. Not only achieve to learn a good model, but also a good policy to how to act to get a good reward.

Main tasks:

- explore: how to act to get more knowledge of the world?
- exploit: once knowing the world, how to act to get a good reward?

Types:

- on-policy RL: agents have full control over which actions to pick
- off-policy RL: agents have no control over actions, only gets observational data. 

# Assumptions

- Markovian: the actions only depend on the current state, instead of past states.

- finite states
- finite actions

# Approaches

- model-based RL:
  1. learn the MDP
     - estimate the transition probabilities P(x'|x,a)
     - estimate reward function r(x,a)
  2. optimize policy based on estimated MDP
- model-free RL:
  - jump the transition probabilities, or reward function, but directly to estimate, what we need to learn to make a policy, *the value function*
  - policy gradient methods: constraint to some specific families of policy. Turn into an optimization problem.
  - actor-critic methods: combine both of them: create values, limit to some families of policies.

## Model-based

The data set looks like: x1,a1,r1,x2,a2,r2,... given an observation xi, take an action ai and get a reward ri, which leads to the next observation xi+1.

Elements: (xi,ai,ri,xi+1) are independent from each other.

D = {(xi,ai,ri,xi+1)}, expressed in this way, we can do counting on the elements.

MDP can be viewed as controlled Markov chain. 

- estimate transitions (MLE):

   ![est-trans](http://latex.codecogs.com/svg.latex?%5CPr%5BX_%7Bt%2B1%7D%7CX_t%2CA%5D%5Csimeq%5Cfrac%7BCount%28X_%7Bt%2B1%7D%2CX_t%2CA%29%7D%7BCount%28X_t%2CA%29%7D)

- estimate rewards:

   ![est-rew](http://latex.codecogs.com/svg.latex?r%28x%2Ca%29%5Csimeq%5Cfrac%7B1%7D%7BN_%7Bx%2Ca%7D%7D%5Csum_%7Bt%3AX_t%3Dx%2CA_t%3Da%7DR_t)

  

How accurate could the estimation be? More data will give more accurate estimations.

### Trade-off exploration and exploitation

- pick a random action
  - exploration: will eventually correctly estimate all probabilities and rewards.
  - exploitation: may do extremely poorly in terms of rewards. Because each action is independent from others, not a "good" policy
- pick the currently "best" action
  - exploration: can get stuck in suboptimal actions, local best
  - exploitation: can yield some reward.

- combination these two, using **randomized** strategies, called epsilon-greedy.

  - with probability epsilon: pick a random action
  - with probability 1-epsilon: pick the best action.

  If epsilon satisfies some condition, it will converge to optimal policy with probability 1.

  Condition: **Robbins Monro condition** 

  - Potential issue: the random action doesn't rule out purely bad actions.

- The Rmax algorithm: the principle is *optimism in the face of uncertainty*. When an action is unknown, try it!

  Assume the reward has a upper bound Rmax

  - if you don't know r(x,a): set it to Rmax. That means try this action.
  - if you don't know P(x'|x,a): set P(x'|x,a)=1. That means explore a new state.

### Complexity

- memory:  store P(x'|x,a)=>O(|x|^2|A|), store r(x,a)=> O(|X||A|)
- time: solving once MDP requires poly(|X|,|A|,1/epsilon,log(1/delta). Need to do this often. 

## Model-free

According to Theorem Bellman, once we have the optimal value function, we can get a greedy policy, which is optimal.

Q^{pi}(x,a) : the expected reward of a policy pi, given the state and action pair (x,a)

![qfunction](http://latex.codecogs.com/svg.latex?Q%5E%7B%5Cpi%7D%28x%2Ca%29%3Dr%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV%5E%7B%5Cpi%7D%28x%27%29)

For the optimal policy pi^*: It holds  

![optvalue](http://latex.codecogs.com/svg.latex?V%5E%2A%28x%29%3D%5Cmax_a%20Q%5E%2A%28x%2Ca%29)

The key idea is to estimate Q*(x,a) from samples.

### Q-learning

To estimate the best policy's Q function, which is  

![optimalq](http://latex.codecogs.com/svg.latex?Q%5E%2A%28x%2Ca%29%3Dr%28x%2Ca%29%2B%5Cgamma%5Csum_%7Bx%27%7D%5CPr%5Bx%27%7Cx%2Ca%5DV%5E%2A%28x%27%29)

instead of using transition probability to get the exact expectation, we estimate from on instance (x,a,x').

![qudpate](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%29%5Cgets%20r%28x%2Ca%29%2B%5Cgamma%20V%5E%2A%28x%27%29%3Dr%28x%2Ca%29%2B%5Cgamma%5Cmax_%7Ba%27%7D%20Q%28x%27%2Ca%27%29)

To trade-off huge variance from just one sample, we weight this update by alpha

![weightqudpate](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%29%5Cgets%20%281-%5Calpha_t%29Q%28x%2Ca%29%2B%5Calpha_t%28r%28x%2Ca%29%2B%5Cgamma%5Cmax_%7Ba%27%7D%20Q%28x%27%2Ca%27%29%29)

alpha: usually decrease along the time. means: at the beginning we put much weight on a new sample. Later, we put less and less.

#### Algorithm

Random version

1. have initial estimate of Q(x,a)
2. observe transition x,a,x' with reward r. Update Q(x,a) for long enough times.

Optimistic algorithm (similar to Rmax):

starting with an optimistic initialization. But only Rmax is not enough, have to consider the discount effect and weight effect.

1. initialize ![optimisticq](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%29%5Cgets%5Cfrac%7BR_%7Bmax%7D%7D%7B1-%5Cgamma%7D%5Cprod_%7Bt%3D1%7D%5E%7BT_%7Binit%7D%7D%281-%5Calpha_t%29%5E%7B-1%7D)

#### Convergence

**Theorem** (for random) If learning rate alpha satisfies:

- never stops updating: ![alpha](http://latex.codecogs.com/svg.latex?%5Csum_t%5Calpha_t%3D%5Cinfty)
- later samples have smaller weights: ![alpha](http://latex.codecogs.com/svg.latex?%5Csum_t%5Calpha_t%5E2%3C%5Cinfty)
- and actions are chosen at random,

Then

Q learning converges to optimal Q* with probability 1.

**Theorem** (for optimistic) With probability 1-delta, optimistic Q-learning obtains an epsilon-approximation policy after a number of time steps that is polynomial in |X|,|A|, 1/epsilon and log(1/delta).

#### Complexity

- memory: store the Q-table: Q(x,a): O(|X||A|)
- time: 
  - update per sample: find the action which gives the maximum Q(x',a'), O(|A|) 
  - iterations: polynomial in |X|,|A|, 1/epsilon and log(1/delta).

#### Parametric Q-function approximation

The general idea is that we don't update the entries of Q table one by one in the update, but use some parameters to calculate the the entries and update the parameters in each step. In this way, we turn the question into an optimization problem and can solve it by approximation. Also, this approach allows us to go from the finite tabular Q to infinite Q.

At convergence, we want:

![qexp](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%29%3D%5Cmathbb%7BE%7D_%7B%28r%2Cx%27%29%7Cx%2Ca%7D%28r%2B%5Cgamma%5Cmax_%7Ba%27%7DQ%28x%27%2Ca%27%29%29)

![qexp](http://latex.codecogs.com/svg.latex?%5CRightarrow%5Cmathbb%7BE%7D_%7B%28r%2Cx%27%2Cx%2Ca%29%7D%28Q%28x%2Ca%29-r-%5Cgamma%5Cmax_%7Ba%27%7DQ%28x%27%2Ca%27%29%29%3D0)

![qexp](http://latex.codecogs.com/svg.latex?%5CRightarrow%5Cmin_%7B%5Ctheta%7D%5Cmathbb%7BE%7D_%7B%28r%2Cx%27%2Cx%2Ca%29%7D%5BQ%28x%2Ca%29-r-%5Cgamma%5Cmax_%7Ba%27%7DQ%28x%27%2Ca%27%29%5D%5E2)

In this way, we can use mean to approximate expectation.



Example of parametric q-function: linear function approximation: ![paramq](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%3B%5Ctheta%29%3D%5Ctheta%5ET%5Cphi%28x%2Ca%29)

Fit parameters to data: define the loss function on parameters as:

![lossq](http://latex.codecogs.com/svg.latex?L%28%5Ctheta%29%3D%5Csum_%7B%28x%2Ca%2Cr%2Cx%27%29%5Cin%20D%7D%28r%2B%5Cgamma%5Cmax_%7Ba%27%7DQ%28x%27%2Ca%27%3B%5Ctheta%5E%7Bold%7D%29-Q%28x%2Ca%3B%5Ctheta%29%29%5E2)

So, the goal is to find parameters to minimize the loss function:

![loss-mini](http://latex.codecogs.com/svg.latex?%5Ctheta%5E%2A%3D%5Carg_%7B%5Ctheta%7D%5Cmin%20L%28%5Ctheta%29)

- label: the estimation of Q entry given the observed sample:![label](http://latex.codecogs.com/svg.latex?r%2B%5Cgamma%5Cmax_%7Ba%27%7DQ%28x%27%2Ca%27%3B%5Ctheta%5E%7Bold%7D%29)
- prediction given theta: ![pred](http://latex.codecogs.com/svg.latex?Q%28x%2Ca%3B%5Ctheta%29)

#### Deep learning

Recall what deep learning does: deep learning is a tool to solve the loss minimization problem, given data:

![dlobj](http://latex.codecogs.com/svg.latex?w%5E%2A%3D%5Carg_w%5Cmin%20%5Csum_%7Bi%3D1%7D%5EN%20l%28y_i%2Cf%28x_i%3Bw%29%29)

by fitting nested nonlinear function of f(x;w)

#### Deep Q Networks

this is a variant of Q-learning:

- use convolutional neural nets to approximate Q function
- important empirical insights:
  
  - maintain constant "target" values across episodes. save the initial labels as y, and use these ys throughout training, instead of calculating new label in each iteration.
  
  - double DQN: two networks, use old parameters to evaluate Q function, but new parameters for action selection. Want to use old parameters to calculate the value to avoid oscillations. Current parameters are more closed to the policy would do.
  
     ![double](http://latex.codecogs.com/svg.latex?L%28%5Ctheta%29%3D%5Csum_%7B%28x%2Ca%2Cr%2Cx%27%29%5Cin%20D%7D%28r%2B%5Cgamma%20Q%28x%27%2C%5Chat%7Ba%7D%28x%2C%5Ctheta%29%3B%5Ctheta%5E%7Bold%7D%29-Q%28x%2Ca%3B%5Ctheta%29%29%5E2)
  
    where ![hata](http://latex.codecogs.com/svg.latex?%5Chat%7Ba%7D%28x%2C%5Ctheta%29%3D%5Carg_%7Ba%27%7D%5Cmax%20Q%28x%2Ca%27%2C%5Ctheta%29)
  



### Policy search methods

Learning a policy without the detour of learning a value function.

Given a policy, do forward sampling on the controlled Markov chain and evaluate this policy J(theta). Then, adjust the parameters by gradient descent or other methods to get a new policy.

#### Bayesian Learning for policy search

Go beyond point estimation, but the whole distribution.

Using the Bayesian, on the data domain, find which part we are not certain about and which part we are certain about and then guide the samples drawing procedure.

### Summary

Model-based (MDP) and model-free (RL) are polynomial in |A| and |X|. However, structured domains (|A|,|X| exponential in #agents) and continuous domains (|A| and |X| are infinite) are not applicable.

# Outlook

## Bayesian Deep RL

Express the uncertainty of the model itself, by maintaining multiple models, ensemble of models.

How to go beyond point estimate's uncertainty?

## Improving action selection by planning

Beyond one-step transitions, multiple steps forward to plan. 

## Risk in exploration

### Safe Bayesian optimization

not only consider the reward, but also maintains safety constraints. Try to never violate these constraints. Formally,![constraint](http://latex.codecogs.com/svg.latex?%5Cmax%20f%28x%29%2C%20s.t.%20g%28x%29%5Cgeq%5Ctau)

One challenge is that none of f(x), g(x) are given in closed form, access only via noisy black box.

Approach: 

1. find a safe start point
2. go to reachable optimal points, instead of global optimal.



![alpha](http://latex.codecogs.com/svg.latex?