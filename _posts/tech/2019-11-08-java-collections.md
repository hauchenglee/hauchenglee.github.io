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
            <th>Inteface</th>
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

- **List**: List is ordered means preserve the order, but not sorted. It allows duplicate elements and null as well. Elements can be accessed by using the index number,
 indexing starts from zero.

- **Set**: Set doesn't allow duplicates elements. But other features can vary depending on the implementing class like HashSet doesn't preserve order but LinkedHashSet
 and TreeSet does.

- **Queue**: A Java Queue is a collection that holds elements before performing any operation. Here Objects are added to the tail and removed from the head and the object
 added first will be removed first.

- **Map**: Map, as said does not extend *collection* Interface so represent separately. Boxes in blue are the classes that are used for programming. It contains Key-Value
 pair. Key will be unique. No class extending Map preserves the order accept TreeMap class.

> -- [JAVA COLLECTION FRAMEWORK - Java User Group Albania](https://jugalbania.wordpress.com/2018/01/09/java-collection-framework/)

### List

ArrayList vs LinkedList:



### Set

### Map

## Algorithms

## Comparable and Comparator

---