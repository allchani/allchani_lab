---
title: "[Coursera03] Data Structures week03" 
excerpt: "Priority Queues and Disjoint Sets"
mathjax : True
---
## Priority Queues and Disjoint Sets
### Priority Queues: Introduction
#### Introduction
- Learning objectives
    + You will be able to:
        * Implement a priority queue
        * Explain what is going on inside built-in implementations:
            - C++: prioirty_queue
            - Java: PrioirtyQueue
            - Python: heapq

- Queue
    + A queue is an abstract data type supporting the following main operations:
        * PushBack(e) adds an element to the back of the queue;
        * PopFront() extracts an element from the front of the queue.

- Priority Queue(Informally)
    + A priority queue is a generalization of a queue where each element is assigned a priority and elements come out in order by priority.

- Priority Queues: Typical Use Case
- Scheduling jobs
    + Want to process jobs one by one in order of decreasing priority. While the current job is processed, new jobs may arrive.
    + To add a job to the set of scheduled jobs, call Insert(job).
    + To process a job with the highest priority, get it by calling ExtractMax().

- Priority Queue(Formally)
    + Definition
        * Priority queue is an abstract data type supporting the following main operations:
            - Insert(p) adds a new element with priority p.
            - ExtractMax() extracts an element with maximum priority.

- Additional Operations
    + Remove(_it_) removes an element pointed by an iteratior _it_
    + GetMax() returns an element with maximum priority(without changing the set of elements)
    + ChangePriority(_it_,_p_) changes the priority of an element pointed by _it_ to _p_

- Algorithms that Use Priority Queues
    + Dijsktra's algoritm: finding a shortest path in a graph
    + Prim's algorithm: constructing a minimum spanning tree of a graph
    + Huffman's algorithm: constructing an optimum prefix-free encoding of a string
    + Heap sort: sorting a given sequence

#### Naive Implementations of Priority Queues 
- Unsorted Array/List
    + Insert(e)
        * add e to the end
        * running time: O(1)
    + ExtractMax()
        * scan the array/list
        * running time: O(n)

- Sorted Array
    + ExtractMax()
        * extract the last element
        * running time: O(1)
    + Insert(e)
        * find a position for e (O(logn)) by using binary search), shift all elements to the right of it by 1 (O(n)), insert e (O(1))
        * running time: O(n)

- Sorted List
    + ExtractMax()
        * extract the last element
        * running time: O(1)
    + Insert(e)
        * find a position for e (O(n); note: cannot use binary search), insert e (O(1))
        * running time: O(n)


- Summary
|                     | Insert   |ExtractMax|
|--------------------:|:--------:|:--------:|
| Unsorted array/list | O(1)     | O(n)     |
| Sorted array/list   | O(n)     | O(1)     |
|     Binary heap     | O(log n) | O(log n) |

### Priority Queues: Heaps
#### Binary Trees
- Definition
    + Binary max-heap is a binary tree(each node has zero, one, or two children) where the value of each node is at least the values of its children.
    + In other words
        * For each edge of the tree, the value of the parent is at least the value of the child.

![Heap/not Heap]({{ site.url }}{{ site.baseurl }}/assets/images/heap_notheap.png)
{: .image-center}

#### Basic Operations
- 1_GetMax
    + return the root value
    + running time: O(1)
- 2_Insert
    + 1_attach a new node to any leaf(this may violate the heap property)
- 3_Sift Up
    + 1_to fix this, we let the new node sift up
    + 2_for this, we swap the problematic node with its parent until the property is satisfied.
    + *invariant*: heap property is violated on at most one edge
    + this edge gets closer to the root while sifting up
    + running time: O(tree height)
- 4_ExtractMax
    + 1_replace the root with any leaf(again, this may violate the heap property)
    + 2_to fix it, we let the problematic node sift down
    + 3_for this, we swap the problematic node with larger child until the heap property is satisfied.
    + 4_we swap with the larger child which automatically fixes one of the two bad edges.
    + running time: O(tree height)
- 5_ChangePriority
    + 1_change the priority and let the changed element sift up or down depending on whether its priority decreased or increased
    + running time: O(tree height)
- 6_Remove
    + 1_change the priority of the element to infinite, let it sift up, and then extract maximum.
    + running time: O(tree height)

- Summary
    + GetMax works in time O(1), all other operations work in time O(tree height)
    + We definitely want a tree to be shallow

#### Complete Binary Trees
- How to Keep a Tree Shallow?
    + Definition:
    + A binary tree is **complete** if all its levels are filled except possibly the last one which is filled from left to right.
![complete tree and not]({{ site.url }}{{ site.baseurl }}/assets/images/comlete_tree.png)
{: .image-center}

- First Advantage: Low Height
    + Lemma:
        * A complete binary tree with n nodes has height at most O(log n)
    + Proof:
        * Complete the last level to get a full binary tree on n'>=n nodes and the same number of levels _l_.
        * n' = n_full + 1, n = n_full + alpha, alpha >= 1
        * Note that n' <= 2n
        * Then n' = 2^l - 1 and hence l = log_2(n'+1) <= log_2(2n+1) = O(log n)

