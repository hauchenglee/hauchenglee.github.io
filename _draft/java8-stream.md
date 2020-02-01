---
layout: post
title: Java 8 - Stream
category: java
tags: [java]
---

## Overview

### Oracle

**Package java.util.stream Description**

Classes to support functional-style operations on streams of elements, such as map-reduce transformations on collections. 
For example:

```
public static void main(String[] args) {
    int sum = widgets.stream()
            .filter(b -> b.getColor() == RED)
            .mapToInt(b -> b.getWeight())
            .sum();
}
```

Here we use widgets, a Collection<Widget>, as a source for a stream, and then perform a filter-map-reduce on the stream 
to obtain the sum of the weights of the red widgets. (Summation is an example of a reduction operation.)

Streams differ from collections in several ways:

- No storage. A stream is not a data structure that stores elements; instead, it conveys elements from a source such as a 
data structure, an array, a generator function, or an I/O channel, through a pipeline of computational operations.

不儲存元素。流不是存儲元素的數據結構。相反，它通過一系列計算操作從數據結構、數組、生成器功能或I/O流等源中傳遞元素。

- Functional in nature. An operation on a stream produces a result, but does not modify its source. For example, filtering 
a `Stream` obtained from a collection produces a result, but does not modify its source. For example, filtering 
elements from the source collection.

本質上是功能性的。對流的操作會產生結果，但不會修改其源。例如，對`Stream`從集合中獲取的元素，`Stream`進行過濾會產生一個不包含過濾元素的新元素，而不是從源集合中刪除元素。

- Laziness-seeking. Many stream operations, such as filtering, mapping, or duplicate removal, can be implemented 
lazily, exposing opportunities for optimization. For example, "find the first `String` with three consecutive vowels" need 
not examine all the input strings. Stream operations. Intermediate operations area always lazy.

延遲尋求。許多流操作（例如過濾，映射或重複刪除）可以延遲實施，從而提升優化的效能。例如，"`String`使用三個連續的元音查找第一個"不需要檢查所有輸入字符串。
流操作分為中間（產生`Stream`）操作和最終（產生值或副作用）操作。中間操作總是延遲的。

- Possibly unbounded. While collections have a finite size, streams need not. Short-circuiting operations such as 
`limit(n)` or `findFirst()` can allow computations on infinite streams to complete in finite time.

可能無界。儘管集合的大小是有限的，但流不是必需的。例如`limit(n)`或`findFirst()`的短路操作可以允許對無限流的計算在有限時間內完成。

- Consumable. The elements of a stream are only visited once during the life of a stream. Like an [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html){:target="_blank"}, a new 
stream must be generated to revisit the same elements of the source.

可消耗的。在流的生存期內，流的元素只能訪問一次。與`Iterator`相同，必須生成新的流以重新訪問源中的相同元素。

### Summary

- 不是數據結構（Not a data structure）
- 專為lambdas設計（Designed for lambdas）
- 不支持索引訪問（Do not support indexed access）
- 可以輕鬆輸出為數組或列表（Can easily be outputted as arrays or lists）
- 支持延遲訪問（Lazy access supported）
- 可並行化（Parallelizable）

Ref: [Java 8流-Java流示例-HowToDoInJava](https://howtodoinjava.com/java8/java-streams-by-examples/#ways_to_build_streams){:target="_blank"}

## Arch

1. Stream Creation
1. Stream Pipelines
1. Stream Operations
1. Lazy Invocation
1. Parallelism

## Reference

英文教程：
- [java.util.stream (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html){:target="_blank"}
- [Java 8 Stream - Java Stream Example - HowToDoInJava](https://howtodoinjava.com/java8/java-streams-by-examples/){:target="_blank"}
- [A Guide to Streams in Java 8: In-Depth Tutorial with Examples](https://stackify.com/streams-guide-java-8/){:target="_blank"}
- [The Java 8 Stream API Tutorial - Baeldung](https://www.baeldung.com/java-8-streams){:target="_blank"}

中文教程：
- [Java 8 中的 Streams API 详解](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/index.html){:target="_blank"}
- [[译] 一文带你玩转 Java8 Stream 流，从此操作集合 So Easy - 掘金](https://juejin.im/post/5cc124a95188252d891d00f2){:target="_blank"}
- [【翻译】Java 8 Stream API 教程 - 简书](https://www.jianshu.com/p/b464487ff844){:target="_blank"}

---