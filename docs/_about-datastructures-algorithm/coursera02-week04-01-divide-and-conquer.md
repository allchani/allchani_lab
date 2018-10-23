---
title: "[Coursera02] Algorithmic Toolbox week04" 
excerpt: "Divide-and-Conquer"
mathjax : True
---
- Learning Objectives
    + Express the recurrence relation on the runnig time of an algorithm
    + Create a program for searching huge lists
    + Create a program for finding a majority element
    + Create a program for organizing a lottery


## Introduction
### Intro
- Main idea of divide-and-conquer
    + Divide : Break into non-overlapping subproblems of the same type.
    + Solve subproblems
    + Combine results
### Linear Search
- Searching in an array
    + input : An array A with n elements. A key k.
    + output : "An"(not the, it can be more than one) index, i, where A[i]=k. If there is no such i, then NOT_FOUND.

- Definition
    + A recurrence relation is an equation recursively defining a sequence of values.

- Summary
    + Create a recursive solution
    + Define a corresponding recurrence relation, T
    + Determine T(n):worst-case runtime
    + Optionally, create iterative solution

### Binary Search
- Searching in a sorted array
    + input : A sorted array A\[low...high\]
        * $\forall low <= i < high: A[i] =< A[i+1])$.
        * A key *k*
        - this first element is no more than the next element.
            + We don't say less than because we want to allow for arrays that have repeated elements. Officially this is called a monotonic non-decreasing array.
    * output : An index, i, (low =< i =< high) where A[i] = k
        - Otherwise, the greatest index i, where A[i] < k
        - Otherwise (k < A[low]), the result is low-1
    

~~~
BinarySearch(A, low, high, key)
    if high < low:
        return
    mid <-- [low + (high-low)/2]
    if key = A[mid]:
        return mid
    else if key < A[mid]:
        return BinarySearch(A, low, mid-1, key)
    else:
        return BinarySearch(A, mid+1, high, key)
~~~

- Summary
    + Break problem into non-overlapping subproblems of the same type.
    + Recursively solve those subproblems.
    + Combine results of subproblems.

### Binary Search Runtime
- Binary Search Recurrence Relation
    + T(n) = T([n/2]) + c 
    + T(0) = c
- Runtime of Binary Search
    + n, n/2, n/4,..., 2, 1, 0
    + It's going to take log base two such iterations until we get down to 1.
    + So the total here, is actually log base two of n+1.
    + The amount of work we're doing is c.
    + Total : $\sum \_{ i=0 }^{ \log \_{ 2 }{ n }  }{ c } =\Theta (\log \_{ 2 }{ n } )$
- Iterative Version
~~~
BinarySearchIt(A, low, high, key)
while low <= high:
    mid <-- [low + (high-low)/2 ]
    if key = A[mid]:
        return mid
    else if key < A[mid]:
        high = mid - 1
    else:
        low = mid + 1
return low - 1
~~~

- Sorted array
    + good part : it's easy to find a particular word in a particular language.
    + The problem is I no longer have this correspondence, because the order of the words that are sorted in English is different from the order of the words sorted in Spanish.
        + Solution: augmented set of arrays : pointers back into the original arrays.

- Summary
    + The runtime of binary search is $\Theta (\log{ n })$

## Polynomial Multiplication
### Problem Overview and Naive Solution
- Uses of multiplying polynomials
    + Error-correcting codes
    + Large-integer multiplication
    + Generationg functions
    + Convolution in signal processing

![Multiplying polynimials]({{ site.url }}{{ site.baseurl }}/assets/images/multi_poly.png)
{: .image-center}

- Naive Algorithm
~~~
MultPoly(A, B, n)
product <-- Array[2n-1]
for i from 0 to 2n-2:
    product[i] <-- 0
for i from 0 to n-1:
    for j from 0 to n-1:
        product[i+j] <-- product[i+j] + A[i]*B[j]
return product
~~~
Runtime : O(n^2)

### Naive Divide and Conquer Algorithm
![Multiplying polynimials]({{ site.url }}{{ site.baseurl }}/assets/images/mp.png)
{: .image-center}

