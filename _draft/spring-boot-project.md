---
layout: post
title: Spring Boot Project 項目總結
category: spring-boot
tags: [spring-boot]
---

## design pattern

api response design:
- [Spring MVC/Boot 统一异常处理最佳实践 - 赵俊的博客](http://www.zhaojun.im/springboot-exception/){:target="_blank"}
- [后端返回数据全局处理_我表示我来过-CSDN博客](https://blog.csdn.net/my_nice_life/article/details/79945279){:target="_blank"}
- [分页设计 · HTTP后台端：RESTful API接口设计](https://crifan.github.io/http_restful_api/website/restful_experience/pagination.html){:target="_blank"}

constants:
- [Constants in Java - The Anti-Pattern - DZone Java](https://dzone.com/articles/constants-in-java-the-anti-pattern-1){:target="_blank"}
- [java - What is the use of interface constants? - Stack Overflow](https://stackoverflow.com/questions/2659593/what-is-the-use-of-interface-constants){:target="_blank"}

## restful

- [前后端分离项目，接口返回 200 但是里面返回 500 合理吗？ - 知乎](https://www.zhihu.com/question/309888255){:target="_blank"}
- [数据库没有找到，restful的返回值statusCode应该是多少？ - 知乎](https://www.zhihu.com/question/310737821){:target="_blank"}
- [关于 RESTful API 中 HTTP 状态码的定义的疑问？ - 知乎](https://www.zhihu.com/question/58686782){:target="_blank"}

## ResponseEntity

- [使用spring ResponseEntity处理http响应_neweastsun的专栏-CSDN博客](https://blog.csdn.net/neweastsun/article/details/81142870){:target="_blank"}
- [When use ResponseEntity<T> and @RestController for Spring RESTful applications - Stack Overflow](https://bit.ly/3aojtE6){:target="_blank"}

## filter

- [补习系列(7)-springboot 实现拦截的五种姿势 - 美码师 - 博客园](https://www.cnblogs.com/littleatp/p/9496009.html){:target="_blank"}

## dao

- [@DynamicUpdate with Spring Data JPA - Baeldung](https://www.baeldung.com/spring-data-jpa-dynamicupdate){:target="_blank"}

## json

解決JSON lazy loading
- [json - spring rest lazy loading with hibernate - Stack Overflow](https://stackoverflow.com/questions/46190099/spring-rest-lazy-loading-with-hibernate){:target="_blank"}
- [hibernate - Converting Lazy Loaded object to JSON in spring boot jpa - Stack Overflow](https://stackoverflow.com/questions/55942356/converting-lazy-loaded-object-to-json-in-spring-boot-jpa){:target="_blank"}

## application.properties

- [Spring Boot application.properties file - Dev in Web](http://dolszewski.com/spring/spring-boot-application-properties-file/){:target="_blank"}

```properties
## Spring DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.url = jdbc:mysql://localhost:3306/test?useSSL=false&serverTimezone=UTC&useLegacyDatetimeCode=false
spring.datasource.username = root
spring.datasource.password = password


## Hibernate Properties

# The SQL dialect makes Hibernate generate better SQL for the chosen database
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect

# Hibernate ddl auto (create, create-drop, validate, update)
spring.jpa.hibernate.ddl-auto = update
```

- [Spring Boot读取配置的几种方式](https://mp.weixin.qq.com/s/aen2PIh0ut-BSHad-Bw7hg){:target="_blank"}
- [Spring Boot Application Properties Example - Examples Java Code Geeks - 2020](https://examples.javacodegeeks.com/enterprise-java/spring/boot/spring-boot-application-properties-example/){:target="_blank"}

---