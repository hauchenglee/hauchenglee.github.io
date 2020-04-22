---
layout: post
title: Design Pattern 設計模式
category: java
tags: [java]
---

## 設計模式六大原則

1.單一職責原則：不要存在多於一個導致類變更的原因。通俗的說，即一個類只負責一項職責。
2.里氏替換原則：子類可以擴展父類的功能，但不能更改父類內置的功能。
3.依賴倒置原則：高層模塊不應該應該依賴低層模塊，兩個都應該依賴其抽象，抽像不應該依賴細節，細節應該依賴抽象。
4.接口隔離原則：建立單一接口，不要建立龐大臃腫的接口，盡量細化接口，接口中的方法盡量少。
5.迪米特原則：一個類對象應該對其他對象保持最少的了解，盡量降低類與類之間的替換，例如使用中介的類來發生聯繫。
6.開閉原則：當軟件需要變化時，應該通過擴展軟件實體的行為來實現變化，而不是通過修改現有的代碼來實現變化（對擴展開放，對修改關閉的原則）。

遵循設計模式前面5大原則，以及使用23種設計模式的目的就是遵守開閉原則。

Ref:
- [设计模式六大原则](http://www.uml.org.cn/sjms/201211023.asp){:target="_blank"}
- [设计模式详解（总纲） - 左潇龙 - 博客园](https://www.cnblogs.com/zuoxiaolong/p/pattern1.html){:target="_blank"}

## 24 種設計模式

![](http://www.hauchenglee.com/assets/images/java/design-pattern-gof-analysis.png)



## Reference

Overview:
 
- [设计模式大杂烩（24种设计模式的总结以及学习设计模式的几点建议） - 左潇龙 - 博客园](https://www.cnblogs.com/zuoxiaolong/p/pattern26.html){:target="_blank"}
- [Java设计模式：23种设计模式全面解析（超级详细）](http://c.biancheng.net/design_pattern/){:target="_blank"}
- [设计模式 \| 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html){:target="_blank"}
- [前言 · GitBook](https://xiaoyureed.github.io/my/e_book/design_pattern/){:target="_blank"}
- [重构与设计模式](https://refactoringguru.cn/){:target="_blank"}

Pattern:

- 代理模式：
   - [我的Java設計模式代理模式 \| 程式前沿](https://bit.ly/2RHVpnH){:target="_blank"}
- 裝飾器模式：
   - [设计模式——装饰器模式 - 掘金](https://juejin.im/post/5add8e9cf265da0b9d77d377){:target="_blank"}
   - [java设计模式－装饰器模式(Decorator) - 简书](https://www.jianshu.com/p/d80b6b4b76fc){:target="_blank"}
- 迭代器：
   - [23种设计模式（13）：迭代器模式_Java_三级小野怪的专栏-CSDN博客](https://blog.csdn.net/zhengzhb/article/details/7610745){:target="_blank"}

---