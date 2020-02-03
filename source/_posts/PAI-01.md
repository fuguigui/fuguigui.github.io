---
title: Introduction
date: 2020-01-23
tags: [courses, Probability, AI]
categories: Probabilistic Artificial Intelligence
---

I took part in the course [Probabilistic Artifical Intelligence](https://las.inf.ethz.ch/pai-f19) in the autumn festival 2019, which is opened by Professor Andreas Krause(https://las.inf.ethz.ch/krausea). This series of notes are created in course review and contain the main ideas/concepts/methods of each lecture, my own thoughts/ideas/questions as well as out-of-class helpful materials. More detailed information please refer to the lecture notes posted on the course homepage.

http://latex.codecogs.com/svg.latex?

## Topic covered

The focus of this course is about **reasoning and decision making under uncertainty**.

- Probabilistic reasoning
  - Bayes Net
  - graphical models
- Learning
  - Bayesian deep learning
- Planning under uncertainty
  - MDPs
  - POMDPs
- (Deep) reinforcement learning

## Fundamental Settings

Use Agent-environment model to represent all tasks.

### Agent

has a set A and a function f.

- A: the elements in A are called actions.
- f: map sequence of percepts to action![img](http://latex.codecogs.com/svg.latex?f%3AP%5Cto%20A)

### Environment

has a set P, a function f, a measure function R.

- P: the elements in P are called percepts or environment states.
- f: map sequence of actions to percept ![img](http://latex.codecogs.com/svg.latex?f%3AA%5Cto%20P)
- R: evaluates any given sequence of environment states: ![img](http://latex.codecogs.com/svg.latex?R%3A%20S%5E%2A%5Cto%5Cmathbb%7BR%7D)

We want to design an agent to be rational, doing the right thing. How to define "right"?  Based on the evaluation of **environment** states. 

**A rational agent** is defined as: for each possible percept sequence, a rational agent should select an action that is expected to maximize its performance measure, given

- the evidence provided by the percept sequence 
- and whatever built-in knowledge the agent has.(???? how to show this formally?? How to mathematically evaluate the performance measurement related to agent's built-in knowledge)