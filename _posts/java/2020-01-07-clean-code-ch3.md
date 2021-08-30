---
layout: post
title: Clean Code 無暇的程式碼- Ch3 函數
category: java
tags: [design-pattern]
---

## 【每個函數只有一層抽象概念】

- 抽象概念高層次：`getHTML();`
- 抽象概念中層次：`String pagePathName = PathParser.render(pagePath);`
- 抽象概念低層次：`string.append("\n")`

一個函數擁有混合層次的抽象概念，總是令人困惑，無法分辨某個表達式（expression）是一個基本概念還是一個細節。

## 變量與參數

零參數 > 一個參數 > 兩個參數（合理的順序） > 三個參數 > 全局變量

### 禁止使用全局變量

> In my opinion using global variables is a worse practice than more parameters irrespective of the qualities you described. 
> My reasoning is that more parameters may make a method more difficult to understand, 
> but global variables can cause many problems for the code including poor testability, concurrency bugs, and tight coupling.

在我看來，不管您描述的質量如何，使用全局變量比使用更多參數都更糟糕。
我的理由是，更多的參數可能會使方法更難以理解，但是全局變量會導致代碼出現許多問題，包括可測試性差，並發性錯誤和緊密耦合。

Ref: [programming practices - Clean Code: Functions with few parameters - Software Engineering Stack Exchange](https://bit.ly/2Qu63gE){:target="_blank"}

### 避免布爾值參數

禁止傳值`Boolean` -> 將布爾值往外提取，拆分兩個函數

```
// Bad:
doSth(boolean isExist) { }

// Good:
if true -> doExist() { };
if false -> doDisappear() { };
```

### 多個參數

同形態參數可以包裝成類別

```
// Bad:
Circle makeCircle(double x, double y, double radius);

// Good:
Circle makeCircle(Point center, double radius);
```

### 輸出型參數

輸出型參數 & 輸入型參數：
- 輸入型參數：這個參數的值已知，由外面傳給函數裡使用
- 輸出型參數：這個參數的值未知，要通過函數傳遞出來

Ref:
- [输入型参数和输出型参数 - 代码先锋网](https://www.codeleading.com/article/2191827214/){:target="_blank"}
- [输入型参数与输出型参数 - 简书](https://www.jianshu.com/p/0818daa21a6f){:target="_blank"}

<br>

原則：盡量避免修改參數

第一個好處：可讀性高。

> Output arguments are counterintuitive. Readers expect arguments to be inputs, not outputs. 
> If your function must change the state of something, have it change the state of the object it is called on.
>
> Ref: [Robert C. Martin's Clean Code Tip of the Week #11: Output Arguments are Counterintuitive](http://www.informit.com/articles/article.aspx?p=1382188){:target="_blank"}

輸出參數違反直覺。讀者期望參數是輸入，而不是輸出。如果您的函數必須更改某些對象的狀態，請讓它更改被調用對象的狀態。

<br>

第二個好處：將參數對象設定為不可變，可以避免多線程的線程安全問題。

> It is also good practice to favor having immutable objects. 
> Immutable objects cause far fewer problems in most aspects of development (such as threading). 
> So by returning a new object, you are not forcing the parameter to be mutable.
>
> Ref: [java - Passing a parameter versus returning it from function - Stack Overflow](https://bit.ly/2Qt65oX){:target="_blank"}

<br>

輸出型參數：

```
user changePassword(String pwd, User user) {
    user.password = pwd;
    return user;
}
```

更好的作法是：

`output = someMethod(input)`

當然，像`Collections.sort`這種輸出型參數是可以接受的，比起將每個對象都聲明一個`sort()`來的方便。

```
Collections.sort(myList)

// or

myList.sort();
```

如果排序不使用輸出型參數：

`sortedList = sort(myList);`

Ref:
- [java - What is an output argument, as refered to in Martin's Clean Code? - Software Engineering Stack Exchange](https://bit.ly/37thiwP){:target="_blank"}

<br>

More Ref:
- [CA1021: Avoid out parameters - Visual Studio - Microsoft Docs](https://docs.microsoft.com/en-us/visualstudio/code-quality/ca1021?view=vs-2019){:target="_blank"}
- [Is it generally considered bad practice to use method input parameters as output parameters as well? : java](https://bit.ly/2sl5A8B){:target="_blank"}
- [refactoring - Always avoid in-out parameters in Java? - Stack Overflow](https://bit.ly/2Qwxvdo){:target="_blank"}

## 確認函數意義

### 單一職責

函數內部不要暗度陳倉，偷偷做多餘事情，造成不可預期的改變

```
public boolean checkPassword(String username, String userpassword) {
    User user = dao.findByname(username);
    user != null;

    user.password.equal(userpassword) {
        Session.initialize(); // bad way!
        return true;
    }
}
```

### 指令查詢分離

不好的作法：

```
public boolean set(String username, String value) {
    // return if setting value to username is success
}

if (set("username", "person")) { }
```

將布爾值與後續操作分開，改成

```
if (usernameExist("username")) {
    set("username", "person");
}
```

<br>

第二個例子：

```
boolean tryParse(string, out) {
    if string exist -> then string.append(out) -> return true (success);
    else return false (failure);
}
```

根本原因是TryParse方法負責解析整數，並告訴您是否可以將字符串解析為整數，
這既違反了命令/查詢原則（Command/Query），又違反了單一職責原則（Single Responsibility principles）。

好的寫法：

```
if (string.exist()) {
    out = tryParse(string);
}
```

Ref: [Real World TDD](http://paytonrules.com/software-development/2014/10/03/output-arguments.html){:target="_blank"}

### 避免多個返回值

> If you need to return more than one value, the method is likely doing more than one thing. 
> If the data is tightly related, then it would probably benefit from a class that holds both values.

如果您需要返回多個值，則該方法可能要做的不只是一件事情。如果數據緊密相關，那麼它可能會受益於包含兩個值的類。

Ref: [c# - What's wrong with output parameters? - Stack Overflow](https://bit.ly/2ZyhO9P){:target="_blank"}

## 異常處理

- 使用`Enum`的錯誤代碼容易造成耦合度高，不易擴展後續異常需求
- 改用`try-catch`取代回傳錯誤碼
- 提取`try-catch`到外層，內部不用關心處理異常，只要專心處理業務邏輯

## 其它

- 將判斷變量化

```
if (pageData.hasAttribute("Test")) {
    // do sth
}
```

```
boolean isTestPage = pageData.hasAttribute("Test");
if (isTestPage) {
    // do sth
}
```

---
