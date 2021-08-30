---
layout: post
title: Spring Boot - Configuration
category: spring-boot
tags: [spring-boot]
---

## Configuration Metadata

- XML
- Annotation
- Java Class

## XML

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
  <import resource="resource1.xml" />
  <bean id="bean1" class=""></bean>
  <bean id="bean2" class=""></bean>
  <bean name="bean2" class=""></bean>

  <alias alias="bean3" name="bean2"/>
  <import resource="resource2.xml" />
</beans>
```

- `<beans>` 是 Spring 配置文件的根节点。
- `<bean>` 用来定义一个 JavaBean。`id` 属性是它的标识，在文件中必须唯一；`class` 属性是它关联的类。
- `<alias>` 用来定义 Bean 的别名。
- `<import>` 用来导入其他配置文件的 Bean 定义。这是为了加载多个配置文件，当然也可以把这些配置文件构造为一个数组（new String[] {“config1.xml”, config2.xml}）传给 ApplicationContext 实现类进行加载多个配置文件，
  那一个更适合由用户决定；这两种方式都是通过调用 Bean Definition Reader 读取 Bean 定义，内部实现没有任何区别。`<import>` 标签可以放在 `<beans>` 下的任何位置，没有顺序关系。

## Annotation

### Register

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

### Annotation & Description

- `@Required`:
   - 注解只能用于修饰 bean 属性的 setter 方法。受影响的 bean 属性必须在配置时被填充在 xml 配置文件中，否则容器将抛出`BeanInitializationException`。
   - `@Required` is only for validation it will not auto wire anything
- `@Autowired`:
   - 注解可用于修饰属性（properties, also known field）、setter 方法、构造方法。
   - By default, Spring resolves `@Autowired` entries **by type**. If more than one bean of the same type is available in the container, the framework will throw `NoUniqueBeanDefinitionException`.
   - To resolve this conflict, we need to tell Spring explicitly which bean we want to inject. (`@Qualifier`)
- `@Qualifier`: 当有多个相同类型的bean时，`@Qualifier`批注与`@Autowired`一起使用可以通过指定要连接的确切bean来消除混乱。
- JSR 250 注解:
   - 包括`@Resource`, `@PostConstruct`, `@PreDestroy`
   - `@Resource` does auto-wiring **byName** by default.
- JSR 330 注解: `@Inject`。(SR 330’s `@Inject` annotation can be used in place of Spring’s `@Autowired` annotation in the examples included in this section.)

Ref: 
- [java - @Required not auto-wiring byName bydefault - Stack Overflow](https://stackoverflow.com/questions/50966984/required-not-auto-wiring-byname-bydefault){:target="_blank"}
- [Guide to Spring @Autowired \| Baeldung](https://www.baeldung.com/spring-autowire){:target="_blank"}

### Classpath Scanning and Managed Components

使用各式各样的annotation:
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

为了让 Spring 识别这个定义类为一个 Spring 配置类，需要用到两个注解：`@Configuration`和`@Bean`。

### @Bean

> Annotating a class with the `@Configuration` indicates that the class can be used by the Spring IoC container as a source of bean definitions.

用`@Configuration`注释类表示该类可以被Spring IoC容器用作Bean定义的源。

@Bean 的修饰目标只能是方法或注解。

@Bean 只能定义在`@Configuration`或`@Component`注解修饰的类中

```
@Configuration
public class AnnotationConfiguration {
    private static final Logger log = LoggerFactory.getLogger(JavaComponentScan.class);

    @Bean
    public Job getPolice() {
        return new Police();
    }

    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(AnnotationConfiguration.class);
        ctx.scan("org.zp.notes.spring.beans");
        ctx.refresh();
        Job job = (Job) ctx.getBean("police");
        log.debug("job: {}, work: {}", job.getClass(), job.work());
    }
}

public interface Job {
    String work();
}

@Component("police")
public class Police implements Job {
    @Override
    public String work() {
        return "抓罪犯";
    }
}
```

### @Configuration

> The `@Bean` annotation tells Spring that a method annotated with `@Bean` will return an object that should be registered as a bean in the Spring application context.

@Bean注释告诉Spring使用`@Bean`注释的方法将返回一个对象，该对象应在Spring应用程序上下文中注册为Bean

Ref: [Spring - Java Based Configuration - Tutorialspoint](https://www.tutorialspoint.com/spring/spring_java_based_configuration.htm){:target="_blank"}

`@Configuration`是一个类级别的注解，用来标记被修饰类的对象是一个**BeanDefinition**。

```
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

---