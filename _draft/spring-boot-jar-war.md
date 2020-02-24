---
layout: post
title: Spring Boot jar & war 文件
category: spring-boot
tags: [spring-boot]
---

## deployed by jar

這裡有完整的討論與官方鏈接
- [java - Differences between jar and war in Spring Boot? - Stack Overflow](https://bit.ly/2uR7dfv){:target="_blank"}

## deployed by war

因為Spring Boot預設有嵌入Tomcat，並且使用jar來部署環境，所以如果要使用war部署到web container，需要依照下列步驟：

步驟1：添加`starter-web`依賴

```xml
<dependencies>
    <dependency> 
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-web</artifactId> 
    </dependency> 
</dependencies>
```

步驟2：指定maven打包war：

```xml
<packaging>war</packaging>
```

步驟3：指定war文件名稱：

```xml
<build>
    <finalName>${artifactId}</finalName>
    ... 
</build>
```

或是使用新版的語法：

```xml
<build>
    <finalName>${project.artifactId}</finalName>
    ... 
</build>
```

原因：

```console
The expression ${artifactId} is deprecated. Please use ${project.artifactId} instead.
```

步驟4：啟動類繼承`SpringBootServletInitializer`並override`configure()`

```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {
  public static void main(String[] args) {
      SpringApplication.run(Application.class, args);
  }

  @Override
  protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
      return builder.sources(Application.class);
  }
}
```

如果沒有步驟4的繼承（extends）`SpringBootServletInitializer`與改寫（override）`configure`，就會出現錯誤：

```console
Spring Boot application gives 404 when deployed to Tomcat
```

關於此錯誤的討論：
- [java - Spring Boot application gives 404 when deployed to Tomcat but works with embedded server - Stack Overflow](https://bit.ly/328wCNI){:target="_blank"}

官方文件：
- [92. Traditional Deployment](https://docs.spring.io/spring-boot/docs/2.1.10.RELEASE/reference/html/howto-traditional-deployment.html){:target="_blank"}
- [“How-to” Guides](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-traditional-deployment){:target="_blank"}

部署教程：
- [Deploy a Spring Boot WAR into a Tomcat Server \| Baeldung](https://www.baeldung.com/spring-boot-war-tomcat-deploy){:target="_blank"}
- [How to make a Spring Boot Jar into a War to deploy on tomcat \| Software Developer Blog](https://mtdevuk.com/2015/07/16/how-to-make-a-spring-boot-jar-into-a-war-to-deploy-on-tomcat/){:target="_blank"}

`SpringBootServletInitializer`說明：
- [Spring Boot - SpringBootServletInitializer Examples](https://www.logicbig.com/how-to/code-snippets/jcode-spring-boot-springbootservletinitializer.html){:target="_blank"}
- [SpringBootServletInitializer Class in Spring Boot](https://www.javaguides.net/2019/01/springbootservletinitializer-class-in-springboot.html){:target="_blank"}
- [SpringBootServletInitializer tutorial - deploying Spring Boot application from WAR](http://zetcode.com/springboot/springbootservletinitializer/){:target="_blank"}

---