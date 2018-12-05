---
title: "[Coursera03] Data Structures week04" 
excerpt: "Hash Tables"
mathjax : True
---
## Hash Tables
### Introduction, Direct Addressing and Chaining
#### Applications of Hashing
- Programming Languages
    + python : dict
    + Java : HashMap
- File Systems
- Password Verification
- Storage Optimization

#### Analysing Service Access Logs
- IP Access List
    + Analyse the access log and quickly answer queries: did anybody access the service from this IP during the last hour? How many times? How many IPs were used to access the service during the last hour?
- Log Processing
    + 1h of logs can contain millions of lines
    + Too slow to process that for each query
    + Keep count: how many times each IP appears in the last 1h of the access log
    + C is some data structure to store the mapping from IPs to counters

![log processing]({{ site.url }}{{ site.baseurl }}/assets/images/log_processing.png)
{: .image-center}

```
Main Loop:
log - array of log lines (time, IP)
C - mapping from IPs to counters
i - first unprocessed log line
j - first line in current 1h window
i = 0
j = 0
C = none
Each second
    UpdateAccessList(log,i,j,C)
```

```
UpdateAccessList(log,i,j,C):
while log[i].time <= Now():
    C[log[i].IP] <-- C[log[i].IP] + 1
    i <-- i + 1
while log[j].time <= Now() - 3600:
    C[log[j].IP] <-- C[log[j].IP] - 1
    j <-- j + 1

AccessedLastHour(IP, C):
return C[IP] > 0
```

#### Direct Addressing

- Need a data structure for C
- There are 2^32 different IP(v4) addresses
- Convert IP to 32-bit integer
- Create an integer array A of size 2^32
- Use A[int(IP)] as C[IP]

- int(IP)
    + An IPv4 address (dotted-decimal notation)
    + 172.16.254.1
    + 10101100.00010000.11111110.00000001

```
int(IP):
return IP[1]*2^24 + IP[2]*2^16 + IP[3]*2^8 + IP[4]
```

- Asymptotics
    + UpdateAccessList is O(1) per long line
    + AccessedLastHour is O(1)
    + But need 2^32 memory even for few IPs
    + IPv6: 2^128 won't fit in memory
    + In general: O(N) memory, N=\|S\|, where N is the size of our universe
    + this method only works when the universe is somewhat small!

#### List-based Mapping

- Direct addressing requires too much memory
    + It tries to store something for each possible IP address while we're only interested in the active IP addresses. Those from which at least some user has accessed our service during the last hour.

```
UpdateAccessList(log, i, L):
while log[i].time <= Now():
    L.Append(log[i])
    i <-- i + 1
while L.Top().time <= Now() - 3600:
    L.Pop()
```

```
AccessedLastHour(IP, L):
return L.FindByIP(IP) != NULL

AccessCountLastHour(IP, L):
return L.CountIP(IP)
```

- Asymptotics
    + n is number of active IPs
    + Memory usage is O(n)
    + L.Append, L.Top, L.Pop are O(1)
    + UpdateAccessList is O(1) per log line
    + L.FindByIP and L.CountIP are O(n)
    + AccessedLastHour and AccessCountLastHour are O(n)

#### Hash Functions

- Encoding IPs
    + Encode IPs with small numbers
    + i.e numbers from 0 to 999
    + Different codes for currently active IPs

- Hash Function
- Definition
    + For any set of objects S and any integer m > 0, a function h : S -> {0,1,...,m-1} is called a hash function.
- Definition
    + m is called the cardinality of hash function h.

- Desirable Properties
    + h should be fast to compute
    + Different values for different objects
    + Direct addressing with O(m) memory
    + Want small cardinality m
    + Impossible to have all different values if number of objects \|S\| is more than m.

- Collisions
- Definition
    + When h(o_1) = h(o_2) and o_1 != o_2, this is a collision.

#### Chaining Scheme
- Map
    + Store mapping from objects to other objects:
        * Filename -> location of the disk
        * Student ID -> student name
        * COntact name -> contact phone number
- Definition
    + Map from S to V is a data structure with methods HasKey(O), Get(O), Set(O, v), where O in S, v in V.

![Chaining]({{ site.url }}{{ site.baseurl }}/assets/images/chaining.png)
{: .image-center}

#### Chaining Implementation and Analysis

- h : S -> {0,1,...,m-1}
- O, O' $\in$ S
- v, v' $\in$ V
- A <- array of m lists(chains) of pairs(O,v)

```
HasKey(O):
L <-- A[h(O)]
for (O', v') in L:
    if O' == O:
        return true
return false
```

