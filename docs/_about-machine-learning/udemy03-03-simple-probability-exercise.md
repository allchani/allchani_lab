---
title: "[Udemy03] 03. Simple Probability Problem & Monty Hall Problem" 
excerpt: Probability Exercise
mathjax : True
---

### Probability Exercise
- We have a fair coin : p(H)=p(T)=0.5
- We plan to toss the coin 200 times in total
- After 20 tosses, we have 15H, 15T
- What is the total # of heads we expect to get by the end of the experiment?
- (for N = 200)
    + answer) We expect 105, since the first 15 are given, and we are just trying to guess the next 180 outcomes
    + 15 H is already given
    + The expected value for the next 180 tosses is 90
    + so 90 + 15 = 105

### Gambler's Fallacy
- This mistake is so common it has a name - "gambler's fallacy"
- It is believing that things will "even out" in the end
- E.g. You just lost 100 times, you must have a better chance of winning next!
    + --> Incorrect
- All games are independent.
- Doesn't matter how many times you have lost already. Your chances of losing next are the same as they have always been.

### Monty Hall Problem
- Famous problem in probability, inspired by a TV game show
- TV show was "Let's Make a Deal", host was "Monty Hall", hence "The Monty Hall Problem"


- How the game works
    + You pick a door(you don't get to see what's behind it) (door #1)
    + Monty Hall opens a door you didn;t pick, reveals a goat (door #2)
    + You are given a choice: stay with door #1, or switch to door #3
        * Which do you choose?

- Solution
    + Assume we choose door #1 (each probability is conditioned on this, so we just won't show it at all)
    + C = where the car really is
    + p(C=1) = p(C=2) = p(C=3) = 1/3
    + H = door that Monty Hall opens
    + Assume he opens door #2 without loss of generality, problem is symmetric
    + p(H=2 \| C=1) = 0.5
    + p(H=2 \| C=2) = 0
    + p(H=2 \| C=3) = 1

- Solution cont.
    + What probability do we actually want?
    + Since we want to know whether to stick with door#1 or switch to door #3:

$$p(C=1 | H=2), p(C=3 | H=2)$$

```
p(C=3 | H=2) = p(H=2 | C=3)p(C=3)/[p(H=2 | C=1)p(C=1) + p(H=2 | C=2)p(C=2) + p(H=2 | C=3)p(C=3)]
             = 1*1/3 / (1/2*1/3 + 0*1/3 + 1*1/3) = 2/3
```

Can similarly show that p(C=1 \| H=2) = 1/3

Therefore, we should always switch.
