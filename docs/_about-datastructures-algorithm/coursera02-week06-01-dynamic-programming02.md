---
title: "[Coursera02] Algorithmic Toolbox week06" 
excerpt: "Dynamic Programming 2"
mathjax : True
---
## Knapsack
### Problem Overview
- TV commercial placement
    + Select a set of TV commercials(each commmercial ahs duration and cost) so that the total revenue is maximal while the total length does not exceed the length of the available time slot.
- Optimizing data center performance
    + Purchase computers for a data center to achieve the maximal performance under limited budget.

- Problem Variations
    + fractional knapsack : can take fractions of items --> greedy algorithm
    + discrete knapsack : each item is either taken or not --> dynamic programming solution
        * with repetitions : unlimited quantities
        * without repetitions : one of each item
- Example
    + (30,6), (14,3), (16,4), (9,2)
    + w/o repeats : (30,6), (16,4) total:46
    + w repeats : (30,6), (9,2),(9,2) total:48
    + fractional : (30,6), (14,3), (9,2)/2 total:48.5

- Why does greedy fail for the discrete knapsack?
    + taking an element of maximum value per unit of weight is not safe!

### Knapsack with Repetitions
- With repetitions: unlimited quantities of each item.
- Knapsack with repetitions problem
    + input : wieghts w1,...,wn and values v1,...,vn of n items; total weight W(vi's, wi's, and W are non-negative integers).
    + output : The maximum value of items whose weight does not exceed W. Each item can be used any number of times.

- Subproblems
    + Consider an optimal solution and an item in it:
        * W-wi
    + If we take this item out then we get an optimal solution for a knapsack of total weight W-wi.

>To come up with a dynamic programing algorithm, let's analyze the structure of an optimal solution. For this consider some subset of items, of total weight, at most capital W, whose total value is maximal. And let's consider some element i in it, let's see what happens if we take this element out of this solution. So what remains is some subset of items whose total weight is at most capital W minus wi. Right? So this is easy. What is crucial for us is that the total value of this remaining subset of items must be optimal. I mean it must be maximal amount all subset of items whose total weight is at most capital w minus w i. Why is that? Well, assume that there is some other subset of items whose total weight is at most, capital W- wi, but whose total value is higher? Let's then take the highest item and put it back to this subset of items. What we get, actually, is the solution to our initial problem of higher value. I mean, its total weight is at most capital W, and its value is higher than the value of our initial solution. But these contradicts to the fact that we started with an optimal solution. 

>So, such trick is known as cut and paste trick. And it is frequently used in designing dynamic programming algorithms. So, let me repeat what we just proved. If we take an optimal solution for a knapsack of total weight W and take some item i out of it, then what remains must be an optimal solution for a knapsack of smaller weight. 

So this suggests that we have a separate subproblem for each possible total weight from zero to capital W. Namely, let's define value of w as a optimal total value of items whose weight is, at most w.

- Subproblems
    + Let value(w) be the maximum value of knapsack of weight w.
    + $value(w)\quad =\quad \max \_{ i:{ w }\_{ i }\le w }{ \{ value(w-{ w }\_{ i })+{ v }\_{ i }\}  }$

```
Knapsack(W):
value(0) <-- 0
for w from 1 to W:
    value(w) <-- 0
    for i from 1 to no:
        if wi <= w:
            val <-- value(w-wi)+vi
            if val > value(w):
                value(w) <-- val
return value(W)
```
--> O(n*W)

### Knapsack without Repetitions
- Without repetitions: one of each item
- Knapsack without repetitions problem
    + input: Weights w1,...,wn and values v1,...,vn of n items; total weight W(vi's, wi's and W are non-negative integers).
    + The maximum value of items whose weight does not exceed W. *Each item can be used at most once.**

- Subproblems
    + If the n-th item is taken into an optimal solution: then what is left is an optimal solution for a knapsack of total weight W-wn using items 1,2,...,n-1
    + If the n-th item is not used, then the whole knapsack must be filled in optimally with items 1,2,...,n-1.
    + For 0 <= w <= W and 0 <= i <= n, value(w,i) is the maximum value achievable using a knapsack of weight w and items 1,...,i.
    + The i-th item is either used or not:value(w,i) is equal to
        * max{value(w-wi,i-1)+vi, value(w,i-1)}

