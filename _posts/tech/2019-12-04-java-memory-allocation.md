---
layout: post
title: Java筆記-Memory Allocation 內存分配
category: tech
tags: [java]
---

## Where storage lives

### where storage lives

 - Registers<br>
   This is the fastest storage because it exists in a place different from that of other storage: inside the
   central processing unit (CPU). However, the number of registers is severely limited, so registers are
   allocated as they are needed. You don't have direct control over register allocation, nor do you see
   any evidence in your programs that registers even exist (C & C++, on the other hand, allow you to
   suggest register allocation to the compiler)

 - The stack<br>
   This lives in the general random-access memory (RAM) area, but has direct support from the
   processor via its stack pointer. The stack pointer is moved down to create new memory and moved
   up to release that memory. This is an extremely fast and efficient way to allocate storage, second
   only to registers. **The Java system must know, while it is creating the program, the exact lifetime of
   all the items stored on the stack.** This constraint places limits on the flexibility of your programs, so
   themselves are not placed on the stack.

 - The heap<br>
   This is a general-purpose pool of memory (also in the RAM area) where all Java objects live. **Unlike
   the stack, the compiler doesn't need to know how long objects must stay on the heap. Thus, there's
   a great deal of flexibility when using heap storage. Whenever you need an object, you write the
   code to create it using new, and the storage is allocated on the heap when that code is executed.**
   There's a price for this flexibility: It can take more time to allocate and clean up heap storage than
   stack storage (if you even could create objects on the stack in Java, as you can in C++). Over time,
   however, Java's heap allocation mechanism has become very fast, so this is not an issue for
   concern.

 - Constant storage<br>
   **Constant values are often placed directly in the program code, which is safe since they can never
   change.** Sometimes constants are cordoned off by themselves so they can be optionally placed in
   read-only memory (ROM), in embedded systems.

 - Non-RAM storage<br>
   **If data lives completely outside a program, it can exist while the program is not running, outside the
   control of the program.** The two primary examples of this area serialized object, where objects are
   turned into streams of bytes, generally send to another machine, and persistent objects, where the
   objects are placed on disk so they will hold their state even when the program is terminated. The
   trick with there types of storage is turning the objects into something that exist on the other medium,
   and yet be resurrected into a regular RAM-based object when necessary. Java provides support for
   lightweight persistence. Libraries such as JDBC and Hibernate provide more sophisticated support
   for storing and retrieving object information using databases.

> -- *Thinking in Java 4th edition page 42*

## Stack and Heap

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>


### stack

- **Instance variables and Objects lie on Heap**

Remember this is where the state is maintained and when you get *[memory leaks](https://en.wikipedia.org/wiki/Memory_leak){:target="_blank"}* this is where your profiler helps you to find the allocation of memory.

- **Local variables and methods lie on the Stack**<br>

So if we have a main method which calls the `go()` method which calls the `gone()` method then the stack from top to bottom would consist of:

```
gone()
go()
main()
```

as soon as `gone()` has been processed, it would be removed from the stack. Any corresponding local variables which are used in `gone()` would also would also be removed from the stack.
Stack would have references to objects on the Heap.

![](http://www.hauchenglee.com/assets/images/tech/stacknheap.png)

> - [QuickTip: Java Basics: Stack and Heap - c0nnexx10n : C0nnect1ng L1fe w1th Techn010gy](https://vikashazrati.wordpress.com/2007/10/01/quicktip-java-basics-stack-and-heap/){:target="_blank"}

### heap

## Memory exception and error

`StackOverFlow`

`OutOfMemory`

`MemoryLeak`

> - [Difference Between Stack and Heap - Java Question](https://www.erpgreat.com/java/difference-between-stack-and-heap.htm){:target="_blank"}
> - [java - What is the difference between an OutOfMemoryError and a memory leak - Stack Overflow](https://stackoverflow.com/questions/4943518/what-is-the-difference-between-an-outofmemoryerror-and-a-memory-leak){:target="_blank"}


## Memory allocation in Java


---