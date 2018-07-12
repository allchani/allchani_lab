---
title: "[Udacity02] 03. Smoov & Culry's Bogus Journey" 
excerpt: "Policies"
mathjax : True
---

## Policies

- ${ \prod  }^{ \* } = arg\max \_{ \prod   }{ \quad E } \[\sum\_{ t=0 }^{ \infty  }{ { \gamma  }^{ t } } R({ S }\_{ t })\quad \| \quad \prod  \]$
- ${ U }^{ \prod   }(s)\quad =\quad E\[\sum \_{ t=0 }^{ \infty  }{ { \gamma  }^{ t } } R({ S }\_{ t })\quad \| \quad \prod  ,\quad s\_{ 0 }\quad =\quad s\]$
- ${ \prod   }^{ \* }(s)=arg\max \_{ a }{ \sum \_{ s' }{ T(s,a,s') U(s') }  }$
- $U(s)=R(s) + \gamma \max \_{ a }{ \sum \_{ s' }{ T(s,a,s') U(s') }  }$
- Bellman equation
- [참고자료](http://sanghyukchun.github.io/76/)

------

## Finding Policies : Value iteration

- $U(s)=R(s) + \gamma \max \_{ a }{ \sum \_{ s' }{ T(s,a,s') U(s') }  }$
    + We have N equations and N unkowns
    + but function max is non-linear
- Finding policies
    + ${ U }\_{ t+1 }(s)=R(s)+\gamma \max \_{ a }{ \sum \_{ s' }{ T(s,a,s'){ U }\_{ t }(s') }  }$ 
    + Start with *arbitary* utilities
    + update utilites based on neighbors
    + repeat until convergence
    + 시간이 흐림에 따라 업데이트를 한 후 값을 계속 계산해나간다.
![예제사진]({{ site.url }}{{ site.baseurl }}/assets/images/policiesquiz.png)
{: .image-center}

- 이전 강의에서 (3,2)에서 최적의 action은 위로 향하는 것이었으나 지금 상황에서는 $U\_0(s)$ 가 0이므로 왼쪽에 있는 벽을 향해 머리를 박는 것이 최적이다. 그리고 $U\_t(s)$는 시간에 따른 x state에서의 value이므로 항상 주변의 state의 update가 필요하다. 
- 즉, 초기에는 (3,2)에서의 action이 왼쪽 벽을 향하는 것이 유리하지만 다음 텀에서 (3,3)의 Utility 값이 0.36 이 되었고 (3,2)의 Utility의 값이 -0.04가 되어 위로 action을 취하는 것이 유리하게 되도록 바뀌었다.
- 그리고 $U\_1(x) = 0.36$, $U\_2(x) = 0.376$ 으로 점점 green x에서의 utility의 값이 증가하는 것으로 보아 점점 진행하면 할 수록 (3,2)에서의 action은 위를 향할 가능성이 높다.

-------

- +1 propagate out more, -1 propagate out less because for maximizing rewards. 
- policy is the fuction from state to action, not state to utility.
- U is actually much more information than we need to figure out pi.
If we have a U that is not the correct utility, but say, has the ordering of the actions correct, then we're actually doing pretty well. It doesn't matter whether we have the wrong utilities. 
- The only thing we care about is we have a right policy.
- Um. Pi is a kind of classifier to find a right policy.
- get a utility good enough to get you to your pie.

---

## Let's find!! : Policy iteration

- start with ${\Pi}\_{0}$ <-- guess
- evaluate : given ${\Pi}\_{t}$ calculate ${U}\_{t} = { U }^{\Pi\_t}$
- improve : ${\Pi}\_{t+1} = arg\max \_{ a } \sum { T(s,\quad a,\quad s') } { U }\_{ t }(s')$ 
- $U(s)\quad =\quad R(s)\quad +\quad \gamma \sum \_{ s' }^{  }{ T(s,\quad { \Pi  }\_{ t }(s),\quad s'){ U }\_{ t }(s') }$
    + there is not max fuction so it is linear.
    + n equations in n unknowns
    + ${\Pi}\_{t}(s)$ is constants.
    + we can actually compute!!
- keep iterate until a policy doen't change.
- It doesn't making jumps in value, making jumps in policy space. without worrying about the details of constants. 
- Policy iteration!!