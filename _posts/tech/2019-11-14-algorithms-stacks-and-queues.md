---
layout: post
title: Algorithms筆記-Stacks and Queues
category: tech
tags: [algorithms]
---

以下資料整理自：
**Algorithms, Part I by Princeton University. Week 2: Stacks and Queues**
[Lecture Slides - Coursera](https://www.coursera.org/learn/algorithms-part1/supplement/UAJbP/lecture-slides){:target="_blank"}

其他參看資料：
- [Stacks and Queues - OmarElGabry's Blog - Medium](https://medium.com/omarelgabrys-blog/stacks-and-queues-d96c6f35fae3){:target="_blank"}

## BAGS, QUEUES, AND STACKS

### Stacks and Queues

Fundamental data types:
- Value: collection of objects.
- Operations: **insert**, **remove**, **iterate**, test if empty.
- Intent is clear when we insert.
- Which item do we remove?

![](http://www.hauchenglee.com/assets/images/tech/algs4-stacks-and-queues.png)

**Stack.** Examine the item most recently added. ← **LIFO = "last in first out"**
<br>
**Queue.** Examine the item most recently added. ← **FIFO = "first in first out"**

### Client, implementation, interface

Separate interface and implementation.
<br>
Ex: stack, queue, bag, priority queue, symbol table, union-find, ....

Benefits.
- Client can't know details of implementation → client has many implementation from which to choose.
- Implementation can't know details of client needs → many clients can re-use the same implementation.
- **Design:** creates modular, reusable libraries.
- **Performance:** use optimized implementation where it matters.

> **Client:** program using operations defined interface.
><br>
> **Implementation:** actual code implementing operation.
><br>
> **Interface:** description of data type, basic operations.

## stacks

### Stack API

**Warmup API.** Stack of strings data type.

```
publci class StackOfStrings
---
StackOfStrings() // create an empty stack
void push(String item) // insert a new string onto stack
String pop() // remove and return the string most recently added
boolean isEmpty() // is the stack empty?
int size() // number of strings on the stack
```

**Warmup client.** Reverse sequence of strings from standard input.

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

### Stack: linked-list implementation performance

**Proposition.** Every operation takes constant time in the worst case.

**Proposition.** A stack with *N* items uses ~ 40 *N* bytes.

![](http://www.hauchenglee.com/assets/images/tech/algs4-stack-performance.png)

**Remark.** This accounts for the memory for the stack
<br>
(but not the memory for strings themselves, which the client owns).

### Stack: array implementation

**Array implementation of a stack.**
- Use array s[] to store N items on stack
- push(): add new item at s[N]
- pop(): remove item from s[N-1]

### Stack: array implementation

### Stack considerations

## resizing arrays

## queues

## generics

## iterators

## applications


---