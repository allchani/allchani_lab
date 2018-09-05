---
title: "[Coursera02] Algorithmic Toolbox week05" 
excerpt: "Dynamic Programming 1"
mathjax : True
---
## Change Problem
### Change Problem

- Change problem
    + Find the minimum number of coins needed to make change.
    + Input: An integer money and positive integers coin1,...,coind.
    + Output: The minimum number of coins with denominations coin1,...,coind that changes money.

```
GreedyChange(money)
Change <-- empty collection of coins
while money > 0:
    coin <-- largest denomination
             that does not exceed money
    add coin to Change
    money <-- money - coin
return Change 
```

```
RecursiveChange(money, coins)
if money = 0:
    return 0
MinNumCoins <-- infinite
for i from 1 to |coins|:
    if money >= coin_i:
        NumCoins <-- RecursiveChange(money-coin_i, coins)
        if NumCoins + 1 < MinNumCoins:
            MinNumCoins <-- NumCoins + 1
return MinNumCoins
```

```
DPChange(money, coins)
MinNumCoins(0) <-- 0
for m from 1 to money:
    MinNumCoins(m) <-- infinite
    for i from 1 to |coins|:
        if m >= coin_i:
            NumCoins <-- MinNumCoins(m-coin_i) + 1
            if NumCoins < MinNumCoins(m):
                MinNumCoins(m) <-- NumCoins
return MinNumCoins(money)
```

## String Comparison
### The Alignment Game
- The Alignment Game
    + A T G T T A T A
    + A T C G T C C
    + Alignment game: remove all symbols from two strings in such a way that the number of points is maximized:
    + Remove the 1st symbol from both strings:
        * 1 point if the symbols match,
        * 0 points if they don't match
    + Remove the 1st symbol from one of the strings:
        * 0 points

- Sequence Alignment
    + A T - G T T A T A
    + A T C G T - C - C
    + Alignment of two strings is a two-row matrix:
        * 1st row: symbols of the 1st string(in order) interspersed by "-"
        * 2nd row: Symbols of the 2nd string(in order) interspersed by "-"
    + insertions: select from 2nd string
    + deletions: select from 1st string

- Alignment Score
    + #matches - $\mu$\*#mismatches - $\sigma$\*indels
    + Optimal alignment
        * input: Two strings, mismatch penalty $\mu$, and indel penalty $\sigma$.
        * output: An alignment of the strings maximizing the score.

- Common Subsequence
    + *Matches* in an alignment of two strings (ATGT) form their common subsequence. 

- Longest common subsequence
    + input: Two strings.
    + output: A longest common subsequence of these strings.
    + Maximizing the length of a common subsequence corresponds to maximizing the score of an alignment with $\mu = \sigma = 0$.

- Edit distance
    + input: Two strings.
    + output: The minimum number of operations(insertions, deletions, and substitutions of symbols) to transform one string into another.
    + The minimum number of insertions, deletions and mismatches in alignment of two strings(among all possible alignments).

> minimizing edit distance = maximizing alignment score

### Computing Edit Distance

A[1...i]  
B[1...j]  

- Given strings A[1...n] and B[1...m], what is an optiaml alignment(an alignment that results in minimum edit distance) of an i-prefix A[1...i]of the first string and a j-prefix B[1...j] of the second string?
- The last column of an optimal alignment is either 
    + an insertion,
    + a deletion,
    + a mismatch,
    + or a match
- What is left(after the removal of the last column) is an optimal alignment of the corresponding two prefixes.
- insertion, deletion, mismatch : +1
- match : +0
- Let D(i,j) be the edit distance of an i-prefix A[1...i] and a j-prefix B[1...j].
- $D(i,j)=min\begin{cases} D(i,j-1)+1 \\ D(i-1,j)+1 \\ D(i-1,j-1)+1\quad if\quad A[i]\neq B[j] \\ D(i-1,j-1)\quad \quad \quad if\quad A[i]=B[j] \end{cases}$
- comparing A[1...n] = EDITING
- and B[1...m] = DISTANCE

```
EditDistance(A[1...n], B[1...m])
D(i,0) <-- i and D(0,j) <-- j for all i,j
for j from 1 to m:
    for i from 1 to n:
        insertion <-- D(i,j-1)+1 (blue)
        deletion <-- D(i-1,j)+1 (green)
        match <-- D(i-1,j-1) (orange)
        mismatch <-- D(i-1,j-1)+1 (purple)
        if A[i]=B[j]:
            D(i,j) <-- min(insertion, deletion, match)
        else:
            D(i,j) <-- min(insertion, deletion, mismatch)
return D(n,m)
```

### Reconstructing on Optimal Alignment

- Optimal Alignment
    + We have computed the edit distance, but how can we find an optimal alignment?
    + The backtracking pointers that we stored will help us to reconstruct an optimal alignment.
    + any path from (0,0) to (i,j) spells an alignment of prefixes A[1...i] and B[1...j]

![editing_distance]({{ site.url }}{{ site.baseurl }}/assets/images/editing_distance.png)
{: .image-center}

![editing_distance02]({{ site.url }}{{ site.baseurl }}/assets/images/editing_distance02.png)
{: .image-center}

![outputAlignment]({{ site.url }}{{ site.baseurl }}/assets/images/outputAlignment.png)
{: .image-center}

![outputAlignment02]({{ site.url }}{{ site.baseurl }}/assets/images/outputAlignment02.png)
{: .image-center}




