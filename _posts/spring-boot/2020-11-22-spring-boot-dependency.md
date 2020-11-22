---
layout: post
title: Spring Boot - Dependency
category: spring-boot
tags: [spring Boot]
---

## Dependency Injection

依赖注入（DI）是一个过程，通过该过程，对象仅通过构造函数参数，工厂方法的参数或在构造或创建对象实例后在对象实例上设置的属性来定义其依赖关系（即，与它们一起工作的其他对象）。从工厂方法返回。然后，容器在创建bean时注入那些依赖项。从根本上讲，此过程是通过使用类的直接构造或服务定位器模式来控制bean自身依赖关系的实例化或定位的bean本身的逆过程（因此称为Control Inversion）。

使用DI原理，代码更简洁，当为对象提供依赖项时，去耦会更有效。该对象不查找其依赖项，并且不知道依赖项的位置或类。结果，您的类变得更易于测试，尤其是当依赖项依赖于接口或抽象基类时，它们允许在单元测试中使用存根或模拟实现。

DI存在两个主要变体：**基于构造函数的依赖注入**和**基于Setter的依赖注入**。

### Constructor-based

xml:

```
<bean id="txService" class="com.example.service.TxServiceImpl">
    <constructor-arg ref="accountDao"/>
    <constructor-arg ref="txDao"/>
    <constructor-arg ref="nextIdDao"/>
</bean>

<bean id="accountDao" class="com.example.dao.AccountDaoImpl">
    <constructor-arg ref="dataSource"/>
</bean>

<bean id="nextIdDao" class="com.example.dao.NextIdDaoImpl">
    <constructor-arg ref="dataSource"/>
</bean>

<bean id="txDao" class="com.example.dao.TxDaoImpl">
    <constructor-arg ref="dataSource"/>
</bean>
```

上面的配置可以转换为下面7到11行中的代码：

```
// Create the application from the configuration
ApplicationContext appContext = new ClassPathXmlApplicationContext("com/example/spring-config.xml");
 
// get from context to help define dataSource - we not do this 'manually'
javax.sql.DataSource dataSource = appContext.getBean("dataSource", javax.sql.DataSource.class );
 
AccountDao accountDao = new AccountDaoImpl(dataSource);
NextIdDao nextIdDao = new NextIdDaoImpl(dataSource);
TxDao txDao = new TxDaoImpl(dataSource);
 
TxService txService = new TxServiceImpl(txDao, accountDao, nextIdDao);
try {
    // Use the service
    txService.withdraw(new BigDecimal("100"), "Withdraw 100$", "5008");
} catch (InvalidParameterException | AccountNotFoundException | IllegalAccountTypeException | InsufficientFundsException ex) {
    System.out.println("Exception: " + ex.getMessage());
}
```

### Setter-based

```
<bean id="accountDao" class="com.example.dao.impl.AccountDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
 
<bean id="nextIdDao" class="com.example.dao.impl.NextIdDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
 
<bean id="txDao" class="com.example.dao.impl.TxDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
 
<bean id="txService" class="com.example.service.impl.TxServiceImpl">
    <property name="accountDao" ref="accountDao"/>
    <property name="nextIdDao" ref="nextIdDao"/>
    <property name="txDao" ref="txDao"/>  
</bean>
```

当然，要启用setter注入，我们必须在接口中（以及* Impl类中的实现）具有setter：

```
import javax.sql.DataSource;
 
public interface AccountDao {
    
    void setDataSource(DataSource dataSource);
 
    ...
}
```

```
import javax.sql.DataSource;
 
public interface NextIdDao {
    
    void setDataSource(DataSource dataSource);
    
    ...
}
```

```
import javax.sql.DataSource;
 
public interface TxDao {
    
    void setDataSource(DataSource dataSource);
 
    ...
}
```

```
import com.example.dao.AccountDao;
import com.example.dao.NextIdDao;
import com.example.dao.TxDao;
...
 
public interface TxService {
    
    void setTxDao(TxDao txDao);
 
    void setAccountDao(AccountDao accountDao);
 
    void setNextIdDao(NextIdDao nextIdDao);
 
    ...
}
```

