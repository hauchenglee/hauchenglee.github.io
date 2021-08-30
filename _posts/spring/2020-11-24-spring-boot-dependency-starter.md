---
layout: post
title: Spring Boot - Dependency Starter
category: spring-boot
tags: [spring-boot]
---

## starter-tomcat vs starter-web

簡言之，`spring-boot-starter-web`包含`spring-boot-starter-tomcat`

下面是`spring-boot-starter-web`依賴關係層次結構：

![](https://www.hauchenglee.com/assets/images/spring/spring-boot/spring-boot-starter-web-hierarchy.png)

如果要排除`spring-boot-starter-tomcat`依賴：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

Ref: [spring-boot-starter-tomcat vs spring-boot-starter-web - Stack Overflow](https://bit.ly/2SIn5tS){:target="_blank"}

---