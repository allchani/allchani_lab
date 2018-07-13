---
title: "[Udacity02] 04. Smoov & Culry's Bogus Journey" 
excerpt: "The Bellman Equation"
mathjax : True
---

## The Bellman Equation

$$V(s)\quad =\quad \max \_{ a }{ (R(s,\quad a)\quad +\quad \gamma \sum \_{ s' }^{  }{ T(s,\quad a,\quad s')V(s') } ) }$$

![bellman equation]({{ site.url }}{{ site.baseurl }}/assets/images/bellman.png)
{: .image-center}

- $V(s\_{ 1 })=\max \_{ a\_{ 1 } } (R(s\_{ 1 },\quad a\_{ 1 })+\gamma \sum \_{ s\_{ 2 } } T(s\_{ 1 },a,s\_{ 2 })\max \_{ a\_{ 2 } } (R(s\_{ 2 },\quad a\_{ 2 })+\gamma \sum \_{ s\_{ 2 } } T(s\_{ 2 },a,s\_{ 3 })\cdots$ \]
- stripe bracket has traditionally represented the balancing of all left parentheses.
![bellman equation02]({{ site.url }}{{ site.baseurl }}/assets/images/bellman02.png)
{: .image-center}

- $Q(s, \quad a)=R(s,\quad a)+\gamma \sum \_{ s' } T(s,a,s')\max \_{ a' } Q(s', \quad a')$
    + This gives us another Bellman equation that instead of starting at the S point. kind of starts after the action has started.
- V is value, Q is quality.
- Why Q equation??
    + Q form of the bellman equation is going to be much more useful in the context of reinforcement learning. 
    + The reason for that is we're going to be able to take expectations of this quantity(quality equation의 우변) using just experienced data.
    + And we don't need to actually to have direct access to the reward function or the transition function to do that.(where we don't know the transitions and rewards in advance)
    + Whereas if we're going to try to learn V values, the only way to connect one V value to the next V value is by knowing the transitions and rewards.

![bellman equation03]({{ site.url }}{{ site.baseurl }}/assets/images/bellman03.png)

![bellman equation quiz]({{ site.url }}{{ site.baseurl }}/assets/images/bellman04.png)

![The relation between bellman equations]({{ site.url }}{{ site.baseurl }}/assets/images/bellman05.png)

## What Have we Learned?

- MDPs (Scooby Doo MDP) : S, A, T, R...
- Q functions (relation to U and V)
- C functions (continuations)
- Bellman Equations.
