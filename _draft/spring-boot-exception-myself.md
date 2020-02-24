---
layout: post
title: Spring Boot Exception 項目錯誤記錄總結
category: spring-boot
tags: [spring-boot]
---

## Failed to determine a suitable driver class

錯誤訊息：

```console
Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
	If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
```

可能發生的原因：

【Spring boot配置文件中引用錯誤的數據庫地址、url、drive class等】

解決

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

或是

```
@Configuration
public class DatasourceConfig {
    @Bean
    public DataSource datasource() {
        return DataSourceBuilder.create()
                .driverClassName("com.mysql.cj.jdbc.Driver")
                .url("jdbc:mysql://localhost:3306/myDb")
                .username("root")
                .password("pass")
                .build();
    }
}
```

如果項目不需要資料庫相關信息就排除此類的autoconfig

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class Application {
    public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
}
```

Ref:
- [Resolving Failed to Configure a DataSource Error \| Baeldung](https://www.baeldung.com/spring-boot-failed-to-configure-data-source){:target="_blank"}
- [hibernate - Spring Boot JPA MySQL : Failed to determine a suitable driver class - Stack Overflow](https://bit.ly/2SLtGUp){:target="_blank"}
- [SpringBoot啟動報錯Failed to configure a DataSource: 'url' . - 每日頭條](https://kknews.cc/code/j9pmome.html){:target="_blank"}

【錯誤的依賴影響引用工作】
   - 例如添加不合適的依賴，或是配置不正確，導致無法正確解析數據庫參數信息

> I had created a `BaseEntity` having JPA annotations in my commons module. The `commons` dependency was specified as
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
 	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

> Once I added the `<scope>provided</scope>`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
	<scope>provided</scope>
</dependency>
```

> The issue got resolved for me.

Ref:
- [Failed to determine a suitable driver class · Issue #13796 · spring-projects/spring-boot](https://github.com/spring-projects/spring-boot/issues/13796){:target="_blank"}

【使用mvn部署專案丟失文件】
   - IDEA不自动复制资源文件到编译目录classes的问题
   - 大佬的解决方案是：rebuild project

Ref:
- [springboot项目提示“Failed to determine a suitable driver class”错误_代码编程_积微成著](https://www.jiweichengzhu.com/article/e6cbb2d6aa7648f1b3046ba7e8580803){:target="_blank"}

【在maven添加`resources`造成的錯誤】

自行在maven測試發現如果在`<resources>`添加以下參數`<include>application-${activatedProperties}.properties</include>`，會造成無法在IDE內運行：

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
            <includes>
                <include>application-${activatedProperties}.properties</include>
            </includes>
        </resource>
    </resources>
</build>
```

原因不明。

解決辦法：移除掉`<include>application-${activatedProperties}.properties</include>`標籤，或是省略`<includes>`標籤。

---