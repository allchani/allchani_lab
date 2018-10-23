---
title: "[Coursera03] Data Structures week01" 
excerpt: "Basic Data Structures"
mathjax : True
---
## Arrays and Linked Lists
### Arrays
- Definition
    + Array: Contigouous area of memory 
    + consisting of equal-size elements 
    + indexed by contiguous integers.

- What's Special about Arrays?
    + **Constant-time access(Huge advantage!! read or write)**
    + array_addr + elem_size x (i-first_index)
        * Given an array whose:
            - adress is 1000,
            - element size is 8
            - first index is 0
            - what is the address of the element at index 6? 1048

- Multi-Dimensional Arrays
    + 3x6 array, example:(3,4)
    + array_addr + elem_size x((3-1)x6 + (4-1))
- row-major ordering(indexing)
    + (1,1), (1,2), (1,3), (1,4), (1,5), (1,6), (2,1), (2,2)...
    + column changes rapidly
- column-major ordering(indexing)
    + (1,1), (2,1), (3,1), (1,2), (2,2), (3,2), (1,3), (2,3)...
    + row changes rapidly

|           | Add  | Remove |
|----------:|:----:|:------:|
| Beginning | O(n) |  O(n)  |
|    End    | O(1) |  O(1)  |
|   Middle  | O(n) |  O(n)  |

- Summary
    + Array: contiguous area of memory consisting of equal-size elements indexed by contiguous integers.
    + Constant-time access to any element.
    + Constant time to add/remove at the end.
    + Linear time to add/remove at an arbitary location.


### Singly-Linked Lists
- Singly-Linked List
    + it's named kind of like links in a chain.
    + We've got a head pointer that points to a node that then has some data and points to another node, points to another node...

![Singly_liked_list]({{ site.url }}{{ site.baseurl }}/assets/images/singlelinkedlist.png)
{: .image-center}

- Node contains:
    + Key
    + next pointer

|          List API                      |
|:------------------|:-------------------|
|PushFront(Key)     |add to front        |
|Key TopFront()     |return front item   |
|PopFront()         |remove front item   |
|PushBack(Key)      |add to back         |
|Key TopBack()      |return back item    |
|PopBack()          |remove back item    |
|Boolean Find(key)  |is key in list?     |
|Erase(Key)         |remove key from list|
|Boolean Empty()    |empty list?         |
|AddBefore(Node,Key)|adds key before node|
|AddAfter(Node,Key) |adds key after node |

```
PushFront(key):
node <-- new node #allocate a new node
node.key <-- key #set its key
node.next <-- head #set its next to point to the old head
head <-- node #update the current head pointer.
if tail == nil:    #If the tail is equal to nil,
    tail <-- head  #that meant that before the insertion, 
                   #the head and the tail were nil, it was empty list.
                   #So we've got to update the tail to point to the same thing the head points to.
```

```
PopFront():
if head == nil:
    ERROR: empty list
head <-- head.next
if head == nil:
    tail <-- nil #just in case that there was only one element in the list and now there are no elements, we check if our new head is nil and if so update our tail to also be nil.
```

```
PushBack(key):
node <-- new node
node.next = nil
if tail = nil:
    head <-- tail <-- node
else:
    tail.next <-- node
    tail <-- node
```

```
PopBack():
if head == nil: ERROR: empty list
if head == tail:
    head <-- tail <-- nil
else:
    p <-- head
    while p.next.next != nil:
        p<--p.next
    p.next <-- nil; tail <-- p
```

```
AddAfter(node,key):
node2 <-- new node
node2.key <-- key
node2.next = node.next
node.next = node2
if tail = node:
    tail <-- node2
```

![Running_Time]({{ site.url }}{{ site.baseurl }}/assets/images/runningtime.png){: .image-center}

### Doubly-Linked Lists
![Doubly_Linked_List]({{ site.url }}{{ site.baseurl }}/assets/images/doubly_linked_list.png)
{: .image-center}

```
PushBack(key):
node <-- new node
node.key <-- key; node.next = nil
if tail = nil:
    head <-- tail <-- node
    node.prev <-- nil
else:
    tail.next <-- node
    node.prev <-- tail
    tail <-- node
```

```
PopBack():
if head = nil: ERROR: empty list
if head = tail:
    head <-- tail <-- nil
else:
    tail <-- tail.prev
    tail.next <-- nil
```

```
AddAfter(node,key):
node2 <-- new node
node2.key <-- key
node2.next <-- node.next
node2.prev <-- node
node.next <-- node2
if node2.next != nil:
    node2.next.prev <-- node2
if tail = node:
    tail <-- node2
```

```
AddBefore(node,key):
node2 <-- new node
node2.key <-- key
node2.next <-- node
node2.prev <-- node.prev
node.prev <-- node2
if node2.prev != nil:
    node2.prev.next <-- node2
if head = node:
    head <-- node2
```

![Doubly_Linked_List_running_time]({{ site.url }}{{ site.baseurl }}/assets/images/doublelinkedlist.png)
{: .image-center}

- Summary
    + Constant time to insert at or remove from the front
    + With tail and doubly-linked, constant time to insert at or remove from the back.
    + O(n) time to find arbitary element.
    + List elements need not be contiguous.
    + With doubly-linked list, constant time to insert between nodes or remove a node.

