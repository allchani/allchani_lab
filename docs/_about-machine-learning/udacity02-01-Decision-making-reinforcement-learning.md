---
title: "[Udacity02] 01. Smoov & Culry's Bogus Journey" 
excerpt: "Markov Decision Processes"
mathjax : True
---

## Decision Making & Reinforcement Learning 

- Supervised Learning : $y=f(x)$ - function approx
- Unsupervised Learning : $f(x)$ - clustering, description
- Reinforcement Learning : $y=f(x), z$
    + superficially a lot like supervised learning
    + a string of pairs of data
    + we were given a bunch of x, z and y pairs. We were asked to learn F,
    + reinforcement learning is one mechanism for doing decision making.

## Markov Decision Processes
|          |          |          |G         |
|:--------:|:--------:|:--------:|:--------:|
|          | __B__    |          | __N__    |
| __S__    |          |          |          |

- In this world we can go Up, Down, Right, Left.
- Probabiliy an action happens : 0.8 
- others 0.2 to move at a right angle direction.

### Problem
- States : S - represent something
    + ex) start state : (1,1) goal state(4,4) but we can state with everything like 1, 2, 3 or Fred mark Jason...
- Model : T(s, a, s') ~ Pr(s' \| s, a)
    + T : transition function, 
    + s : state, a : action, a' : another state
    + Pr(s' \| s, a) : the probability that you will end up transitioning the state s', given that you were in state s, and you took action a.
    + deterministic world vs non-deterministic(stochastic) world
- Action : A(s), A
    + A(s) : the thing that you can do in a particular state
    + ex. up down left right,(not move, teleport)
- Reward : R(s), R(s,a), R(s, a, s')
    + scalar value
    + you get for being in a state
    + tells you the usefulness of entering into that state.
--------

The Markovian property is that 
- only the present matters. So, Pr(s' \| s, a) is only depends on the current state.
- things are stationary. (The rules don't change)

----
### Solution to Markov Decision process
- Policy : $\prod { (s) }$ --> a
    + takes a state, return an action
    + any given state that you're in, it tells you the action that you should take(not a hint, will be taken, it's command)
    + ${ \prod }*$ : the optimal policy, and that is the policy that maximizes your long-term expected reward. 
    + <s, a, r>, <s, a, r> .....like (x, y, z)
    + a function that tells you what action to take at every, in any state you happen to come across.
    + policy ask to you that what state you are in. tell me what action you should take
    + Markov Decision Process tells you what action to take in a particular state.
    + it doesn't tell you what sequence of actions to take from a particular state. and it doesn't talk about plans directly.
    + infer a plan, 
    + tells us what to do in any state, anywhere

### Next, given that we have MDP, How do we go from this problem definition to finding a good policy, in particular, finding the optimal policy.