上面的XML配置可以转换为以下7到19行中的代码：

```
// Create the application from the configuration
ApplicationContext appContext = new ClassPathXmlApplicationContext("com/dariawan/bankofjakarta/spring-config.xml");
 
// get from context to help define dataSource - we not do this 'manually'
javax.sql.DataSource dataSource = appContext.getBean("dataSource", javax.sql.DataSource.class );
 
AccountDao accountDao = new AccountDaoImpl();
accountDao.setDataSource(dataSource);
 
NextIdDao nextIdDao = new NextIdDaoImpl();
nextIdDao.setDataSource(dataSource);
 
TxDao txDao = new TxDaoImpl(dataSource);
txDao.setDataSource(dataSource);
 
TxService txService = new TxServiceImpl();
txService.setAccountDao(accountDao);
txService.setNextIdDao(nextIdDao);
txService.setTxDao(txDao);
 
try {
    // Use the service
    txService.withdraw(new BigDecimal("100"), "Withdraw 100$", "5008");
} catch (InvalidParameterException | AccountNotFoundException | IllegalAccountTypeException | InsufficientFundsException ex) {
    System.out.println("Exception: " + ex.getMessage());
}
```

### Compare

`constructor`案例：
- 强化强制依赖：这符合POJO构造函数的黄金法则；强制依赖关系最好在构造函数方法中声明和初始化。
- 促进不变性：将依赖项分配给最终字段，不适用于`setter`。这是为了避免错误地“重新设置”此依赖关系。
- 简洁的程序用法：就代码的“可读性”和对象关联而言，它更干净。创建和注入在一行代码中。

`setter`的案例
- 允许可选的依赖项和默认值：我们可以在`setter`中设置您的可选依赖项，也可以有可以在`setter`中覆盖的默认值。例如，在具有将默认日志写入文件的库中，设置器可以替换写入到写入控制台或数据库的不同实现的日志。
- 自动继承
- 循环引用：如果对象A和B相互依赖；A取决于B，反之亦然。在创建对象A和B时，Spring会引发`ObjectCurrentlyInCreationException`，因为只有在创建对象B之后才能创建对象A，反之亦然。
  因此spring可以通过`setter`注入解决循环依赖性。在调用`setter`方法之前先构造对象。在编程实践中，循环引用是不好的，必须避免。
- 具有描述性名称：属性“名称”必须出现在元素“属性”上。（Attribute `name` must appear on element `property`.）

Ref: [Constructor vs Setter Dependency Injection \| Dariawan](https://www.dariawan.com/tutorials/spring/constructor-vs-setter-dependency-injection/){:target="_blank"}

## Dependency Lookup



## Autowiring Collaborators

表2.自动装配模式（Autowiring modes）

- `no`: (Default) No autowiring. Bean references must be defined by `ref` elements. 
  Changing the default setting is not recommended for larger deployments, 
  because specifying collaborators explicitly gives greater control and clarity. 
  To some extent, it documents the structure of a system.
- `byType`: Autowiring by property name. 
  Spring looks for a bean with the same name as the property that needs to be autowired. 
  For example, if a bean definition is set to autowire by name and it contains a `master` property (that is, it has a `setMaster(..)` method), 
  Spring looks for a bean definition named master and uses it to set the property
- `byName`: Lets a property be autowired if exactly one bean of the property type exists in the container. 
  If more than one exists, a fatal exception is thrown, 
  which indicates that you may not use `byType` autowiring for that bean. 
  If there are no matching beans, nothing happens (the property is not set).
- `constructor`:

上面copy自官方文档，太长不看。直接上code：

By Name: 通过Bean的id或者name

`<bean name="employee" class="com.example.bean.Employee" autowire="byName">`

By Type: 按Bean的Class的类型

`<bean name="employee" class="com.example.bean.Employee" autowire="byType">`

Constructor:

`<bean name="employee" class="com.example.bean.Employee" autowire="constructor">`

Default:

`<bean name="employee" class="com.concretepage.bean.Employee" autowire="default">`

---