- Second Advantage: Store as Array
![Second Advantage]({{ site.url }}{{ site.baseurl }}/assets/images/second_advantage.png)
{: .image-center}
- What do we pay for these advantages?
    + We need to keep the tree complete.
- Which binary heap operations modify the shape of the tree?
    + Only Insert and ExtractMax(Remove changes the shape by calling ExtractMax)

- Keeping the Tree Complete
    + to insert an element, insert it as a leaf in the **leftmost vacant position in the last level** and let it sift up
    + to extract the maximum value, replace the root by **the last leaf** and let it sift down
    
#### Pseudocode
- General Setting
    + **maxSize** is the maximum number of elements in the heap
    + **size** is the size of the heap
    + **H[1...maxSize]** is an array of length maxSize where the heap occupies the first *size* elements

```
Parent(i):
return i // 2

LeftChild(i):
return 2i

RightChild(i):
return 2i + 1
```

```
SiftUp(i):
while i>1 and H[Parent(i)] < H[i]:
    swap H[Parent(i)] and H[i]
    i <-- Parent(i)
```

```
SiftDown(i):
maxIndex <-- i
l <-- LeftChild(i)
if l <= size and H[l] > H[maxIndex]:
    maxIndex <-- l
r <-- RightChild(i)
if r <= size and H[r] > H[maxIndex]:
    maxIndex <-- r
if i != maxIndex:
    swap H[i] and H[maxIndex]
    SiftDown(maxIndex)
```

```
Insert(p):
if size = maxSize:
    return ERROR
size <-- size + 1
H[size] <-- p
SiftUp(size)
```

```
ExtractMax():
result <-- H[1]
H[1] <-- H[size]
size <-- size - 1
SiftDown(1)
return result
```

```
Remove(i):
H[i] <-- infinite
SiftUp(i)
ExtractMax()
```

```
ChangePriority(i,p):
oldp <-- H[i]
H[i] <-- p
if p > oldp:
    SiftUp(i)
else:
    SiftDown(i)
```

- Summary
- The resulting implementation is
    + fast: all operations work in time O(log n) (GetMax even works in O(1))
    + space efficient: we store an array of prioirties; parent-child connections are not stored, but are computed on the fly
    + easy to implement: all operations are implemented in just a few lines of code

### Priority Queues: Heap Sort
- Sort Using Priority Queues

```
HeapSort(A[1...n]):
create an empty priority queue
for i from 1 to n:
    Insert(A[i])
for i from n downto 1:
    A[i] <-- ExtractMax()
```

- The resulting algorithms is comparison-based and has running time O(nlog n)(hence, asymptotically optimal!).
- Natural generalization of selection sort: instead of simply scanning the rest of the array to find the maximum value, use a smart data structure.
- Not in-place: uses additional space to store the priority queue.

- This lesson
    + In-place heap sort algorithm. For this, we will first turn a given array into a heap by permuting its elements.
- Turn Array into a Heap

```
BuildHeap(A[1...n]):
size <-- n
for i from n//2 downto 1:
    SiftDown(i)
```

- We repair the heap property going from bottom to top.
- Initially, the heap property is satisfied in all the leaves(i.e., subtrees of depth 0).
- We then start repairing the heap property in all subtrees of depth 1.
- When we reach the root, the heap property is satisfied in the whole tree.
- Online visualization
- Running time: O(nlog n)

- In-place Heap Sort

```
HeapSort(A[1...n]):
BuildHeap(A)            {size=n}
repeat (n-1) times:
    swap A[1] and A[size]
    size <-- size - 1
    SiftDown(1)
```

- Building Running Time
    + The running time of BuildHeap is O(nlogn) since we call SiftDown for O(n) nodes.
    + If a node is already close to the leaves, then sifting it down is fast.
    + We have many such nodes!
    + Was our estimate of the running time of BuildHeap too pessimistic?

![Building Running Time]({{ site.url }}{{ site.baseurl }}/assets/images/building_running_time.png)
{: .image-center}

![Estimating the Sum]({{ site.url }}{{ site.baseurl }}/assets/images/estimating_the_sum.png)
{: .image-center}

- Partial sorting
    + Input: An array A[1....n], an integer 1<=k<=n
    + Output: The last k elements of a sorted version of A.
    + Cna be solved in O(n) if k = O(n/log n)

```
PartialSorting(A[1...n],k):
BuildHeap(A)
for i from 1 to k:
    ExtractMax()
```

- Running time: O(n + klogn), if k = O(n/log n) --> O(n) 

- Summary
    + Heap sort is a time and space efficient comparison-based algorithm: has running time O(nlogn), uses no additional space.

### Disjoint Sets: Naive Implementations
#### Overview
![Maze]({{ site.url }}{{ site.baseurl }}/assets/images/maze.png)
{: .image-center}
- Definition
    + A disjoint-set data structure supports the following operations:
        * MakeSet(x) creates a singleton set {x}
        * Find(x) returns ID of the set containing x:
            - if x and y lie in the same set, then Find(x)=Find(y)
            - otherwise, Find(x) != Find(y)
        * Union(x, y) merges two sets containing x and y