![Theta value]({{ site.url }}{{ site.baseurl }}/assets/images/thetavalue.png)
{: .image-center}

### Faster Divide and Conquer Algorithm
- Karatsuba approach
A(x) = a1x + a0
B(x) = b1x + b0
C(x) = a1b1x^2 + (a1b0 + a0b1)x + a0b0
Needs 4 multiplications

Rewrite as:
C(x) = a1b1x^2 + ((a1+a0)(b1+b0) - a1b1 - a0b0)x + a0b0
Needs 3 multiplications

![Fast Theta value]({{ site.url }}{{ site.baseurl }}/assets/images/fastthetavalue.png)
{: .image-center}

## Master Theorem
### What is the Master Theorem?

- Theorem
If T(n) = aT([n/b]) + O(n^d) (for constants a>0, b>1, d>=0), then:

$$T(n)\quad =\quad \begin{cases} O({ n }^{ d })\quad \quad \quad \quad \quad \quad \quad if\quad d>\log\_{ b }{ a }  \\ O({ n }^{ d }\log { n } )\quad \quad \quad if\quad d=\log\_{ b }{ a }  \\ O({ n }^{ \log\_{ b }{ a }  })\quad \quad \quad \quad \quad if\quad d<\log\_{ b }{ a }  \end{cases}$$

### Proof of the Master Theorem?



## Sorting Problem
### Problem Overview
- Sorting
    + Input: Sequence A[1...n]
    + Output: Permutation A'[1...n] of A[1...n] in non-decreasing order

- Why Sorting?
    + Sorting data is an important step of many efficient algorithms
    + Sorted data allows for more efficient queries.

### Selection Sort
- Selection sort: example
    + Find a minimum by scanning the array
    + Swap it with the first element
    + Repeat with the remaining part of the array

```
SelectionSort(A[1...n])
for i from 1 to n:
    minIndex <-- i
    for j from i+1 to n:
        if A[j]<A[minIndex]:
            minIndex <-- j
    {A[minIndex] = min A[i...n]}
    swap(A[i], A[minIndex])
    {A[1...i] is in final position}
```

- Lemma
    + The running time of SelectionSort(A[1...n]) is O(n^2).
- Proof
    + n iterations of outer loop, at most n iterations of inner loop.

- Too Pessimistic Estimate?
    + As i grows, the number of iterations of the inner loop decreases: j iterates from i+1 to n
    + A more accurate estimate for the total number of iterations of the inner loop is (n-1) + (n-2) + ... + 1.
    + We will show that this sum is $Theta(n^2)$ implying that our initial estimate is actually tight.

- Selection Sort: Summary
    + Selection sort is an easy to implement algorithm with running time O(n^2).
    + Sorts in place: requires a constant amount of extra memory.
    + There are many other quadratic time sorting algorithms: e.g., insertion sort, bubble sort.

### Merge Sort
```
MergeSort(A[1...n])
if n=1:
    return A
m <-- [n/2]
B <-- MergeSort(A[1...m])
C <-- MergeSort(A[m+1...n])
A' <-- Merge(B,C)
return A'
```

```
Merge(B[1...p], C[1...q])
{B and C are sorted}
D <-- empty array of size p+q
while B and C are both non-empty:
    b <-- the first element of B
    c <-- the first element of C
    if b <= c:
        move b from B to the end of D
    else:
        move c from C to the end of D
move the rest of B and C to the end of D
return D
```

- Lemma
    + The running time of MergeSort(A[1...n]) is O(nlogn).

- Proof
    + The running time of merging B and C is O(n).
    + Hence the running time of MergeSort(A[1...n]) satisfies a recurrence T(n) <= 2T(n/2) + O(n).

### Lower Bound for Comparison Based Sorting
- Definition
    + A comparison based sorting algorithm sorts objects by comparing pairs of them.
    + example: Selection sort and merge sort are comparison based.

- Lemma
    + Any comparison based sorting algorithm performs $Omega(nlogn)$ comparisons in the worst case to sort n objects.
    + In other words: For any comparison based sorting algorithm, there exists an array A[1...n] such that the algorithm performs at least $Omega(nlogn)$ comparisons to sort A.

