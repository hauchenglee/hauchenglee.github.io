---
layout: post
title: Algorithms筆記-Stacks and Queues
category: tech
tags: [algorithms]
---

以下資料整理自：
<br>
**Algorithms, Part I by Princeton University. Week 2: Stacks and Queues**
<br>
-- [Lecture Slides - Coursera](https://www.coursera.org/learn/algorithms-part1/supplement/UAJbP/lecture-slides){:target="_blank"}

其他參看資料：
- [Stacks and Queues - OmarElGabry's Blog - Medium](https://medium.com/omarelgabrys-blog/stacks-and-queues-d96c6f35fae3){:target="_blank"}

## BAGS, QUEUES, AND STACKS

### Stacks and Queues

<span style="color:lightblue">**Fundamental data types.**</span>
- Value: collection of objects.
- Operations: **insert**, **remove**, **iterate**, test if empty.
- Intent is clear when we insert.
- Which item do we remove?

![](http://www.hauchenglee.com/assets/images/tech/algs4-stacks-and-queues.png)

<span style="color:lightblue">**Stack.**</span> Examine the item most recently added. <span style="color:brown">**← LIFO = "last in first out"**</span>
<br>
<span style="color:lightblue">**Queue.**</span> Examine the item most recently added. <span style="color:brown">**← FIFO = "first in first out"**</span>

### Client, implementation, interface

<span style="color:lightblue">**Separate interface and implementation.**</span>
<br>
Ex: stack, queue, bag, priority queue, symbol table, union-find, ....

<span style="color:lightblue">**Benefits.**</span>
- Client can't know details of implementation → client has many implementation from which to choose.
- Implementation can't know details of client needs → many clients can re-use the same implementation.
- **Design:** creates modular, reusable libraries.
- **Performance:** use optimized implementation where it matters.

> **Client:** program using operations defined interface.
>
> **Implementation:** actual code implementing operation.
>
> **Interface:** description of data type, basic operations.

## stacks

### Stack API

<span style="color:lightblue">**Warmup API.**</span> Stack of strings data type.

```
publci class StackOfStrings
---
StackOfStrings() // create an empty stack
void push(String item) // insert a new string onto stack
String pop() // remove and return the string most recently added
boolean isEmpty() // is the stack empty?
int size() // number of strings on the stack
```

<span style="color:lightblue">**Warmup client.**</span> Reverse sequence of strings from standard input.

### Stack test client

Read strings from standard input.
- If string equals "-", pop string from stack and print.
- Otherwise, push string onto stack.

```
public static void main(String[] args) {
    StackOfStrings stack = new StackOfString();
    while (!StdIn.isEmpty()) {
        String s = StdIn.readString();
        if (s.equals("-")) StdOut.print(stack.pop());
        else               stack.push(s);
    }
}
```

```
% more tobe.txt
to br or not to - be - - that - - - is

% java StackOfStrings < tobe.txt
to be not that or be
```

### Stack: linked-list representation

- push

![](http://www.hauchenglee.com/assets/images/tech/algs4-push-on-stack.png)

- pop

![](http://www.hauchenglee.com/assets/images/tech/algs4-pop-from-stack.png)

### Stack: linked-list implementation in Java

```
public class LinkedStackOfStrings {
    private Node first = null;

    private class Node {
        String item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public void push(String item) {
        Node oldFirst = first;
        first = new Node();
        first.item = item;
        first.next = oldFirst;
    }
    
    public String pop() {
        String item = first.item;
        first = first.next;
        return item;
    }
}
```

### Stack: array implementation

<span style="color:lightblue">**Array implementation of a stack.**</span>
- Use array s[] to store N items on stack
- push(): add new item at s[N]
- pop(): remove item from s[N-1]

![](http://www.hauchenglee.com/assets/images/tech/algs4-fixed-array.png)

<span style="color:lightblue">**Defect.**</span> Stack overflows when N exceeds capacity. [stay tuned]

### Stack: array implementation

```
public class FixedCapacityStackOfStrings {
    private String[] s;
    private int N = 0;

    /**
     * constructor
     *
     * @param capacity a cheat (stay tuned)
     */
    public FixedCapacityStackOfStrings(int capacity) {
        s = new String[capacity];
    }

    public boolean isEmpty() {
        return N == 0;
    }

    public void push(String item) {
        // use to index into array;
        // then increment N
        s[N++] = item;
    }

    public String pop() {
        // decrement N;
        // then use to index into array 
        return s[--N];
    }
}
```

### Stack considerations with array implementation

<span style="color:lightblue">**Overflow and underflow.**</span>
- Underflow: throw exception if pop from an empty stack.
- Overflow: use resizing array for array implementation. [stay tuned]

<span style="color:lightblue">**Null items.**</span> We allow null item to be inserted.

<span style="color:lightblue">**Loitering.**</span> Holding a reference to an object when it is no longer needed.

```
public String pop() {
    return s[--N];
}
```

<span style="color:brown">loitering</span>

<br>

```
public String pop() {
    String item = s[--N];
    s[N] = null;
    return item;
}
```

<span style="color:brown">this version avoids "loitering":
<br>garbage collector can reclaim memory
<br>only if no outstanding references</span>

<br>

> *We need to pass the size of the array. This violates the idea of the stack which
>  says it should be able to grow and shrink at any size. That's why stacks not
>  usually implement using a fixed size array.*

### Stack analysis — linked list vs arrays

<span style="color:lightblue">**Proposition.**</span> Every operation takes constant time in the worst case.

<span style="color:lightblue">**Proposition.**</span> A stack with *N* items uses ~ 40 *N* bytes.

![](http://www.hauchenglee.com/assets/images/tech/algs4-stack-performance.png)

<br>

**Time →** With linked List, the methods takes constant time, but it's still not that efficient due to object declaration and dealing with links.
 While with arrays, accessing the arrays is much faster; It takes `O(1)`.

**Memory →** Linked Lists requires more memory because of the size of node objects. While with arrays it requires less memory space.

## Queues

## Generics

## Iterators

## Applications


---