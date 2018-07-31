---
title: "[Coursera02] Algorithmic Toolbox week02" 
excerpt: "Algorithmic Warm-up"
mathjax : True
---

## Why Study Algorithms?
### Learning Objectives
- Understand the type of problem that will be covered in this class.
- Recognize some problems for which sophisticated algorithms might not be necessary.
- Describe some artificial intelligence problems which go beyond the scope of this course.

### Simple Programming Problems
- Has linear scan.
- Cannot do much better.
- The obvious program works.

### Algorithms Problems
- Not clear how to do
- Simple ideas too slow
- Room for optimization

### Artificial Intelligence Problems
- Hard to even clearly state.

### What We'll Cover
- Focus on algorithms problems
    + Clearly formulated.
    + Hard to do efficiently

## Fibonacci Numbers
### Problem Overview
- Learning Objectives
    + Understand the definition of the Fibonacci numbers.
    + Show that Fibonacci numbers become very large.

### Naive Algorithm
- Learning Objectives
    + Produce a simple algorithm to compute Fibonacci numbers.
    + Show that this algorithm is very slow.

### Efficient Algorithm
- Learning Objectives
    + Compute Fibonacci numbers efficiently.

### Summary
- Introduced Fibonacci numbers.
- Naive algorithm takes ridiculously long time on small examples.
- Improved algorithm incredibly fast.

> The right algorithm makes all the difference.

## Greatest Common Divisor
- Learning Objectives
    + Define greatest common divisors.
    + Compute greatest common divisors inefficiently.

- GCDs
    + Put fraction a/b in simplest form.
    + Divide numerator and denominator by d, to get(a/d)/(b/d)

- Euclidean Algorithm
    + Lemma
        * Let a' be the remainder when a is divided by b, then
        * gcd(a, b) = gcd(a', b) = gcd(b, a')
    + Proof(sketch)
        * a = a' + bq for some q
        * d divides a and b if and only if it divides a' and b

## Big-O Notation
- Learning Objectives
    + Describe some of the issuse involved with computing the runtime of an actual program.
    + Understand why finding exact runtimes is a problem.

### Computing Runtimes
- To figure out how long this simple program would actually take to run on a real computer, we would also need to know things like:
    + Speed of the computer
    + The System Architecture
    + The Compiler Being Used
    + Details of the Memory Hierarchy

- Problem
    + Figuring out accurate runtime is a huge mess.
    + In practice, you might not even know some of these details.

- Goal 
    + Measure runtime without knowing these details.
    + Get results that work for large inputs.

### Asymptotic Notation
- Idea
    + All of these issues can multiply runtimes by (large) constant.
    + So measure runtime in a way that ignores constant multiples.

- Problem
    + Unfortunately, 1 second, 1 hour, 1 year only differ by constant multiples.

- Solution
    + Consider asymptotic runtimes. How does runtime scale with input size.

> $\log { n } <\sqrt { n } <\quad n\quad <\quad n\log { n } <\quad { n }^{ 2 }\quad <\quad { 2 }^{ n }$

### Big-O Notation
- Learning Objectives
    + Understand the meaning of Big-O notation
    + Describe some of the advantages and disadvantages of using Big-O notation.

- Definition
    + f(n) = O(g(n)) (f is Big-O of g) or
    + $f \le g$ if there exist constants N and c so that for all $n \ge N$, $f(n) \le c*g(n)$
    + **f** is bounded above by *some* constant multiple of g.
        * Example
        * $3n^2 + 5n + 2 = O(n^2)$ since if $n \ge 1$, 
        * $3n^2 + 5n + 2 \le 3n^2 + 5n^2 + 2n^2 = 10n^2$

- Using Big-O
    + We will use big-O notation to report algorithm runtimes. This has several advantages.

- Warning
    + Using Big-O loses important information about constant multiples.
    + Big-O is *only* asymptotic.

### Using Big-O
- Learning Objectives
    + Manupulate expressions involving Big-O and other asymptotic notation.
    + Compute algorithm runtimes in terms of Big-O.

- Common Rules
    + Multiplicative constants can be omitted
        * 7n^3 = O(n^3), n^2/3 = O(n^2)
    + n^a < n^b for 0 < a < b:
        * n = O(n^2), $\sqrt { n }$ = O(n)
    + n^a < b^n (a>0, b>1):
        * n^5 = $O({ \sqrt { 2 }  }^{ n })$, n^100 = O(1.1^n)
    + (log n)^a < n^b (a,b > 0):
        * (log n)^3 = $O(\sqrt { n })$, nlogn = O(n^2)
    + Smaller terms can be omitted:
        * n^2 + n = O(n^2), 2^n + n^9 = O(2^n)

- Big-O in Practice

| Operation                 | Runtime | 
|:-------------------------:|:-------:|
| create an array F[0...n]  |O(n)     |
| F[0] <-- 0                |O(1)     |
| F[1] <-- 1                |O(1)     |
| for i from 2 to n: |Loop O(n) times |
| F[i] <-- F[i-1] + F[i-2]  |O(n)     |
| return F[n]               |O(1)     |

Total:
O(n)+O(1)+O(1)+O(n)*O(n)+O(1) = O(n^2)

Big O really just says that my runtime is sort of bounded above by some multiple of this thing.

- Other notation
![Big_O_other]({{ site.url }}{{ site.baseurl }}/assets/images/bigo_other.png)
{: .image-center}

![Big_O_other2]({{ site.url }}{{ site.baseurl }}/assets/images/bigo_other2.png)
{: .image-center}

- Asymptotic Notation
    + Lets us ignore messy details in analysis.
    + Produces clean answers.
    + Throws away a lot of practically useful information.


## Course Overview

- Algorithm Design is Hard
    + Algprithms very general
    + No generic procedure for designing good algorithms.
    + Finding good algorithms often requires coming up with unique insights.


### Toolbox

- What can we teach you? 
    + Practice designing algorithms.
    + Common tools used in algorithm design.
    + We will discuss three of the most common algorithmic design techniques:
        * Greedy Algorithms
        * Divide and Conquer
        * Dynamic Programming

### Levels of Design

- Naive Algorithm : Definition to algorithm. Slow.
- Algorithm by way of standard Tools:
    + Standard techniques
- Optimized Algorithm : Improve existing algorithm.
- Magic Algorithm : Unique insight
  

- The Rest of the Course
    + Each unit covers a technique.
    + Exercises help build intuition.