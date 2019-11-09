---
layout: post
title: Java筆記-Collections 集合
category: tech
tags: [java]
---

## Introduction



## Java Collection Framework

<table>
    <thead>
        <tr>
            <th>Concrete Class / Map</th>
            <th>Interface</th>
            <th>Duplicate Elements</th>
            <th>Ordered</th>
            <th>Sorted</th>
            <th>Allow Null</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ArrayList</td>
            <td>List</td>
            <td>Yes</td>
            <td>By Index</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Vector</td>
            <td>List</td>
            <td>Yes</td>
            <td>By Index</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>LinkedList</td>
            <td>List, Queue</td>
            <td>Yes</td>
            <td>By Index</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>HashSet</td>
            <td>Set</td>
            <td>No</td>
            <td>No Order</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>LinkedHashSet</td>
            <td>Set</td>
            <td>No</td>
            <td>By Insertion Order</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>TreeSet</td>
            <td>SortedSet</td>
            <td>No</td>
            <td>Sorted</td>
            <td>Natural / Custom</td>
            <td>No</td>
        </tr>
        <tr>
            <td>HashMap</td>
            <td>Map</td>
            <td>unique keys</td>
            <td>No Order</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>HashTable</td>
            <td>Map</td>
            <td>unique keys</td>
            <td>No Order</td>
            <td>No</td>
            <td>No</td>
        </tr>
        <tr>
            <td>LinkedHashMap</td>
            <td>Map</td>
            <td>unique keys</td>
            <td>By Insertion Order or Last Accessed</td>
            <td>No</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>TreeMap</td>
            <td>SortedMap</td>
            <td>unique keys</td>
            <td>Key Order</td>
            <td>Natural / Custom</td>
            <td>No</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>Queue</td>
            <td>Yes</td>
            <td>Natural Ordering</td>
            <td>Natural Ordering</td>
            <td>No</td>
        </tr>                                                
    </tbody>
</table>

- **Java Collections Framework**: As its name suggests, it is a collection, collection of objects called elements. A collection can be ordered or unordered some collections allow duplicate and some
 are not.

- **Collection Interface**: You cannot use the Collection Interface directly in the code. Instead, it has been extended by List, Set and Queue.

- **List**: List is ordered means preserve the order, but not sorted. It allows duplicate elements and null as well. Elements can be accessed by using the index number, indexing starts from zero.

- **Set**: Set doesn't allow duplicates elements. But other features can vary depending on the implementing class like HashSet doesn't preserve order but LinkedHashSet and TreeSet does.

- **Queue**: A Java Queue is a collection that holds elements before performing any operation. Here Objects are added to the tail and removed from the head and the object added first will be removed
 first.

- **Map**: Map, as said does not extend *collection* Interface so represent separately. Boxes in blue are the classes that are used for programming. It contains Key-Value pair. Key will be unique.
 No class extending Map preserves the order accept TreeMap class.

> -- [JAVA COLLECTION FRAMEWORK - Java User Group Albania](https://jugalbania.wordpress.com/2018/01/09/java-collection-framework/){:target="_blank"}

### List

ArrayList vs LinkedList:

<table>
    <thead>
        <tr>
            <th>ArrayList</th>
            <th>LinkedList</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ArrayList internally uses a "dynamic array" to store the elements</td>
            <td>LinkedList internally uses a "doubly inked list" to store the elements</td>
        </tr>
        <tr>
            <td>Manipulation with ArrayList is "slow"
             because it internally uses an array. If any
             element is removed from the array, all the bits
             are shifted in memory.</td>
            <td>Manipulation with LinkedList is "faster"
             than ArrayList because it uses a doubly
             linked list, so no bit shifting is required
             in memory.</td>
        </tr>
        <tr>
            <td>An ArrayList class can "act as a list" only
             because it implements List only.</td>
            <td>LinkedList class can "act as a list and
             queue" both because it implements list
             and Deque interfaces.</td>
        </tr>
        <tr>
            <td>ArrayList is "better for storing and
             accessing" data.</td>
            <td>LinkedList is "better for manipulating"
             data</td>
        </tr>     
    </tbody>
</table>

> -- [Difference between ArrayList and LinkedList - javatpoint](https://www.javatpoint.com/difference-between-arraylist-and-linkedlist){:target="_blank"}

### Set

A `Set` is a `Collection` that cannot contain duplicate elements. It models the mathematical set abstraction. The `Set` interface contains *only* methods inherited from `Collection` and adds the
 restriction that duplicate elements are prohibited. `Set` also a stronger contract on the behavior of the `equals` and `hashCode` operations, allowing `Set` instances to be compared meaningfully
 even if their implementation types differ. Two `Set` instances are equal if they contain the same elements.

The Java platform contains three general-purpose `Set` implementations: `HashSet`, `TreeSet`, and `LinkedHashSet`. `HashSet`, which stores its elements in a hash table, is the best-performing
 implementation; however it makes no guarantees concerning the order of iteration. `TreeSet`, which stores its elements in a red-black tree, orders its elements based on their values; it is
 substantially slower than `HashSet`. `LinkedHashSet`, which is implemented as a has table with a linked list running through it, orders its elements based on the Order in which they where inserted
 into the set (insertion-order). `LinkedHashSet` spares its clients from the unspecified, generally chaotic ordering provided by `HashSet` at a cost that is only slightly higher.

### Map

A `Map` is an object that maps keys to value. A map cannot contain duplicate keys: Each key can map to at most one value. It models the mathematical *function* abstraction. The `Map` interface
 includes methods for basic operations (such as `put`, `get`, `remove`, `containsKey`, `containsValue`, `size`, and `empty`), bulk operations (such as `pullAll` and `clear`), and collection views
 (such as `keySet`, `entrySet` and values).

The Java platform contains three general-purpose `Map` implements: `HashMap`, `TreeMap`, and `LinkedHashMap`. Their behavior and performance are precisely analogous to `HashSet`, `TreeSet`, adn
 `LinkedHashSet`, as described in [The Set Interface](#set) section.

## Algorithms

## Ordering

---