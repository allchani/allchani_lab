---
title: "[Udacity02] 02. Smoov & Culry's Bogus Journey" 
excerpt: "More about Rewards and Sequences of Rewards"
mathjax : True
---
## More about Rewards
General observation about rewards and what makes the reinforcement learning MDP problem different from the supervised learning problems that we did before.

|    --    |    --    |    --    |    G+1   |
|:--------:|:--------:|:--------:|:--------:|
|    --    |   __B__  |    --    |  __N__-1 |
|    --    |    --    |    --    |    --    |


- delayed reward : not a idea of getting a reward at every state, it's an idea of getting delayed reward. So you don't know how your immediate action is going to lead to things down the road.
    + All you know is that you're taking a bunch of actions. You get rewards signals back from the environment.
    + What was the action that led to me ultimately winning or losing, or getting whatever reward that I got at the end of sequence.
    + supervised learning: State --> specific action
    + reinforce learning : state, some actions, some rewards
    + temporal credit assignment problem

|-->|-->|-->|G+1|
|:-:|:-:|:-:|:-:|
|↑|__B__|__↑__|__N__-1|
|↑|<--|<--|<--|

- the minor changes to your reward function actually matter.
    + why should go left(1,3) is that this way has low probability go into -1. Because of stochastic situation.
    + depending on the reward, game differently ends.
    + So, choose the reward __carefully and with some forethought__
    + Reward is sort of teaching signal that tells you ought to do, and what you ought not to do.
    + The rewards are our domain knowledge.
    + Inject domain knowledge.

## Sequences of Rewards : Assumptions

>main assumtion = stationary

- Infinite Horizons
    + If you don't have an infinite horizon but you have a finite horizon the tow things happen.
        * 1. Policy might change because things might end.
        * 2. but secondary and importantly, the policy can change or will change, even though you're in the same state.
            - time steps infinite vs finite(100000 vs 3)
        * Markovian thing said, it doesn't matter where I've been it only matters where I am. --> and I'm going to want to take the same action.
        * finite horrizon, depending on step that's left, you might take a different action.
    + Without this infinite horizon assumtion, you loose this function of stationary in your policies.

- Utility of Sequences
    + We have been sort of implicitly discussing not just the rewards we get in a sigle state, but the rewards that we get through a sequences of states that we take.
    + if U(S0, S1, S2...) > U(S0, S1', S2'...) then U(S1, S2...) > U(S1', S2'...)
        * stationarity of preferences. Even thoungh we don't have S0.
        * It will be the same, because all we're doing is adding the reward for S0. 
    + Math version
        * $U({ S }\_{ 0 },\quad { S }\_{ 1 },\quad { S }\_{ 2 }...)\quad =\quad \sum \_{ t=0 }^{ \infty  }{ R( } { S }\_{ t })$
            - +1 +1 +1 +1 +1 +1 +1 ....
            - +1 +1 +1 +2 +1 +1 +2 ....
                + which is better?
                    * we can't compare because the utility of both of those sequences is equally infinite!! So same.
        * = $\sum \_{ t=0 }^{ \infty  }{ \gamma  } ^{ t }{ R( }{ S }\_{ t })$
            - $0\quad \le \quad \gamma \quad <\quad 1$
            - It dosen't exponentially blow up. It exponencially implodes.
        * $\le \sum \_{ t=0 }^{ \infty  }{ { \gamma  }^{ t } } { R }\_{ MAX }\quad =\quad \frac { { R }\_{ MAX } }{ 1-\gamma  }$
            - discounted --> geometric, infinite --> finite
            - gamma value means that at any given point in time, you only really have to think about what amounts to a finite distance. 
            - Think about Horizon!, sigularity



