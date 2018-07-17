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

## TD Lambda : Learning to predict over time

- TD : temporal difference
![Example]({{ site.url }}{{ site.baseurl }}/assets/images/tdex.png)
{: .image-center}

## Estimating From Data
- Let's think about how we can get those same values from data. Instead of you knowing in advance what the model of this particular Markov Chain.
- Imagine what happens is we're going to do a series of simulations starting from S1. It's going to hop around and with some probability it's going to hop. 
- There are the sequences that come out of episodes.
- How we would actually estimate, develop an estimate of the value of state one. Given just this data after say the first three episodes, or the first four episodes. The fifth episode doesn't involve S1 so presumably it shouldn't.

![Estimate]({{ site.url }}{{ site.baseurl }}/assets/images/est.png)
{: .image-center}

- do an expectation, average things.
- how we can incrementally compute an estimate for the value of a state, given the previous estimate. Given after 3 episodes, how do we get 4. and how do we get 5, 6....10000, 10001...
- how close we estimate?
- last quiz V(s1) was 2.9.<--- with an infinite amount of data.
    + In this case, we get a little bit high value. why? Because we just haven't seen enough data yet, we saw 11 once for 3 episodes. 11 happens a third of the time instead of a tenth of the time.
    + We have kind of an over-representation of the higher reward. So our estimate ends up being skewed high. 

## Computing Estimates Incrementally

- derive a piece of the temporal difference equation.
- ${ V }\_{ T-1 }({ S }\_{ 1 })\quad { R }\_{ T }({ S }\_{ 1 })\quad V\_{ T }({ S }\_{ 1 })$
- ${ V }\_{ T }({ S }\_{ 1 })=\frac { (T-1){ V }\_{ T-1 }(S\_{ 1 })\quad +\quad { R }\_{ T }({ S }\_{ 1 }) }{ T }$ 
- $= \frac { T-1 }{ T } { V }\_{ T-1 }({ S }\_{ 1 })\quad +\quad \frac { 1 }{ T } { R }\_{ T }({ S }\_{ 1 })$
- $= { V }\_{ T-1 }({ S }\_{ 1 })\quad +\quad { \alpha  }\_{ T }({ R }\_{ T }({ S }\_{ 1 })\quad -\quad { V }\_{ T-1 }({ S }\_{ 1 })),\quad where\quad { \alpha  }\_{ T }\quad =\quad \frac { 1 }{ T }$
- ${ \alpha  }\_{ T }$ : learning rate parameter.
- ${ R }\_{ T }({ S }\_{ 1 })\quad -\quad { V }\_{ T-1 }({ S }\_{ 1 })$ : kind of a temporal difference that our update to the value is going to equal the difference between the reward that we got at this step and the estimate that we had at the previous step. This difference drives the learning. 
    + If the difference is zero then things don't change.
    + If the difference is big positive, then it goes up.
    + If it's big negative then it goes down.
    + As we get more episodes this learning parameter is getting smaller and smaller so we're making smaller and smaller changes. We're kind of honing in on the actual true values.
    + error : ${ R }\_{ T }({ S }\_{ 1 })\quad -\quad { V }\_{ T-1 }({ S }\_{ 1 })$

## Properties of Learning Rates

- This ${V}\_{T}$ will actually go to the true expectations, the actual average value of the Rs once T is big enough.
- There are conditions that we have to put on the learning rate sequence.
- The learning rate sequence has to have the property that if you sum up all the learning rates, it actually equals to infinity.
- But the square of the learning rates, if you sum those up, it's less than infinity.(we're not going to get in the details about why these are the standard properties for making this kind of learning rule work.)
- But the first approximation, the learning rates have to be big enough so that you can move to what the true value is, no matter where you start.
- But they can't be so big that they don't damp out the noise and actually do a proper job of averaging.
- If you just accept that these are the two conditions that make sure that that's true, that gives us a way of choosing different kinds of learning rate sequences.

> What's some alphas that tease to satisfy that and some that don't?

![Learning rates]({{ site.url }}{{ site.baseurl }}/assets/images/learningrates.png)
{: .image-center}

- No. 2 : harmonic sequences, sum of them is infinity.
- It turns out that any time that the power that we're raising, the T to in the denominator is bigger than one, it's going to converge.
- the exponent was greater than 1, then it would converge and if it was less than 1, then it would not converge. 
