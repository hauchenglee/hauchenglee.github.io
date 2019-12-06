---
layout: post
title: Java筆記-Memory Allocation 內存分配
category: tech
tags: [java]
---

## Where storage lives

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

## Stack vs Heap

my summary:

<table>
    <thead>
        <tr>
            <th></th>
            <th>Stack memory</th>
            <th>Heap space</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>storage</td>
            <td>local variables and function call</td>
            <td>instance variables and objects</td>
        </tr>
        <tr>
            <td>static or dymatic</td>
            <td>static memory</td>
            <td>dynamic memory</td>
        </tr>        
        <tr>
            <td>memory error</td>
            <td>java.lang.StackOverFlowError</td>
            <td>java.lang.OutOfMemoryError</td>
        </tr>
        <tr>
            <td>threads</td>
            <td>stack is kind of private memroy of Java Threads (threadsafe)</td>
            <td>heap memory is shared among all threads (not threadsafe)</td>
        </tr>
    </tbody>
</table>

> - [Difference between Stack and Heap memory in Java](https://javarevisited.blogspot.com/2013/01/difference-between-stack-and-heap-java.html){:target="_blank"}

baeldung summary:

<table>
    <thead>
        <tr>
            <th>Parameter</th>
            <th>Stack memory</th>
            <th>Heap Space</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Application</td>
            <td>Stack in used in parts, one at a time during execution of a thread</td>
            <td>THe entire application uses Heap space during runtime</td>
        </tr>
        <tr>
            <td>Size</td>
            <td>Stack has size limits depending upon OS and is usually smaller then Heap</td>
            <td>THere is no size limit on Heap</td>
        </tr>
        <tr>
            <td>Storage</td>
            <td>Stores only primitive variables and references to objects that are created in Heap Space</td>
            <td>All the newly created objects are stored here</td>
        </tr>
        <tr>
            <td>Order</td>
            <td>It is acessed using Last-in First-out (LIFO) memory allocation system</td>
            <td>This memroy is accessed via complex memory management techniques that include Young Generation, Old Generation or Tenured Generation, and Permanent Generation.</td>
        </tr>
        <tr>
            <td>Life</td>
            <td>Stack memory only exists as long as the current methods is running</td>
            <td>Heap space exists as long as the application runs</td>
        </tr>
        <tr>
            <td>Efficiency</td>
            <td>Comparatively much faster to allocate when compared to heap</td>
            <td>Slower to allocate when compared to stack</td>
        </tr>
        <tr>
            <td>Allocation/<br>Deallocation</td>
            <td>This Memory is automatically allocated and deallocated when a method is called and returned respectively</td>
            <td>Heap space is allocated when new objects are created and deallocated by Garbage Collector when they are on longer referenced</td>
        </tr>
    </tbody>
</table>

> - [Stack Memory and Heap Space in Java - Baeldung](https://www.baeldung.com/java-stack-heap){:target="_blank"}

## Memory allocation in Java

### init object

In Java, all objects are dynamically allocated on Heap. This is different from C++ where objects can be allocated memory either on Stack or on Heap. 
In C++, when we allocate the object using `new()`, the object is allocated on Heap, otherwise on Stack if not global or static.

In Java, when we only declare a variable of a class type, only a reference is created (memory is not allocated for the object). To allocate memory to an object, we must 
use `new()`. So the object is always allocated memory on heap. 
(See [this](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/geninfo/diagnos/garbage_collect.html#wp1085825){:target="_blank"} for more details.)

For example, following program fails in the compilation. Compiler gives error *Error here because i is not initialized*.

```
public class Main {
    public static void main(String[] args) {
        Test t;

        // Error here because t is not initialzed
        t.show();
    }
}
```

Allocating memory using `new()` makes above program work.

```
public class Main {
    public static void main(String[] args) {
        // all objects are dynamically allocated
        Test t = new Test();

        t.show(); // no error
    }
}
```

> - [How are Java objects stored in memory? - GeeksforGeeks](https://www.geeksforgeeks.org/g-fact-46/){:target="_blank"}

### invoke method

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

> - [Difference Between Stack and Heap - Java Question](https://www.erpgreat.com/java/difference-between-stack-and-heap.htm){:target="_blank"}

### memory managed

Let's analyze a simple Java code and let's assess how memory is managed here:

```
class Person {
    int pid;
    String name;
    
    // constructor, setters/getters
}

public class Driver {
    public static vodi main(String[] args) {
        int id = 23;
        String pName = "Jon";
        Person p = null;
        p = new Person(id, pName);
    }
}
```

<br>

Let's analyze this step by step:

1. Upon entering  the `main()` method, a space in stack memory would be created to store primitives and references of this method
   - The primitive value of integer `id` will be stored directly in stack memory
   - The reference variable `p` of type `Person` will also be created in stack memory which will point to the actual object in the heap
2. The call to the parameterized constructor `Person(int, String)` from `main()` will allocate further memory on top of the previous Stack. This will store:
   - The `this` object reference of the calling object in stack memory
   - The primitive value `id` in the stack memory
   - The reference variable of `String` argument `personName` which will point to the actual string from string pool in heap memory 
     (See [string in memory](http://www.hauchenglee.com/tech/2019/11/05/java-string.html#string-in-memory))
3. This default constructor is further calling `setPersonName()` method, for which further allocation will take place in stack memory on top of previous one. 
This will again store variables in the manner described above.
4. However, for the newly created object `p` of type `Person`, all instance variables will be stored in heap memory.

This allocation is explained in this diagram:

![](http://www.hauchenglee.com/assets/images/tech/Stack-Memory-vs-Heap-Space-in-Java.jpg)

> - [Stack Memory and Heap Space in Java - Baeldung](https://www.baeldung.com/java-stack-heap){:target="_blank"}

## Reference

more resource:

- [QuickTip: Java Basics: Stack and Heap](https://vikashazrati.wordpress.com/2007/10/01/quicktip-java-basics-stack-and-heap/){:target="_blank"}

---