```
Get(O):
L <-- A[h(O)]
for (O', v') in L:
    if O' == O:
        return v'
return n/a
```

```
Set(O, v):
L <-- A[h(O)]
for p in L: 
    if p.O == O:
        p.v <-- v
        return
L.Append(O,v)
```

- Lemma
    + Let c be the length of the longest chain in A. Then the running time of HasKey, Get, Set is $\Theta(c + 1)$
- Proof
    + If L = A[h(O)], len(L) = c, $O \notin L$, need to scan all c items.
    + If c = 0, we still need O(1) time

- Lemma
    + Let n be the number of different keys O currently in the map and m be the cardinality of the hash function. The the memory consumption for chaining is $\Theta(n+m)$.
- Proof
    + $\Theta(n)$ to store n pairs(O,v)
    + $\Theta(m)$ to store array A of size m

#### Hash Tables

- Set
- Definition
    + Set is a data structure with methods Add(O), Remove(O), Find(O).
- Examples
    + IPs accessed during last hour
    + Students on campus
    + Keywords in a programming language

- Implementing Set
- Two ways to implement a set using chaining:
    + Set is equivalent to map from S to 
    + V = {true, false}
    + Store just objects O instead of pairs (O,v) in chains

- h : S -> {0,1,...,m-1}
- O, O' $\in$ S
- A <- array of m lists(chains) of objects O

```
Find(O):
L <-- A[h(O)]
for O' in L:
    if O' == O:
        return true
return false
```

```
Add(O):
L <- A[h(O)]
for O' in L:
    if O' == O:
        return
L.Append(O)
```

```
Remove(O):
if not Find(O):
    return
L <-- A[h(O)]
L.Erase(O)
```

- Hash Table
- Definition
    + An implementation of a set or a map using hashing is called a hash table.

- Programming Languages
    + Set:
    + unordered_set in C++
    + HashSet in Java
    + set in Python
    + Map:
    + unordered_map in C++
    + HashMap in Java
    + dict in Python

- Conclusion
    + Chaining is a technique to implement a hash table
    + Memory consumption is O(n+m)
    + Operations work in time O(c+1)
    + How to make both m and c small?

### Hash Functions
#### Phone Book Problem
- Phone Book
    + Design a data structure to store your contacts: names of people along with their phone numbers. The data structure should be able to do the following quickly:
        * Add and delete contacts,
        * Lookup the phone number by name,
        * Determine who is calling given their phone number.

- We need two Maps:
- (phone number --> name) and
- (name --> phone number)
- Implement these Maps as hash tables
- First, we will focus on the Map from phone numbers to names

- Direct Addressing
    + int(123-45-67) = 1234567
    + Create array Name of size 10^L where L is the maximum allowed phone number length
    + Store the name corresponding to phone number P in Name[int(P)]
    + If no contact with phone number P, Name[int(P)] = N/A

- Direct Addressing
    + Operations run in O(1)
    + Memory usage: O(10^L), where L is the maximum length of a phone number
    + Problematic with international numbers of length 12 and more: we will need 10^12 bytes = 1TB to store one person's phone book - this won't fit in anyone's phone!

- Chaining 
    + Select hash function h with cardinality m
    + Create array Name of size m
    + Store chains in each cell of the array Name
    + Chain Name[h(int(P))] contains the name for phone number P

- Parameters
    + n phone numbers stored
    + m - cardinality of the hash function
    + c - length of the longest chain
    + O(n+m) memory is used
    + alpha = n/m is called **load factor**
    + Operations run in time O(c+1)
    + You want small m and c!

![Chaining_good example]({{ site.url }}{{ site.baseurl }}/assets/images/hash_good.png)
{: .image-center}

![Chaining_bad example]({{ site.url }}{{ site.baseurl }}/assets/images/hash_bad.png)
{: .image-center}

- First Digits
    + For the map from phone numbers to names, select m=1000
    + Hash function: take first three digits
    + h(800-123-45-67) = 800
    + **Problem: area code**
    + h(425-234-55-67) =
    + h(425-123-45-67) =
    + h(425-223-23-23) = ... = 425

- Last Digits
    + Select m = 1000
    + Hash function: take last three digits
    + h(800-123-45-67) = 567
    + **Problem if many phone numbers end with three zeros**

- Random Value
    + Select m = 1000
    + Hash function: random number between 0 and 999
    + Uniform distribution of hash values
    + Different value when hash function called again - **we won't be able to find anything!**
    + Hash function must be deterministic

- Good Hash Functions
    + Deterministic
    + Fast to compute
    + Distributes keys well into different cells
    + Few collisions