## Stacks and Queues
### Stacks
- Definition
    + Stack: Abstract data type with the following operations:
        *  Push(Key): adds key to collection
        *  Key Top(): Returns most recently-added key
        *  Key Pop(): removes and returns most recently-added key
        *  Boolean Empty(): are there any elements?

- Balanced Brackets
    + Input: A string str consisting of '(',')','[',']' characters.
    + Output: Return whether or not the string's parentheses and square brackets are balanced.
    + example:
        * Balanced:
            - "([])[]()","((([([])]))())"
        * Unbalanced:
            - "([]]()","]["

```
IsBalanced(str):
Stack stack
for char in str:
    if char in ['(','[']:
        stack.Push(char)
    else:
        if stack.Empty(): return False
        top <-- stack.Pop()
        if (top= '[' and char != ']') or
          (top= '(' and char != ')'):
          return False
return stack.Empty()
```

- Given the unbalanced string "()([]", what character is on the top of the stack when the for loop is finished?
- The stack will contain a single value: '(', The procedure will push the initial '(', and then pop it off to match the ')'. It'll then push the next '(', and the '[', and then pop off the '[' to match the ']'. What remains will the the '('.

- Stack Implementation with Array
    + one disadvantage(limitation) of the array is that we have a maximum size, based on the array we initially allocated.
    + We have potentially wasted space.

- Summary
    + Stacks can be implemented with either an array or a linked list.
    + Each stack operation is O(1): Push, Pop, Top, Empty
    + Stacks are ocassionaly known as LIFO(last in first out) queues.

### Queues
- Definition
    + Queue: Abstract data type with the following operations:
        * Enqueue(Key): adds key to collection
        * Key Dequeue(): removes and returns least recently-added key
        * Boolean Empty(): are there any elements?
    + FIFO: First-In, First-Out

- Queue Implementation with Linked List
    + Enqueue: use List.PushBack
    + Dequeue: use List.TopFront and List.PopFront
    + Empty: use List.Empty

- Queue Implementation with Array
    + Queues can be implemented with either a linked list(with tail pointer) or an array.
    + Each queue operation is O(1): Enqueue, Dequeue, Empty

## Trees
### Trees
- Definition
    + A Tree is:
        * empty, or
        * a node with:
            - a key, and
            - a list of child trees.

- Simple Tree
    + Empty tree:
    + Tree with one node:
        * Fred
    + Tree with two nodes:
        * Fred -- Sally

- Terminology
    + Root: top node in the tree
    + A child has a line down directly from a parent
    + Ancestor: parent, or parent of parent, etc.
    + Descendant: child, or child of child, etc.
    + Sibiling: sharing the same parent
    + Leaf: node with no children
    + Interior node(non-leaf)
    + Level: 1+ num edges between root and node
    + Height: maximum depth of subtree node and farthest leaf
    + Forest: collection of trees

- Node contains:
    + key
    + children: list of children nodes
    + (optional) parent

- For binary tree: node contains:
    + key
    + left
    + right
    + (optional) parent

```
Height(tree):
if tree == nil:
    return 0
return 1+Max(Height(tree.left), Height(tree.right))
```

```
Size(tree):
if tree == nil:
    return 0
return 1 + Size(tree.left) + Size(tree.right)
```

### Tree Traversal
- Walking a Tree
    + Often we want to visit the nodes of a tree in a particular order.
    + For example, print the nodes of the tree.
        * Depth-fisrt: We completely traverse one sub-tree before exploring a sibling sub-tree
        * Breadth-first: We traverse all nodes at one level before progressing to the next level


- Depth first
```
InOrderTraversal(tree):
if tree == nil:
 return
InOrderTraversal(tree.left)
Print(tree.key)
InOrderTraversal(tree.right)
```

![InOrderTraversal]({{ site.url }}{{ site.baseurl }}/assets/images/inordertraversal.png)
{: .image-center}

- Depth first
```
PreOrderTraversal(tree):
if tree == nil:
 return
Print(tree.key)
PreOrderTraversal(tree.left)
PreOrderTraversal(tree.right)
```
![PreOrderTraversal]({{ site.url }}{{ site.baseurl }}/assets/images/preordertraversal.png)
{: .image-center}


```
PostOrderTraversal(tree):
if tree == nil:
 return
PostOrderTraversal(tree.left)
PostOrderTraversal(tree.right)
Print(tree.key)
```
![PostOrderTraversal]({{ site.url }}{{ site.baseurl }}/assets/images/postordertraversal.png)
{: .image-center}

- Breadth-first
```
LevelTraversal(tree):
if tree == nil: return
Queue q
q.Enqueue(tree)
while not q.Empth():
    node <-- q.Dequeue()
    Print(node)
    if node.left != nil:
        q.Enqueue(node.left)
    if node.right != nil:
        q.Enqueue(node.right)
```
![LevelTraversal]({{ site.url }}{{ site.baseurl }}/assets/images/leveltraversal.png)
{: .image-center}

- Summary
    + Trees are used for lots of different things.
    + Trees have a key and children.
    + Tree walks: DFS(pre-order, in-order, post-order) and BFS
    + When working with a tree, recursive algorithms are common.
    + In Computer Science, trees grow down!