```
Knapsack(W):
initialize all value(0,j) <-- 0
initialize all value(w,0) <-- 0
for i from 1 to n:
    for w from 1 to W:
        value(w,i) <-- value(w, i-1)
        if wi <= w:
            val <-- value(w-wi, i-1)+vi
            if value(w,i) < val
                value(w,i) <-- val
return value(W,n)
```
running time : O(nW)

- Reconstructing an optimal solution
    + finding not only the optimal value for the knapsack of size of total weight. But the subset of items that lead to this optimal value itself.

### Final Remarks
```
Knapsack(w):
if w is in hash table:
    return value(w)
value(w) <-- 0
for i from 1 to n:
    if wi <= w:
        val <-- Knapsack(w-wi)+vi
        if val > value(w):
            value(w) <-- val
insert value(w) into hash table with key w
return value(w)
```
- What is faster?
    + If all subproblems must be solved then an iterative algoritm is usually faster since it has no recursion overhead.
    + There are cases however when one does not need to solve all subproblems:assum that W and all wi's are multiples of 100; then value(w) is not needed if w is not divisible by 100.

- Running Time
    + The running time O(nW) is not polynomial since the input size is proportional to logW, but not W.
    + In other words, the running time is O(n2^logW).
    + E.g., for 
    + W = 71 345 970 345 617 824 751
    + (twenty digits only!) the algoritm needs roughly 10^20 basic operations.

## Placing Parentheses
### Problem Overview
- How to place parentheses in an expression
- 1 + 2 - 3 x 4 - 5
- to maximize its value?
- example
    + ((((1+2)-3)x4)-5)=-5
    + ((1+2)-((3x4)-5))=-4
- answer
    + ((1+2)-(3x(4-5)))=6
    + to try all things, 4!
- Another example
    + what about
    + 5 - 8 + 7 x 4 - 8 + 9?
    + 5!
- Soon
    + We'll design an efficient dynamic programming algoritm to find the answer.

### Subproblems
- Placing parentheses
    + input: A sequence of digits d1,...,dn and a sequence of operations
    + op1,...opn-1 --> {+,-,x}
    + output: An order of applying these operations that maximizes the value of the expression
    + d1 op1 d2 op2 --- opn-1 dn.

- Intuition
    + Assume that the last operation in an optimal parenthesizing of
    + 5 - 8 + 7 x 4 - 8 + 9 is x:
    + (5-8+7)x(4-8+9).
    + It would help to know optimal values for **subexpressions** 5-8+7 and 4-8+9

- However
    + We need to keep track for both the minimal and the maximal values of subexpressions!
    + example:(5-8+7)x(4-8+9)
        * min(5-8+7)=(5-(8+7))=-10
        * max(5-8+7)=((5-8)+7)=4
        * min(4-8+9)=(4-(8+9))=-13
        * max(4-8+9)=((4-8)+9)=5
        * --> max((5-8+7)x(4-8+9))=130

- Subproblems
    + Let $E\_{i,j}$ be the subexpression
    + $d\_{i} op\_{i}...op\_{j-1} d\_{j}$
    + Subproblems:
        * M(i,j) = maximum value of ${E}\_{i,j}$
        * m(i,j) = minimum value of ${E}\_{i,j}$

![RecurrenceRelation]({{ site.url }}{{ site.baseurl }}/assets/images/rere.png)
{: .image-center}

### Algorithm
```
MinAndMax(i,j):
min <-- +inf
max <-- -inf
for k from i to j-1:
    a <-- M(i,k) op(k) M(k+1,j)
    b <-- M(i,k) op(k) m(k+1,j)
    c <-- m(i,k) op(k) M(k+1,j)
    d <-- m(i,k) op(k) m(k+1,j)
    min <-- min(min,a,b,c,d)
    max <-- max(max,a,b,c,d)
return (min,max)
```

- Order of Subproblems
    + When computing M(i,j) the values of M(i,k) and M(k+1,j) should be already computed.
    + Solve all subproblems in order of increasing (j-i).

![PossibleOrder]({{ site.url }}{{ site.baseurl }}/assets/images/possibleorder.png)
{: .image-center}

```
Parentheses(d1 op1 d2 op2 .. dn):
for i from 1 to n:
    m(i,i)<-di, M(i,i)<-di
for s from 1 to n-1:
    for i from 1 to n-s:
        j <-- i+s
        m(i,j), M(i,j) <-- MinAndMax(i,j)
return M(1,n)
```
running time O(n^3)

### Reconstructing a Solution
![DP_table]({{ site.url }}{{ site.baseurl }}/assets/images/dptable.png)
{: .image-center}