- No Universal Hash Function
    + Lemma: If number of possible keys is big(\|U\| >> m), for any hash function h there is bad input resulting in many collisions.

#### Universal Family
- Idea
    + Remember QuickSort?
    + Choosing random pivot helped
    + Use randomization!
    + Define a family(set) of hash functions
    + Choose random function from the family

- Universal Family
- Definition:
    + Let U be the **universe** - the set of all possible keys. A set of hash functions
    + cali_H = {h: U --> {0,1,2,...,m-1}}
    + is called a universal family if for any two keys x,y $\in$ U, x != y the probability of **collision**
    + Pr[h(x)=h(y)] <= 1/m

- Pr[h(x)=h(y)] <= 1/m
- means that a collision h(x) = h(y) on selected keys x and y, x != y happens for no more than 1/m of all hash functions h $\in$ cali_H.

- How Randomization Works
    + h(x) = random({0,1,2,...,m-1}) gives probability of collision exactly 1/m.
    + It is not deterministic -- can't use it.
    + All hash functions in cali_H are deterministic
    + Select a random function h from cali_H
    + Fixed h is used throughout the algorithm

- Running Time
- Lemma
    + If h is chosen randomly from a **universal family**, the average length of the longest chain c is O(1+alpha), where alpha = n/m is the **load factor** of the hash table.
- Corollary
    + If h is from **universal family**, operations with hash table run on average in time O(1+alpha)

- Choosing Hash Table Size
    + Control amount of memory used with m
    + Ideally, load factor 0.5 < alpha < 1
    + Use O(m) = O(n/alpha) = O(n) memory to store n keys
    + Operations run in time
    + O(1+alpha) = O(1) on average

- Dynamic Hash Tables
    + What if number of keys n is unknown in advance?
    + Start with very big hash table?
    + You will waste a lot of memory
    + Copy the idea of dynamic arrays!
    + Resize the hash table when alpha becomes too large
    + Choose new hash function and **rehash** all the objects

- Keep **load factor** below 0.9:
```
Rehash(T):
loadFactor <-- T.numberOfKeys/T.size
if loadFactor > 0.9:
    Create T_new of size 2*T.size
    Choose h_new with cardinality T_new.size
    For each object O in T:
        Insert O in T_new using h_new
    T <-- T_new, h <-- h_new
```

- Rehash Running Time
    * You should call Rehash after each operation with the hash table
    * Similarly to dynamic arrays, single rehashing takes O(n) time, but amortized running time of each operation with hash table is still O(1) on average, because rehashing will be rare.

#### Hashing Integers
- Take phone numbers up to length 7, for example 148-25-67
- Convert phone numbers to integers from 
- 0 to 10^7 -1 = 9 999 999:
- 148-25-67 --> 1 482 567
- Choose prime number bigger than 10^7.
- e.g. p = 10 000 019
- Choose hash table size, e.g. m=1000

- Hashing Integers
- Lemma:
    + cali_H_p = {h_{p}{a,b}(x) = ((ax+b) mod p) mod m}
    + for all a,b: 1<=a<=p-1, 0<=b<=p-1
    + is a **universal family**

- Hashing Phone Numbers
- Example:
- Select a = 34, b = 2, so h = h_{p}{34,2} and consider x = 1 482 567 corresponding to phone number 148-25-67. p = 10 000 019.
- (34 x 1482567 + 2) mod 10000019 = 407185
- 407185 mod 1000 = 185
- h(x) = 185

- General Case
    + Define maximum length L of a phone number
    + Convert phone numbers to integers from 0 to 10^L-1
    + Choose prime number p > 10^L
    + Choose hash table size m
    + Choose random hash function from **universal family** cali_H_p(choose random a $\in$ [1, p-1] and b $\in$ [0, p-1])

#### Proof: Upper Bound for Chain Length
#### Proof: Universal Family for Integers

#### Hashing Strings - Cardinality Fix
- Lookup Phone Numbers by Name
    + Now we need to implement the Map from names to phone numbers
    + Can also use chaining
    + Need a hash function defined on names
    + Hash arbitary strings of characters
    + You will learn how string hashing is implemented in Java!

- String Length Notation
- Definition
    + Denote by \|S\| the length of string S
- Examples
    + \|"a"\| = 1
    + \|"ab"\| = 2
    + \|"abcde"\| = 5

- Hashing Strings
    + Given a string  S, compute its hash value
    + S =S[0]S[1]...S[\|S\|-1], where S[i] - individual characters
    + We should use all the characters in the hash function
    + Otherwise there will be many collisions:
    + For example, if S[0] is not used, h("aa") = h("ba") = ... = h("za")

