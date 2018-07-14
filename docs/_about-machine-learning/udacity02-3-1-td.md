---
title: "[Udacity02] 3-1. TD and friends" 
excerpt: "Temporal Difference Learning"
mathjax : True
---
## RL context
![RL context]({{ site.url }}{{ site.baseurl }}/assets/images/rlcontext.png)
rlcontext
- Model-based
    + takes the state, action, reward
    + sends them to a model learner, which learns the transitions and rewards.
    + Those transition and rewards, once you've learned them, you can put through an MDPSolver, which could be used to spit out a Q*, a optimal value function.
    + taking the argmax of the state that you are in, you can choose which action you should take in any given state and that gives you the policy.
    + It's still mapping the state action reward sequence to policies, but it's doing it by creating all these intermediate values in between.
- Value-function-based, Model-free
    + We're taking sequences of state action rewards and producing a policy and we even have this Q* in between that we generate the policy from using the argmax.
    + but now, instead of feeding back the transitions and rewards, we're actually feeding back Q* and we have a direct value update equation that takes state action reward that it just experienced, the current estimate of Q.
    + uses that to generate a new Q, which is then used to generate a policy.
    + it just somehow directly learns the Q value from state action and rewards.
- Policy search
    + you can sometimes actually take the policy itself and feed that back to a policy update that directly modifies the policy based on the state action and rewards that you receive.
    + this is much more direct, but learning problem is very difficult because the kind of feedback that you're getting about the policy isn't really very useful for directly modifying the policy.

> A lot of the reseracher has been focused on number two, because in some ways it strikes a nice balance between keeping the computations relatively simple and keeping the learning updates relatively simple.
 
But there are plenty of cases where you'd rather do one and where you'd rather to do three.
