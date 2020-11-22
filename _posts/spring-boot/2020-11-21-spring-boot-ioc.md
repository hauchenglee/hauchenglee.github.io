---
layout: post
title: Spring Boot - IoC
category: spring-boot
tags: [spring Boot]
---

## Overview

架构：
1. IoC容器
   1. What is Inversion of Control
   2. IoC Container
   3. Bean
2. 依赖
   1. 依赖注入
   2. 依赖查找
3. 容器配置
   1. XML
   2. Annotation
   3. Java class

## What is Inversion of Control

> IoC，是 Inversion of Control 的缩写，即控制反转。
>
> - 上层模块不应该依赖于下层模块，它们共同依赖于一个抽象
> - 抽象不能依赖于具体实现，具体实现依赖于抽象
>
> 注：又称为依赖倒置原则。这是设计模式六大原则之一。

IoC 不是什么技术，而是一种设计思想。在 Java 开发中，IoC 意味着将你设计好的对象交给容器控制，而不是传统的在你的对象内部直接控制。

如图：

![](http://www.hauchenglee.com/assets/images/spring-boot/container-magic.png)

或是我画的图：

![](http://www.hauchenglee.com/assets/images/spring-boot/spring-boot-core-ioc.png)

Ref: 
- [5. The IoC container](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html){:target="_blank"}

## IoC Container



## Bean

由 IoC 容器管理的那些组成你应用程序的对象我们就叫它 **Bean**。Bean 就是由 Spring 容器初始化、装配及管理的对象，除此之外，bean 就与应用程序中的其他对象没有什么区别了。

名词解释：
- \<ref\>: 写上对方Bean ID，代表我是小明，我写上扫地人的ID，由IoC帮我准备扫地人，并注入给我
- 下面是constructor注入

> So far we have only injected constructor and property values with static values, which is useful if you want to eliminate configuration files. 
> Values can also be injected by reference -- one bean definition can be injected into another. 
> To do this, you use the constructor-arg or property's ref attribute instead of the value attribute. 
> The ref attribute then refers to another bean definition's id.
>
> In the following example, the first bean definition is a java.lang.String with the id springMessage. 
> It is injected into the second bean definition by reference using the property element's ref attribute.

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="springMessage" 
          class="java.lang.String">
        <constructor-arg value="Spring is fun." />
    </bean>

    <bean id="message"
          class="org.springbyexample.di.xml.SetterMessage">
        <property name="message" ref="springMessage" />
    </bean>
</beans>
```

Ref: [4. Reference Injection](https://www.springbyexample.org/examples/intro-to-ioc-reference-injection.html){:target="_blank"}

---