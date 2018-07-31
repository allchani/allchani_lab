---
title: "[Coursera02] Algorithmic Toolbox week03" 
excerpt: "Greedy Algorithms"
mathjax : True
---
- Learning Objectives
    + Practice implementing greedy solutions
    + Build greedy algorithms
    + Create a program for changing money optimally
    + Create a program for maximizing the value of a loot
    + Create a program for maximizing the number of prize places in a competition

## Introduction
### Largest Number
- Learning objectives
    + Come up with a greedy algorithm yourself

- Greedy Strategy
    + Find *max* digit
    + **Append** it to the number
    + ***Remove*** it from the list of digits
    + Repeat while there are digits in the list

### Car Fueling
- Car Fueling
    + input : A car which can travel at most ***L*** kilometers with full tank, a source point ***A***, a destination point ***B*** and ***n*** gas stations at distances ${x}\_{1} \le {x}\_{2} \le {x}\_{3} \le {x}\_{4} \le ... \le {x}\_{n}$ kilometers from A along the path from A to B.
    + output : The minimum number of refills to get from A to B, besides refill at A.

- Greedy Strategy
    + Make some greedy choice
        * Refill at the closest gas station
        * Refill at the farthest reachable gas station(V)
        * Go until there is no fuel
    + Reduce to a smaller problem
    + Iterate

- Greedy Algorithm
    + Start at A
    + Refill at tha farthest reachable gas station G
    + Make G the new A
    + Get from new A to B with minimum number of refills

> Definition: Subproblem is a similar problem of smaller size.
- Examples
    + LargestNumber(3, 9, 5, 9, 7, 1) = "9" + LargestNumber(3, 5, 9, 7, 1)
    + Min number of refills from A to B = first refill at G + min number of refills from G to B.

>Definition: A greedy choice is called *safe move* if there is an optimal solution consistent with this first move.

- Lemma
    + To refill at the farthest reachable gas station is a safe move.

- Proof
    + Route R with the minimum number of refils
    + G1 - position of first refill in R
    + G2 - next stop in R (refill or B)
    + G - farthest refill reachable from A
    + If G is closer than G2, refill at G instead of G1.
    + Otherwise, avoid refill at G1/ this case contradicts that the route R.

### Car Fueling - Implementation and Analysis
### Main Ingredients of Greedy Algorithms

- Reduction to subproblem
    + Make a first move
    + Then solve a problem of the same kind
    + Smaller: fewer digits, fewer fuel stations
    + This is called a "subproblem"

- Safe move
    + A move is called **safe** if there is an optimal solution consistent with this first move.
    + Not all first moves are safe.
    + Often greedy moves are not safe.

- General Strategy
    + Make a greedy choice from a problem
    + *Prove* that it is a **Safe move**
    + Reduce to a ***subproblem***


## Grouping Children
### Celebration Party Problem
- Many children came to a celebration. Organize them into the minimum possible number of groups such that the age of any two children in the same group differ by at most one year.

- Running time
    + Lemma : The number of operations in MinGroups(C) is at leat 2^n, where n is the number of children in C.
    + Proof
        * Consider just partitions in two groups
        * C = $G1 \cup G2$
        * For each $G1 \subset C$, G2 = C \ G1
        * Size of C is n
        * Each item can be included or excluded from G1
        * There are 2^n different G1
        * Thus, at least 2^n operations

### Efficient Algorithm for Grouping Children
- Covering points by segments
    + input : A set of n points x1,...,xn $\in R$
    + output : The minimum number of segments of unit length needed to cover all the points.

- Safe move : cover the leftmost point with a unit segment with left end in this point.

### Analysis and Implementation of the Efficient Algorithm
- Lemma
    + The running time of PointsCoverSorted is O(n).

- Proof
    + i changes from 1 to n
    + For each i, at most 1 new segment
    + Overall, running time is O(n)

- Total Running Time
    + PointsCoverSorted works in O(n) time
    + Sort {x1, x2, ... , Xn}, then call PointCoverSorted
    + Soon you'll learn to sort in O(nlogn)

- Asymptotics
    + Straightforward solution is $\Omega(2^n)$
    + Very long for n = 50
    + Sort + greedy is O(nlogn)
    + Fast for n = 10 000 000
    + Huge improvement!

- Conclusion
    + Straightforward solution is exponential
    + Important to reformulate the problem in mathematical terms
    + **Safe move** is to cover leftmost point
    + Sort in O(nlogn) + greedy in O(n)


## Fractional Knapsack
### Long Hike
- Fractional knapsack
    + input : Weights w1,...,wn and values v1,...,vn of n items; capacity W.
    + output : The maximum total value of fractions of items that fit into a bag of capacity W.
    + weights : real weights, values : calories

- Safe move
    + Lemma : There exists an optimal solution that uses as much as possible of an item with the maximal value per unit of weight.
    + Proof : 

- Greedy Algorithm
    + While knapsack is not full
    + Choose item i with maximum vi/wi
    + If item fits into knapsack, take all of it
    + Otherwise take so much as to fill the knapsack
    + Return total value and amounts taken

### Factional Knapsack - Implementation, Analysis and Optimization
- Running time
    + Lemma : The running time of Knapsack is O(n^2)
    + Proof
        * Select best item on each step is O(n)
        * Main loop is executed n times
        * Overall, O(n^2)

- Optimization
    + It is possible to improve asymptotics!
    + Fisrt, sort items by decreasing v/w

- Asymptotics
    + Now each iteration is O(1)
    + Knapsack after sorting is O(n)
    + Sort + knapsack is O(nlogn)

### Review of Greedy Algorithms

- Main Ingredients
    + Safe move
    + Prove safety
    + Solve subproblem
    + Estimate running time

- Safe Moves
    * Put max digit first
    * Find first occurrence of first charater
    * Cover leftmost point
    * Use item with maximum value per unit of weight
> Safe move is greedy, but not all greedy moves are safe.
> So prove that the move everytime.

- Optimization
    + Assume everything is somehow sorted
    + Which sort order is convenient?
    + Greedy move can be faster after sorting

- General Strategy
    + Make a greedy choice from a problem
    + *Prove* that it is a **Safe move**
    + Reduce to a ***subproblem***
    + Solve the ***subproblem***