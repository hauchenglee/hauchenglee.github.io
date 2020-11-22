---
layout: post
title: Spring Boot - Configuration
category: spring-boot
tags: [spring Boot]
---

## Configuration Metadata

- XML
- Annotation
- Java Class

## Annotation

步骤1：在xml开启注解：

- 使用`<context:annotation-config/>`
- 使用`<context:component-scan>`

> `<context:annotation-config>` 用于激活已经在应用程序上下文中注册的bean中的注释（无论它们是使用XML定义还是通过包扫描定义的）。
>
> `<context:component-scan>`不仅可以执行`<context:annotation-config>`操作，`<context:component-scan>`还可以扫描软件包以在应用程序上下文中查找并注册Bean。
>
> Ref: [java - Difference between \<context:annotation-config> and \<context:component-scan> - Stack Overflow](https://bit.ly/332icjZ){:target="_blank"}

`<context:component-scan>`用法：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-3.0.xsd">
               
     <context:component-scan base-package="org.example"/>
     
</beans>
```

步骤2：使用各式各样的annotation
1. @component
2. @Controller
3. @Service
4. @Repository

## Java Class

基於Java類定義Bean配置元數據，其實就是通過Java類定義Spring配置元數據，且直接消除XML配置文件。

具體步驟如下：
1. 使用@Configuration註解需要作為配置的類，表示該類將定義Bean的元數據（使用@Configuration註解的類本身也是一個Bean）
2. 使用@Bean註解相應的方法，該方法名默認就是Bean的名稱，該方法返回值就是Bean的對象。
3. AnnotationConfigApplicationContext或子類進行加載基於java類的配置

Ref: [Spring bean配置的三种方式（XML、注解、Java类）_MrLee的博客-CSDN博客](https://blog.csdn.net/echizao1839/article/details/88063013){:target="_blank"}

换句话说，就是原本在xml配置的元数据，改成用Java类来配置。

### @Required and @Autowired



---