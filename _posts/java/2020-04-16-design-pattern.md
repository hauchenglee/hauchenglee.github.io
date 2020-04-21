---
layout: post
title: Design Pattern 設計模式
category: java
tags: [java]
---

## 设计模式六大原则

1. 单一职责原则：不要存在多于一个导致类变更的原因。通俗的说，即一个类只负责一项职责。
2. 里氏替换原则：子类可以扩展父类的功能，但不能改变父类原有的功能。
3. 依赖倒置原则：高层模块不应该依赖低层模块，二者都应该依赖其抽象，抽象不应该依赖细节，细节应该依赖抽象。
4. 接口隔离原则：建立单一接口，不要建立庞大臃肿的接口，尽量细化接口，接口中的方法尽量少。
5. 迪米特原则：一个类对象应该对其他对象保持最少的了解，尽量降低类与类之间的耦合，例如使用中介的类来发生联系。
6. 开闭原则：当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化（对扩展开放，对修改关闭的原则）。

遵循设计模式前面5大原则，以及使用23种设计模式的目的就是遵循开闭原则。

Ref:
- [设计模式六大原则](http://www.uml.org.cn/sjms/201211023.asp){:target="_blank"}
- [设计模式详解（总纲） - 左潇龙 - 博客园](https://www.cnblogs.com/zuoxiaolong/p/pattern1.html){:target="_blank"}

## 24 种设计模式

![](http://www.hauchenglee.com/assets/images/java/design-pattern-gof-analysis.png)


## Reference

Overview:
 
- [设计模式大杂烩（24种设计模式的总结以及学习设计模式的几点建议） - 左潇龙 - 博客园](https://www.cnblogs.com/zuoxiaolong/p/pattern26.html){:target="_blank"}
- [Java设计模式：23种设计模式全面解析（超级详细）](http://c.biancheng.net/design_pattern/){:target="_blank"}
- [设计模式 \| 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html){:target="_blank"}
- [前言 · GitBook](https://xiaoyureed.github.io/my/e_book/design_pattern/){:target="_blank"}

设计模式:

- 代理模式：
   - [我的Java設計模式代理模式 \| 程式前沿](https://bit.ly/2RHVpnH){:target="_blank"}
- 装饰器模式：
   - [设计模式——装饰器模式 - 掘金](https://juejin.im/post/5add8e9cf265da0b9d77d377){:target="_blank"}
   - [java设计模式－装饰器模式(Decorator) - 简书](https://www.jianshu.com/p/d80b6b4b76fc){:target="_blank"}
- 迭代器：
   - [23种设计模式（13）：迭代器模式_Java_三级小野怪的专栏-CSDN博客](https://blog.csdn.net/zhengzhb/article/details/7610745){:target="_blank"}


---