---
title: "[Udacity02] 2-1. Reinforcement Learning Basics" 
excerpt: "Reinforcement Learning Basics"
mathjax : True
---
## Reinforcement Learning Basics

- When we are doing reinforcement learning is that a lot of it takes place as a conversation between **an agent** and **an environment**.
- the environment is going to reveal itself to the agent, in the form of states, S.
- You then get to have some influence on the environment by taking actions, a, and then you also receive back, before the next state, some kind of reward for the most recent state action combination.
- So this is the same kind of elements that we have in an MDP. But the important thing is that instead of just being given an MDP as some kind of a graph structure and then we get to compute on it.
- Really the computation's happening inside the head of the agent and the information about the environment is really only available through the course of this interaction.  
- the agent doesn't know the environment, it's not living inside the agent's head. Instead the agent is just experiencing the environment by interacting with it.

## Mystery Game

## Behavior Structures
- Our goal is going to be to try to create learning algorithms that are not quite as smart as you were in that example, but trying to approximate that.
- the important thing is that we have to figure out, what it is that we are trying to learn. So in this case what we're trying to learn is a behavior, we're trying to learn the way of interacting with the environment that obtains high reward.

- **Plan** : fixed sequence of actions
    + During learning
    + stochasticity
- **Conditional plan** : include "if" statements
    + have sequences, and then there's a branch point.
- dynamic replanning 
    + we're just goint to continue along the plan.
    + making predictions about what's suppose to happen next.
    + if those predictions are violated, we just generate a new plan.
    + we wait for something to break and then we react to it.
    + just in time conditinal planning because the conditionals aren't in there.
- ***Stationary Policy/Universal Plan*** : mapping from states to actions.
    + if at every state
    + very large
    + it can handle any kind of stochasticity
    + there's always an optimal stationary policy for any MDP.

## Evaluating a Policy
![policy evaluation]({{ site.url }}{{ site.baseurl }}/assets/images/evaluating-a-policy.png)

## Evaluating a Learner

- the policy is what gets output as a result of the learning process. So what if we want to actually evaluate the learner? How good is this learner?
    + Value of returned policy
    + Computational complexity (time)
    + Sample(Experience) complexity (time)
        * how much data it needs
        * there can be little trade-offs between how much time that the learner takes in terms of interactions with the environment and how much time that it takes in terms of the computations that it does between interactions.

## What have we learned?

- What RL is : agent <--> enviornment
    + maximizing rewards
- What a policy is --> plans, policies
- Evaluating plans
- Evaluating learners
    + efficiency, effectiveness
