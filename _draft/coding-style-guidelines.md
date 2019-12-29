---
layout: post
title: MVC 架構的二三事
category: ?
tags: [?]
---

## Coding Guidelines

- [Table of Contents - Alibaba-Java-Coding-Guidelines](https://alibaba.github.io/Alibaba-Java-Coding-Guidelines/){:target="_blank"}
- [p3c/阿里巴巴Java开发手册（华山版）.pdf at master · alibaba/p3c](https://bit.ly/3583DcE){:target="_blank"}

### 變量

- [工程实践：如何给变量取一个好的名字 - Matrix海子 - 博客园](https://www.cnblogs.com/dolphin0520/p/10639167.html){:target="_blank"}
- [if后面只有一句话，该不该加大括号？ - 程序园](http://www.voidcn.com/article/p-cltduhop-kq.html){:target="_blank"}
- [java - Check whether a string is not null and not empty - Stack Overflow](https://bit.ly/2MFiJQz){:target="_blank"}

## MVC

### 總原則

- 將任何異常（例如`NullPointerException`）、字段無效（例如`email.invalid`）、業務不合法（例如`user.inactivated`）的判斷，阻斷在外層，進入到下一層必定是正確的代碼。
- 同上，發現任何異常、無效、不合法的判斷，及早拋出、中斷執行，讓外層回傳結果一定是正確的。

### Global

- 系統設置

### Controller

- 處理任何的`HttpRequest`或是`HttpResponse`的邏輯，校驗此連線是否合法，有無異常，不處理業務邏輯
- Controller层不会信任页面的校验，所以页面做过的校验都会再做一次，它要对service层负责
- controller層接收的應該是完全沒加工的，或是已經加工完畢，可以直接回傳前端的資訊

```
if (balance == null ) {
  return HttpStatusCode(400);
}
```

### Service

- service层要对dao层负责，它就需要做业务逻辑的校验，比如添加的用户数据库中是否已存在，更新的数据数据库中有没有等等

```
assert balance > 0
```

Ref: [邏輯歸何處](https://ingramchen.io/blog/2015/02/logic_where.html){:target="_blank"}

### Manager

- 对第三方平台封装的层，预处理返回结果及转化异常信息
- 对 Service 层通用能力的下沉，如缓存方案、中间件通用处理
- 与 DAO 层交互，对多个 DAO 的组合复用。

### Bean

- 如果是一些基础验证，可以用`Spring Hibernate Validation`
- 不要在`getter`、`setter`寫業務邏輯
- [不要在POJO寫預設值](https://stackoverflow.com/questions/21509150/default-boolean-value-in-java){:target="_blank"}

### Dao

- SQL寫在這

Ref: [sql是不是可以写在service层？虽然service是业务层_y_dzaichirou的博客-CSDN博客](https://blog.csdn.net/y_dzaichirou/article/details/53673528){:target="_blank"}

### Reference

- [Java Web应用的代码分层最佳实践。 - 知乎](https://zhuanlan.zhihu.com/p/36558426){:target="_blank"}
- [service层需要数据校验嘛？ - 知乎](https://www.zhihu.com/question/60246780){:target="_blank"}
- [SpringMVC数据（参数）验证放在controller层还是service层？ - 知乎](https://www.zhihu.com/question/342471474){:target="_blank"}

## 異常處理

### 原則

- 方法的返回值可以为null，不强制返回空集合，或者空对象等，必须添加注释充分说明什么情况下会返回null值。（阿里巴巴）
- 
- 異常三原則：
   - 具体明确：盡量縮小try catch範圍，定位清楚可能的異常位置
   - 提早抛出：不应该用异常控制流程
   - 延迟捕获：传递一个危险信号，需要让调用方知道
- 不抛出异常情况：
   - 本方法没有能力处理的异常，调用方有能力处理，抛出是框架层面的选择
   - 在service层抛出异常后，必须在controller或更高层统一捕获处理，格式化后返回
- 好处：
   - Java的函数参数就是输入，返回值就是输出
   - 业务回滚，这样可以使用spring的业务注解Transaction来控制业务
坏处：
   - 异常不要用来做流程控制，条件控制
   - 异常设计的初衷是解决程序运行中的各种意外情况，且异常的处理效率比条件判断方式要低很多
    （但其實不用扣細節，系統內開銷最大的是IO和線程，處理好這兩個比起異常的效率是立竿見影的）

### Reference

異常處理：
- [Java异常处理原则与技巧总结 - John_nok - 博客园](https://www.cnblogs.com/mingorun/p/8900449.html){:target="_blank"}
- [优雅的处理你的Java异常 - 后端 - 掘金](https://juejin.im/entry/5b3c6f66f265da0f65236185){:target="_blank"}
- [Java web， service 层应该通过异常（自定义Exception）来中断业务吗？ - 知乎](https://www.zhihu.com/question/326779714){:target="_blank"}

Spring異常處理：
- [Exception Handling in Spring MVC](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc){:target="_blank"}
- [Exception Handling in Spring MVC](http://www.zhaojun.im/springboot-exception/){:target="_blank"}
- [Guide to Spring Boot REST API Error Handling - Toptal](https://www.toptal.com/java/spring-boot-rest-api-error-handling){:target="_blank"}

Spring
- [controller层 trycatch不影响service层抛出的异常_Summer_i的博客-CSDN博客](https://blog.csdn.net/Summer_i/article/details/83085283){:target="_blank"}

---