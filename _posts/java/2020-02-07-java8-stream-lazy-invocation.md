---
layout: post
title: Java 8 - Stream Lazy Invocation 延遲調用
category: java
tags: [java]
comments: true
toc: true
---

## Lazy

兩個重點：
- 中間操作（intermediate operations）會被延遲執行，直到啟動終端操作（terminal operation）後才執行對源數據的計算
- 所有中間操作都是延遲性的，因此直到實際需要處理結果時才執行它們。（因此不同的中間操作順序會造成不同的執行次數）

## Reference

- [Java 8 Streams - Lazy evaluation](https://www.logicbig.com/tutorials/core-java-tutorial/java-util-stream/lazy-evaluation.html){:target="_blank"}

---