- Estimating Tree Depth
    + the number of leaves 'l' in the tree must be at least n! (the total number of permutations)
    + the worst-case running time of the algorithm (the number of comparisons made) is at least the depth d.
    + d >= $\log \_{2}{n}$ (or, equivalently, ${2}^{d} \ge l$)
    + Thus, the running time is at least
    + $$\log\_{2}{n} = \Omega(nlog{n})$$

- Lemma 
    + $\log \_{2}{n!} = \Omega(n\log{n})$
- Proof
    + $\log \_{2}{n!} = \log _{2}{1\*2\*\*\*\*n}$
    + $= \log \_{2}{1} + \log \_{2}{2} + ... \log \_{2}{n}$
    + $\ge \log \_{2}{\frac {n}{2}}+...+\log \_{2}{n}$
    + $\ge \frac {n}{2}\log _{2}{\frac {n}{2}} = \Omega(nlog{n})$

This concludes the proof of the fact that any comparison based algoritm must make at least n log n operations in the worst case. Once again, another conclusion is that when merged sort algorithms that we considered in the previous lecture, is asymmetrically optimal.

### Non-Comparison Based Sorting Algorithms

- Counting Sort: Ideas
    + Assume that all elements of A[1...n] are integers from 1 to M.
    + By a single scan of the array A, count the number of occurrences of each $1 \le k \le M$ in the array A and store it in Count[k]
    + Using this information, fill in the sorted array A'.

```
CountSort(A[1...n])
count[1...M] <-- [0,....,0]
for i from 1 to n:
    Count[A[i]] <-- Count[A[i]] + 1
{k appears Count[k] times in A}
Pos[1...M] <-- [0,...,0]
Pos[1] <-- 1
for j from 2 to M:
    Pos[j] <-- Pos[j-1] + Count[j-1]
{k will occupy range [Pos[k]...Pos[k+1]-1]}
for i from 1 to n:
    A'[Pos[A[i]]] <-- A[i]
    Pos[A[i]] <-- Pos[A[i]] + 1
```

- Lemma
    + Provided that all elements of A[1...n] are integers from 1 to M, CountSort(A) sorts A in time O(n + M).
- Remark
    + If M = O(n), then the running time is O(n).

- Summary
    + Merge sort uses the divide-and-conquer strategy to sort an n-element array in time O(nlogn).
    + No comparison based algorithm can do this (asymptotically) faster.
    + One can do faster if something is known about the input array in advance (e.g., it contains small integers).

## Quick Sort
### Overview
- Quick Sort
    + comparison based algorithm
    + running time: O(nlogn) (on average)
    + efficient in practice

### Algorithm
```
QuickSort(A, l, r)
if l>=r:
    return
m <-- Partition(A,l,r)
{A[m] is in the final position}
QuickSort(A,l,m-1)
QuickSort(A,m+1,r)
```

![Quick Sort]({{ site.url }}{{ site.baseurl }}/assets/images/quicksort.png)
{: .image-center}

```
Partition(A,l,r)
x <-- A[l] {pivot}
j <-- l
for i from l+1 to r:
    if A[i] <= x:
        j <-- j+1
        swap A[j] and A[i]
    {A[l+1...j]<=x, A[j+1...i]>x}
swap A[l] and A[j]
return j
```

### Random Pivot
- Unbalanced Partitions
    + T(n)=n + T(n-1): T(n)=n + (n-1) + (n-2) + ...=$Theta(n^{2})$
    + T(n)=n + T(n-5) + T(4): T(n) $\ge n+(n-5)+(n-10)+...=Theta(n^{2})$

- Balaced Partitions
    + T(n) = 2T(n/2) + n:
        * T(n) = $Theta(nlog{n})$
> Running time of Algorithm of quick sorts algorithm depends on how balanced our partitions.
> balanced = O(nlogn), unbalanced = O(n^2)

