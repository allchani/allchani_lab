---
title: "[Coursera03] Data Structures week02" 
excerpt: "Dynamic Arrays and Amortized Analysis"
mathjax : True
---

## Dynamic Arrays and Amortized Analysis
### Dynamic Arrays
- Problem: static arrays are static!
- int my_array[100];
- Semi-solution: dynamically-allocated arrays:
- int *my_array = new int[size]; 
  
  
- Problem: might not know max size when allocating an array
    + All problems in computer science can be solved by another level of indirection.
- Solution: dynamic arrays(also known as resizable arrays)
- Idea: store a pointer to a dynamically allocated array, and replace it with a newly-allocated array as needed.


- Definition
    + Dynamic Array: Abstract data type with the following operations(at a minimum):
    + Get(i): returns element at location i*
    + Set(i, val): Sets element i to val*
    + PushBack(val): Adds val to the end
    + Remove(i): Removes element at location i
    + Size(): the number of elements

- Implementation
    + Store:
    + arr: dynamically-allocated array
    + capacity: size of the dynamically-allocated array
    + size: number of elements currently in the array

```
Get(i):
if i<0 or i >= size:
    ERROR: index out of range
return arr[i]
```

```
Set(i,val):
if i<0 or i>=size:
    ERROR: index out of range
arr[i]=val
```

```
PushBack(val):
if size = capacity:
    allocate new_arr[2 x capacity]
    for i from 0 ti size-1:
        new_arr[i] <-- arr[i]
    free arr
    arr <-- new_arr; capacity <-- 2 x capacity
arr[size] <-- val
size <-- size + 1
```

```
Remove(i):
if i<0 opr i>=size:
    ERROR: index out of range
for j from i to size-2:
    arr[j] <-- arr[j+1]
size <-- size-1
```

```
Size():
return size
```

- Common Implementations
    + C++: vector
    + Java: ArrayList
    + Python: list(the only kind of array)

- Runtimes
    + Get(i): O(1)
    + Set(i,val): O(1)
    + PushBack(val): O(n)
    + Remove(i): O(n)
    + Size(): O(1)

- Summary
    + Unlike static arrays, dynamic arrays can be resized.
    + Appending a new element to a dynamic array is often constant time, but can take O(n).
    + Some space is wasted.
```
What is the tightest correct upper bound on the running time of a PushBackPushBack operation on an dynamic array currently containing nn elements in the worst-case?

O(n^2)
O(nlogn)
O(n)

Correct 
Correct! If the dynamic array needs to be resized to accomodate the new element, it will take O(n)O(n) to create a new array of double size and copy all the current elements into it.

O(1).
```

### Amortized Analysis: Aggregate Method

- Sometimes, looking at the individual worst-case may be too severe. We may want to know the total worst-case cost for a sequence of operations.

- Dynamic Array
    + We only resize every so often. Many O(1) operations are followed by an o(n) operations.
    + What is the total cost of inserting many elements?

- Definition
    + Amortized cost: Given a sequence of n operations, the amortized cost is:
    + $$\frac {Cost(n operations)}{n}$$

![Aggregated Method]({{ site.url }}{{ site.baseurl }}/assets/images/aggregate_method.png)
{: .image-center}

![quiz]({{ site.url }}{{ site.baseurl }}/assets/images/amortized_quiz.png)
{: .image-center}

### Amortized Analysis: Banker's Method
- Banker's Method
    + Charge extra for each cheap operation.
    + Save the extra charge as tokens in your data structure(conceptually).
    + Use the tokens to pay for expensive operations.
    + --Like an amortizing loan

- Dynamic array: n calls to PushBack
- Charge 3 for each insertion: 1 token is the raw cost for insertion.
    + Resize needed: To pay for moving the elements, use the token that's present on each element that needs to move.
    + Place one token on the newly-inserted element, and one token $$\frac{capacity}{2}$$ element prior.

### Amortized Analysis: Physicist's Method
- Physicist's Method
    + Define a potential function, $$\Phi$$ which maps states of the data structure to integers:
        * $$\Phi(h0) = 0$$
        * $$\Phi(ht) >= 0$$
    + amortized cost for opertion t:
    + $${C}\_{t}+\Phi({h}\_{t})-\Phi({h}\_{t-1})$$
    + Choose $$\Phi$$ so that:
        * if Ct is small, the potential increases
        * if Ct is large, the potential decreases the same scale

![Physicists Method]({{ site.url }}{{ site.baseurl }}/assets/images/physicists_method.png)
{: .image-center}

- Dynamic array: n calls to PushBack
- Let $$\Phi(h) = 2xsize - capacity$$
    + $$\Phi({h}\_{0}) = 2 x 0 - 0 = 0$$
    + $$\Phi({h}\_{i}) = 2 x size - capacity > 0$$
    + (since size > capacity/2)

- Dynamic Array Resizing
    + Without resize when adding element i
    + Amortized cost of adding element i:
    + $${C}\_{i}+\Phi({h}\_{i})-\Phi({h}\_{i-1})$$
    + = 1 + 2 x size_i - cap_i - (2 x size_i-1 - cap_i-1)
    + = 1 + 2 x (size_i - size_i-1)
    + = 3

- Dynamic Array Resizing
    + With resize when adding element i
    + Let k = size(i-1) = cap(i-1)
    + Then:
    + $$\Phi(h(i-1))$$ = 2size(i-1) - cap(i-1) = 2k-k = k
    + $$\Phi(h(i))$$ = 2size(i) - cap(i) = 2(k+1) -2k = 2
    + Amortized cost of adding element i:
        * $${C}\_{i}+\Phi({h}\_{i})-\Phi({h}\_{i-1})$$
        * = (size(i)) + 2 - k
        * = (k+1) + 2 - k
        * = 3

### Amortized Analysis: Summary
- Alternatives to Doubling the Array Size
    + We could use some different growth factor(1.5, 2.5, etc.).
    + Could we use a constant amount?


![Cannot Use Constant Amount]({{ site.url }}{{ site.baseurl }}/assets/images/cannotuse.png)
{: .image-center}

- Summary
    + Calculate amortized cost of an operation in the context of a sequence of operations.
    + Three ways to do analysis:
        * Aggregate method(brute-force sum)
        * Banker's method(tokens)
        * Physicist's method(potential function, $$\Phi$$)
    + Nothing changes in the code: runtime analysis only.