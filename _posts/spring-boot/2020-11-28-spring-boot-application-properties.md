---
layout: post
title: Spring Boot - Application Properties
category: spring
tags: [spring-boot]
---

## 配置 application.properties

- [Spring Boot Application Properties Example - Examples Java Code Geeks - 2020](https://examples.javacodegeeks.com/enterprise-java/spring/boot/spring-boot-application-properties-example/)

```properties
spring.profiles.active=dev

## URL user and password
spring.datasource.url = jdbc:mysql://127.0.0.1:3306/nobibi?useSSL=false&useLegacyDatetimeCode=false&serverTimezone=Asia/Taipei&characterEncoding=utf8
spring.datasource.username = root
spring.datasource.password = password

## Hibernate Properties
# The SQL dialect makes Hibernate generate better SQL for the chosen database
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect

## JPA/Hibernate
spring.jpa.show-sql=true

## Hibernate ddl auto (create, create-drop, validate, update)
# Difference between create and update is that, update will update your database
# if database tables are already created and will create if database tables are not created.
#
# And create will create tables of database. And if tables are already created then it will drop all the tables
# and again create tables. In this case your data would be lost.
#
# It is my personal advise to use update.
# https://stackoverflow.com/questions/16788068/hibernate-make-database-only-if-not-exists
spring.jpa.hibernate.ddl-auto=update

# https://www.javainuse.com/spring/boot-jwt
# Spring security
```

## 读取 application.properties

- [Spring Boot读取配置的几种方式](https://mp.weixin.qq.com/s/aen2PIh0ut-BSHad-Bw7hg)
- [Spring Boot application.properties file - Dev in Web](http://dolszewski.com/spring/spring-boot-application-properties-file/)

## 打包 application.properties

active:
- [Activating Spring Boot profile with Maven profile - Dev in Web](http://dolszewski.com/spring/spring-boot-properties-per-maven-profile/)
- [通過maven profile 打包指定環境配置 - IT閱讀](https://www.itread01.com/content/1546503853.html)
- [Spring boot 使用profile完成不同环境的maven打包功能_duan9421的专栏-CSDN博客](https://blog.csdn.net/duan9421/article/details/79086335)

spring boot active:
- [java - Spring Boot: spring.profiles.active=dev/test/prod - Stack Overflow](https://bit.ly/386R6sB)

---