- Preparation
    + Convert each character S[i] to integer code
    + ASCII code, Unicode, etc.
    + Choose big prime number p

![Polynomial Hashing]({{ site.url }}{{ site.baseurl }}/assets/images/polynomial_hashing.png)
{: .image-center}

```
PolyHash(S,p,x):
hash <-- 0
for i from |S|-1 down to 0:
    hash <-- (hash*x + S[i]) mod p
return hash
```

- Example: \|S\|=3
    + hash = 0
    + hash = S[2] mod p
    + hash = S[1] + S[2]x mod p
    + hash = S[0] + S[1]x + S[2]x^2 mod p

- Java Implementation
    + The method hashCode of the built-in Java class String is very similar to our PolyHash, it just uses x = 31 and for technical reasons avoids the (mod p) operator.
    + You now know how a function that is used trillions of times a day in many thousands of programs is implemented!

- Lemma
    + For any two different strings s1 and s2 of length at most L+1, if you choose h from P_p at random (by selecting a random x $\in$ [1, p-1]), the probability of collision Pr[h(s1)=h(s2)] is at most L/p.
- Proof idea
    + This follows from the fact that the equation a0+a1x+a2x^2+...+a_Lx^L=0 (mod p) for prime p has at most L different solutions x.

- Cardinality Fix
    + For use in a hash table of size m, we need a hash function of cardinality m.
    + First apply random h from P_p and then hash the resulting value again using integer hashing. Denote the resulting function by h_m.

- Lemma:
    + For any two different strings s_1 and s_2 of length at most L+1 and cardinality m, the probability of collision Pr[h_m(s_1) = h_m(s_2)] is at most 1/m + L/p

- Polynomial Hashing
- Corollary
    + If p > mL, for any two different strings s_1 and s_2 of length at most L+1 the probability of collision Pr[h_m(s_1) = h_m(s_2)] is O(1/m).
- Proof
    + 1/m + L/p < 1/m + L/mL = 1/m + 1/m = 2/m = O(1/m)

- Running time
    + For big enough p again have c = O(1+alpha)
    + Computing PolyHash(S) runs in time O(\|S\|)
    + If lengths of the names in the phone book are bounded by constant L, computing h(S) takes O(L) = O(1) time

- Conclusion
    + You learned how to hash integers and strings
    + Phone book can be implemented as two hash tables
    + Mapping phone numbers to names and back
    + Search and modification run on average in O(1)!

### Searching Patterns
#### Search Pattern in Text

- Searching for Patterns
    + Given a text T(book, website, facebook profile) and a pattern P(word, pharase, sentence), find all occurrences of P in T.
- Examples
    + Your name on a website
    + Twitter messages about your company
    + Detect files infected by virus - code patterns

- Substring Notation
- Definition
    + Denote by *S[i..j]* the substring of string *S* starting in position *i* and ending in position *j*.
- Examples
    + If S = "abcde", then
    + S[0..4] = "abcde",
    + S[1..3] = "bcd"
    + S[2..2] = "c"

- Find Pattern in Text
    + Input: Strings T and P
    + Output : 
    + All such position *i* in *T*,
    + 0 <= *i* <= \|T\|-\|P\| that
    + T[i..i + \|P\|-1] = P

- Naive Algorithm
    + For each position *i* from 0 to \|T\| - \|P\|, check character-by-character whether T[i..i + \|P\| - 1] = P or not
    + If yes, append *i* to the result.

```
AreEqual(S_1,S_2)
if |S_1| != |S_2|:
    return False
for i from 0 to |S_1|-1:
    if S_1[i] != S_2[i]:
        return False
return True
```

```
FindPatternNaive(T,P):
result <-- empth list
for i from 0 to |T|-|P|:
    if AreEqual(T[i..i+|P|-1], P):
        result.Append(i)
return result
```

Running Time
- Lemma
    + Running time of FindPatternNaive(T,P) is O(\|T\|\|P\|).
- Proof
    + Each AreEqual call is O(\|P\|)
    + \|T\|-\|P\|+1 calls of AreEqual total to O((\|T\|-\|P\|+1)\|P\|) = O(\|T\|\|P\|)

- Bad Example
    + If T="aaa...aa" and P="aaa...ab", and \|T\|>>\|P\|, then for each position *i* in *T* from 0 to \|T\|-\|P\| the call to AreEqual has to make all \|P\| comparisons.
    + This is because T[i..i+\|P\|-1] and P differ only in the last character.
    + Thus, in this case the naive algorithm runs in time $\Theta(\|T\|\|P\|)$.