```
RandomizedQuickSort(A,l,r)
if l >= r:
    return
k <-- random number between l and r
swap A[l] and A[k]
m <-- Partition(A,l,r)
{A[m] is in the final position}
RandomizedQuickSort(A,l,m-1)
RandomizedQuickSort(A,m+1,r)
```

- Theorem
    + Assume that all the elements of A[1...n] are pairwise different. Then the average running time of RandomizedQuickSort(A) is O(nlogn) while the worst case running time is O(n^2).
    + Remark: Averaging is over random numbers used by the algorithm, but not over the inputs.

### Running Time Analysis
- Proof Ideas: Comparisons
    + the running time is proportional to the number of comparisons made.
    + balanced partition are better since they reduce the number of comparisons needed:

- Proof Ideas: Probability
   + A (5, 1, 8, 9, 2, 4, 7, 3, 6)
   + A'(1, 2, 3, 4, 5, 6, 7, 8, 9)
   + Prob(1 and 9 are compared) = 2/9
   + Prob(3 and 4 are compared) = 1

- Proof
    + let, for i < j,
    + ${ x }\_{ ij }\begin{cases} 1\quad A'[i]\quad and\quad A'[j]\quad are\quad compared \\ 0\quad otherwise \end{cases}$
    + for all i < j, A'[i] and A'[j] are either compared exactly once or not compared at all (as we compare with a pivot)
    + this, in particular, implies that the worst case running time is O(n^2)
    + **crucial observation**: $x\_{ij}$=1 if the first selected pivot in A'[i...j] is A'[i] or A'[j]
    + then $Prob({x}\_{ij})=\frac{2}{j-i+1}$ and $E({x}\_{ij})=\frac {2}{j-i+1}$
    + Then (the expected value of) the running time is
$$E\sum \_{ i=1 }^{ n }{ \sum \_{ j=i+1 }^{ n }{ { x }\_{ ij } }  } =\sum \_{ i=1 }^{ n }{ \sum \_{ j=i+1 }^{ n }{ E({ x }\_{ ij }) }  } =\quad \sum \_{ i<j }^{  }{ \frac { 2 }{ j-i+1 }  } \le \quad 2n*(\frac { 1 }{ 2 } +\frac { 1 }{ 3 } +...+\frac { 1 }{ n } )\quad =\quad \Theta (n\log { n } )$$

### Equal Elements
- Equal Elements
    + what if all the elements of the given array are equal to each other?
    + the array is always split into two parts of size 0 and n-1
    + T(n)=n+T(n-1)+T(0) and hence T(n)=$Theta(n^{2})$
    + To handle equal elements, we replace the line
        * m <-- Partition(A,l,r)
    + with the line
        * (m1,m2) <-- Partition3(A,l,r)
    + such that
        * for all $l \le k \le m1 -1, A[k] < x$
        * for all $m1 \le k \le m2, A[k] = x$
        * for all $m1 +1 \le k \le r, A[k] > x$
```
RandomizedQuickSort(A,l,r)
if l >= r:
    return
k <-- random number between l and r
swap A[l] and A[k]
(m1, m2) <-- Partition3(A,l,r)
{A[m1...m2] is in final position}
RandomizedQuickSort(A,l,m1-1)
RandomizedQuickSort(A,m2+1,r)\
```

### Final Remarks
- Tail Recursion Elimination
```
QuickSort(A,l,r)
while l < r:
    m <-- Partition(A,l,r)
    QuickSort(A,l,m-1)
    l <-- m+1
```

- which tail we would eliminate...
```
QuickSort(A,l,r)
while l < r:
    m <-- Partition(A,l,r)
    if (m-l) < (r-m):
        QuickSort(A,l,m-1)
        l <-- m+1
    else:
        QuickSort(A,m+1,r)
        r <-- m-1
```

- Intro Sort
    + runs quick sort with a simple deterministic pivot selection heuristic(say, median of the first, middle, and last element)
    + if the recursion depth exceeds a certain threshold c log n the algorithm switches to heap sort
    + the running time is O(nlogn) in the worst case

- Conclusion
    + Quick sort is a comparison based algorithm
    + Running time: O(n log n) on average, O(n^2) in the worst case
    + Efficient in practice

