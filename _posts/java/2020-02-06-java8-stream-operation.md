---
layout: post
title: Java 8 - Stream Operation 操作
category: java
tags: [java]
comments: true
toc: true
---

## Operations

- Intermediate operations
   - Stream.filter(): to filter all elements of the stream
   - Stream.map(): converts each element into another object via the given function
   - Stream.sorted(): returns a sorted view of the stream
- Terminal operations
   - Stream.forEach(): iterating over all elements of a stream and perform some operation on each of them
   - Stream.collect(): receive elements from a stream and store them in a collection
   - Stream.match(): check whether a certain predicate matches the stream
   - Stream.count(): returning the number of elements in the stream as a long
   - Stream.reduce(): performs a reduction on the elements of the stream with the given function (like concat or merge two thing together)

## Short-circuit operations

所謂短路操作，就是符合條件的運算後，就中斷操作，如同if-else一樣。

- Stream.anyMatch()
- Steam.findFirst()

## Convert streams to collections

> Please note that it is not a true conversation. It's just collecting the elements from the stream into a collection or array.

請注意，這不是真正的轉換。它只是將流中的元素收集到集合或數組中。

- Convert Stream to List:
`Stream.collect(Collections.toList())`
- Convert Stream to array:
`Stream.toArray(EntryType[]::new)`

## Reference

- [Java 8 Stream - Java Stream Example - HowToDoInJava](https://howtodoinjava.com/java8/java-streams-by-examples/){:target="_blank"}

---