#### Rabin-Karp's Algorithm

- Need to compare P with all substrings S of T of length \|P\|
- Idea: use hashing to quickly compare P with substrings of T
  
- If h(P) != h(S), then definitely P != S
- If h(P) = h(S), call AreEqual(P,S)
- Use polynomial hash family P_p with prime p
- If P != S, the probability Pr[h(P)=h(S)] is at most \|P\|/p for polynomial hashing

```
RabinKarp(T,P):
p <-- big prime, x <-- random(1, p-1)
result <-- empty list
pHash <-- PolyHash(P,p,x)
for i from 0 to |T|-|P|:
    tHash <-- PolyHash(T[i..i+|P|-1],p,x)
    if pHash != tHash:
        continue
    if AreEqual(T[i..i+|P|-1], P):
        result.Append(i)
return result
```

- False Alarms
    + "False alarm" is the event when P is compared with T[i..i+|P|-1], but P != T[i..i+|P|-1]
    + The probability of "false alarm" is at most \|P\|/p
    + On average, the total number of "false alarms" will be (\|T\|-\|P\|+1)\|P\|/p, which can be made small by selecting p >> \|T\|\|P\|

- Running Time without AreEqual
    + h(P) is computed in O(\|P\|)
    + h(T[i..i+\|P\|-1]) is computed in O(\|P\|), \|T\|-\|P\|+1 times
    + O(\|P\|)+O((\|T\|-\|P\|+1)\|P\|) = O(\|T\|\|P\|)

- AreEqual Running Time
    + AreEqual is computed in O(\|P\|)
    + AreEqual is called only when h(P) = h(T[i..i+|P|-1]), meaning that either an occurence of P is found or a "false alarm" happened.
    + By selecting p >> \|T\|\|P\| we make the number of "false alarms" negligible

- Total Running Time
    + If P is found q times in T, then total time spent in AreEqual is O((q+(\|T\|-\|P\|+1)*\|P\|/p)\|P\|) = O(q\|P\|) for p >> \|T\|\|P\|
    + Total running time is O(\|T\|\|P\|) + O(q\|P\|) = O(\|T\|\|P\|) as q <= \|T\|
    + Same as naive algorithm, but can be improved!

#### Optimization: Precomputation

![Improving Running Time]({{ site.url }}{{ site.baseurl }}/assets/images/improving.png)
{: .image-center}

![Consecutive substrings]({{ site.url }}{{ site.baseurl }}/assets/images/consecutive_substrings.png)
{: .image-center}

![Recurrence of hashes]({{ site.url }}{{ site.baseurl }}/assets/images/recurrence_of_hashes.png)
{: .image-center}

```
PrecomputeHashes(T,|P|,p,x):
H <-- array of length |T|-|P|+1
S <-- T[|T|-|P|..|T|-1]
H[|T|-|P|] <-- PolyHash(S,p,x)
y <-- 1
for i from 1 to |P|:
    y <-- (y*x) mod p
for i from |T|-|P|-1 down to 0:
    H[i] <-- (xH[i+1]+T[i]-yT[i+|P|]) mod p
return H
```

- Precomputing H
    + PolyHash is called once - O(\|P\|)
    + First for loop runs in O(\|P\|)
    + Second for loop runs in O(\|T\|-\|P\|)
    + Total precomputation time O(\|T\|+\|P\|)

#### Optimization: Implementation and Analysis

```
RabinKarp(T,P):
p <-- big prime, x <-- random(1, p-1)
result <-- empty list
pHash <-- PolyHash(P,p,x)
H <-- PrecomputeHashes(T,|P|,p,x)
for i from 0 to |T|-|P|:
    if pHash != H[i]:
        continue
    if AreEqual(T[i..i+|P|-1], P):
        result.Append(i)
return result
```

- Improved Running Time
    + h(P) is computed in O(\|P\|)
    + PrecomputeHashes runs in O(\|T\|+\|P\|)
    + Total time spent in AreEqual is O(q\|P\|) on average where q is the number of occurrences of P in T
    + Average running time
    + O(\|T\| + (q+1)\|P\|)
    + Usually q is small, so this is much less than O(\|T\|\|P\|)

- Conclusion
    + Hash tables are useful for storing Sets and Maps
    + Possible to search and modify hash tables in O(1) on average!
    + Must use good hash families and randomization
    + Hashes are also useful while working with strings and texts
    + There are many more applications in distributed systems and data science.

### Distributed Hash Tables
#### Instant Uploads and Storage Optimization in Dropbox
#### Distributed Hash Tables