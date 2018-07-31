---
title: "[Udacity02] 3-2. TD and friends" 
excerpt: "TD Rules"
mathjax : True
---

## TD(1) Rule

- Episode T
    + For all s, e(s) = 0 at start of episode, ${V}\_{T}(S)={V}\_{T-1}(S)$, e is eligibility
    + After ${ s }\_{ t-1 }\xrightarrow \[ { r }\_{ t } \]{  } { s }\_{ t }$ : (step t) 
        * $e({s}\_{t-1})=e({s}\_{t-1}) + 1$
    + or all s,
        * ${ V }\_{ T }(s)\quad =\quad { V }\_{ T }(s)\quad +\quad { \alpha  }\_{ T }({ r }\_{ t }+\gamma { V }\_{ T-1 }({ s }\_{ t })-{ V }\_{ T-1 }({ s }\_{ t-1 }))e(s)$
        * $e(s)\quad =\quad \gamma e(s)$

- this leaves us in a position where we can actually state the TD(1) update rule.
- for each time we start a new episode, we're going to initialize this thing called the eligibility, e(s) for all the states, to 0. we just start them off as ineligible at the start of the episode.
- We start off our estimate for the value function for this episode, to be initialized to whatever it was at the end of the previous episode.
- each time we take a step within the episode, a step from some st-1 to some state st, getting reward rt, what we're going to do is first.
- update the eligibility of the state that we just left.
- Then go through all the states, compute this one quantity which is the same for all states, this is the temporal difference.
- _sum of the reward + the discounted value of the state that we just got to - the state that we just left_
- we're going to apply that temporal difference to all states, proportional to the eligibility of those states, and of course the learning rate, because we don't want to move too much. 
- And we're just going to update them by that.
- we're going to increase them by this temporal difference times the eligibility. 
- Then we decrease, or decay, the eligibility for those states. 
- And then we're back up to the next step.
  
  
  
- S's are all being done in parallel here.
- So the value at state S is going to be updated based on this quantity, which is the same for everybody, doesn't depend on which S we're currently updating, and e(s) is specific to the S that we're looking at. It's all kind of happening in parallel without any dependencies between them. 
- Because there's an order to the states and the way we're going to visit them, you basically get to see that value kind of applied everywhere.

그림넣고...

1. We say that the eligibility for all of the states is 0.
2. The new values, the new value function is whatever the value function was at the end of the previous episode.
3. Make a transition. the first transition we make is from s1 to s2 to with reward r1. What does that do to the eligibility?
    4. s1 it sets the eligibility to 1.
5. We're now going to loop through all the states all of them. we're going to apply the same little update to all of them. And the update has the form, whatever the current learning rate is times the reward that we just experienced, the R1 plus gamma times whatever the previous iteration value was for the state that we landed in. Minus the previous iteration's value of the state that we just left.
6. This quantity is going to actually get added to all the states, but the amount that it's going to get added to all the states is proportional to the eligibility for that state. And since, at the moment, states s2 and s3 are ineligible, they have their eligibility set to 0. We're only making an update with respect to state s1.
7. decay eligibility
8. on and on and on

> Claim : TD(1) is the same as outcome-based updates(if no repeated states)

### If there were repeated states...

- If we were doing outcome-based updates, then basically, you're just looking at all the rewards that you see, the values that you kind of expected along the way, and it's exactly what you have written down before. 
- But if we had a repeated state, then you're basically ignoring anything you learned along the way during the episode. So outcome-based updates learn nothing during the episode. 
- So if your sequence up there, instead of being S1, S2, S3, SF, had been S1, S2, S3, S1, SF. Then what would have happened is I would have seen the rewards that I saw, r1, r2, r3 and then some other reward that I would see. Let's call it r1 prime or something. 
- I would be pretending as if I didn't already know something about state one, because I also saw it go from state one to state two, and saw a particular reward r1. I'd be ignoring that. 
- So what this TD(1) update let's you do is, when you see S1 again, and sort of back up its value. You've actually captured the fact that last time you were in S1, you actually went to S2 and saw reward r1.
-  