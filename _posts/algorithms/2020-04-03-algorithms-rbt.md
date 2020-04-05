---
layout: post
title: Algorithms - Reb Black Tree 紅黑樹
category: algorithms
tags: [algorithms]
---

## 紅黑樹

rules:
- every node has a color either red or black
- root of tree is always black
- no red-red patent-child
- every path from root to a null node has same number of black nodes

## 時間複雜度



## 插入

![](http://www.hauchenglee.com/assets/images/algorithms/rbt-z's-relationship.png)

rules:
1. If empty tree: create black root node
2. Insert new leaf node as **<span style="color:red">red</span>**
   1. If parent is black:
      1. then done
   2. If parent is red:
      1. if **black** or **absent** sibling: rotate, recolor and done
      2. if **red** sibling: recolor (parent and sibling and grandparent node (if grandparent node is not root: recolor)) and check again

check:
- line: child and grandparent are both on the same line
- triangle: child and grandparent are on different way

strategy:
- z is root: recolor
- z uncle is red: recolor (parent and sibling and grandparent node) and check again
- z uncle is black (line): move z patent to top position and rotate grandparent to bottom position, and recolor origin parent and grandparent
- z uncle is black (triangle): 
   - <: left rotation to line (RR in line) and then right rotation and recolor
   - \>: right rotation to line (RR in line) and then right rotation and recolor

Code:
- [interview/RedBlackTree.java at master · mission-peace/interview](https://github.com/mission-peace/interview/blob/master/src/com/interview/tree/RedBlackTree.java){:target="_blank"}

## 刪除



## Reference

Video:
- [![](http://img.youtube.com/vi/5IBxA-bZZH8/0.jpg)](http://www.youtube.com/watch?v=5IBxA-bZZH8)
- [![](http://img.youtube.com/vi/UaLIHuR1t8Q/0.jpg)](http://www.youtube.com/watch?v=UaLIHuR1t8Q)

Article:
- [Red/Black Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html){:target="_blank"}
- [3.1 红黑树 \| 编程之法：面试和算法心得](https://wizardforcel.gitbooks.io/the-art-of-programming-by-july/content/03.01.html){:target="_blank"}
- [红黑树(一)之 原理和算法详细介绍 - 如果天空不死 - 博客园](https://www.cnblogs.com/skywang12345/p/3245399.html){:target="_blank"}

---