```
Preprocess(maze):
for each cell c in maze:
    MakeSet(c)
for each cell c in maze:
    for each neighbor n of c:
        Union(c, n)
```

```
IsReachable(A,B):
return Find(A)=Find(B)
```

- Building a Network

#### Naive Implementations
- For simplicity, we assume that our n objects are just integers 1,2,...,n.

- Using the Smallest Element as ID
    + Use the smallest element of a set as its ID
    + Use array smallest[1...n]:
    + smallest[i] stores the smallest element in the set i belongs to

![Smallest Example]({{ site.url }}{{ site.baseurl }}/assets/images/smallest_example.png)
{: .image-center}

```
MakeSet(i):
smallest[i] <-- i

Find(i):
return smallest[i]

#running time: O(1)
```

```
Union(i,j):
i_id <-- Find(i)
j_id <-- Find(j)
if i_id = j_id:
    return
m <-- min(i_id, j_id)
for k from 1 to n:
    if smallest[k] in {i_id, j_id}:
        smallest[k] <-- m

#running time: O(n)
```

- Current bottleneck: Union
- What basic data structure allows for efficient merging?
- Linked list!
- Idea: represent a set as a linked list, use the list tail as ID of the set

![Merging Two lists]({{ site.url }}{{ site.baseurl }}/assets/images/merging_two_lists.png)
{: .image-center}

- Pros:
    + Running time of Union is O(1)
    + Well-defined ID
- Cons:
    + Running time of Find is O(n) as we need to traverse the list to find its tail
    + Union(x,y) works in time O(1) **only** if we can get the tail of the list of x and the head of the list of y in constant time!

![Merging Two lists with Tree]({{ site.url }}{{ site.baseurl }}/assets/images/meging_two_lists_tree.png)
{: .image-center}

### Disjoint Sets: Efficient Implementation
#### Trees for Disjoint Sets

- Represent each set as a rooted tree
- ID of a set is the root of the tree
- Use array parent[1...n]: parent[i] is the parent of i, or i if it is the root

![Disjoint example]({{ site.url }}{{ site.baseurl }}/assets/images/disjoint_example.png)
{: .image-center}

```
MakeSet(i):
parent[i] <-- i
#running time: O(1)

Find(i):
while i != parent[i]:
    i <-- parent[i]
return i
#running time: O(tree height)
```

- How to merge two trees?
- Hang one of the trees under the root of the other one
- Which one to hang?
- A shorter one, since **we would like to keep the tree shallow**

#### Union by Rank
- When merging two trees we hang a shorter one under the root of a taller one
- To quickly find a height of a tree, we will keep the height of each subtree in an array rank[1...n]: rank[i] is the height of the subtree whose root is i
- (The reason we call it rank, but not height will become clear later)
- Hanging a shorter tree under a taller one is called a **union by rank heuristic**

```
MakeSet(i):
parent[i] <-- i
rank[i] <-- 0

Find(i):
while i != parent[i]:
    i <-- parent[i]
return i
```

```
Union(i,j):
i_id <-- Find(i)
j_id <-- Find(j)
if i_id = j_id:
    return
if rank[i_id] > rank[j_id]:
    parent[j_id] <-- i_id
else:
    parent[i_id] <-- j_id
    if rank[i_id] = rank[j_id]:
        rank[j_id] <-- rank[j_id] + 1
```

![Rank Example]({{ site.url }}{{ site.baseurl }}/assets/images/rank_example.png)
{: .image-center}

- **Important property**:for any node i, rank[i] is equal to the height of the tree rooted at i

- Lemma: The height of any tree in the forest is at most log_2(n)
- follows from the following lemma.
- Lemma: Any tree of height *k* in the forest has at least 2^k nodes.

- Proof:
- Induction on k.
    + Base: initially, a tree has height 0 and one node: 2^0 = 1.
    + Step: a tree of height k-1. By induction hypothesis, each of two trees has at least 2^k-1 nodes, hence the resulting tree contains at least 2^k nodes

- Summary
    + The union by rank heuristic guarantees that Union and Find work in time O(log n).
- Next part
    + We'll discover another heuristic that improves the running time to nearly constant!


#### Path Compression
- Path Compression: Intuition
    + Find(6) traverses the path from 6 to the root
    + not only it finds the root for 6, it does so for all the nodes on this path
    + let's not lose this useful info

![Path Compression Intuition]({{ site.url }}{{ site.baseurl }}/assets/images/path_compression_intuition.png)
{: .image-center}

```
Find(i)
if i != parent[i]:
    parent[i] <-- Find(parent[i])
return parent[i]
```

- Definition
    + The iterated logarithm of n, log*n, is the number of times the logarithm function needs to be applied to n before the result is less or equal than 1:
    + log*n = 0 if n <= 1, log*n = 1+log*(log n) if n > 1 

![Log Star Example]({{ site.url }}{{ site.baseurl }}/assets/images/log_star_example.png)
{: .image-center}

- Lemma
    + Assume that initially the data structure is empty. We make a sequence of m operations including n calls to MakeSet. Then the total running time is O(m log*n).


#### Analysis