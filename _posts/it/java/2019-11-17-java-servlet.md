---
layout: post
title: Java - Servlet
category: java
tags: [java]
---

## What is Servlet

標準的Java程式，可以在服務器端的[Web Container](https://hauchenglee.github.io/it/java/2019/11/20/web-app-server-compare.html)所提供的環境內執行。
- Servlet：在服務器端執行的小程序（Java程序）
- JSP：在HTML文件中加入JSP元素（文件擴展名jsp）

Links:
- [Servlet/JSP实战教程：搭建博客系统 - 天码营](https://course.tianmaying.com/servlet-and-jsp){:target="_blank"}
- [Servlet第二篇【Servlet调用流程图、Servlet细节、ServletConfig、ServletContext】 - 后端 - 掘金](https://juejin.im/entry/5a60a40df265da3e2e628cf4#toc_4){:target="_blank"}

## Servlet Container

Servlet容器是[Web Container](https://hauchenglee.github.io/it/java/2019/11/20/web-app-server-compare.html)和Servlet進行通訊的主要構件，它的主要職責包括：
- 管理Servlet程序的生命週期
- 將URL映射到指定的Servlet進行處理
- 與Servlet程序合作處理HTTP請求，根據HTTP請求生成`HttpServletResponse`對象並傳給Servlet進行處理，將`HttpServletResponse`對象生成的內容返回給瀏覽器
- 並發請求的多線程處理、線程池管理
- Session管理，HTTP緩存等

將這些公共的任務抽象到Servlet容器這個構件中，也有利於開發者專注於業務邏輯
<br>
（也就是Servlet處理請求的具體響應方法，`doGet()`，`doPost()`）

> Servlet的`service()`方法的两个参数`HttpServletRequest`和`HttpServletResponse`正是对HTTP请求和响应的封装，Servlet接受请求后，可以进行响应的处理

## Handle HTTP Servlet

### Request and Response

請求：
- 一般文字輸入

```
request.setCharacterEncoding("UTF-8");
String username = request.getParameter("username");
String password = request.getParameter("password");
```

- 單選框

```
<input type="radio" name="gender" value="male"/>Official Only
<input type="radio" name="gender" value="female"/>All

String lang = request.getParameter("gender"); // Can be null, "male" or "female"

// form encoding: <name>=<value>
// gender=male
```

- 複選框

```
<input type="checkbox" name="lang" value="java" checked>Java
<input type="checkbox" name="lang" value="python" checked>Python

String[] values = request.getParameterValues("remember-me");

// form encoding: <name>=<value>&<name>=<value>&<name>=<value>...
// lang=java&lang=python
```

回應：
- return html
```
response.setContentType("text/html; charset=UTF-8")
response.getWrite().append("Hello World")
```

- return binary
```
response.setContentType("image/gif")
response.getOutputStream().write()
```

- redirect url
```
response.sendRedirect("/index.html);
```

- send error
```
response.sendError(404, "Resource Not Found");
```

### URL Data

通過不同的用戶輸入（HTTP請求中的參數區分），返回不同的內容，這樣就能實現了對頁面的動態化。

假設請求的URL為`/add`，則我們訪問`localhost:8080/add?a=1&b=2`時，讓頁面返回`a`參數與`b`參數之和。

```
@WebServlet("/add")
public class Add extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 拿到key后在后台搜索包含了关键字的内容，然后返回
        String key = request.getParameter("key");
            
		int a = Integer.parseInt(request.getParameter("a"));
		int b = Integer.parseInt(request.getParameter("b"));
		
		response.setCharacterEncoding("UTF-8");
		response.getWriter().append(a + b + "");
	}
}
```

同理，如果我們希望獲取一篇特定博客的內容，就可以把博客`id`作為請求參數，在Servlet中獲取`id`參數後，從後台（比如數據庫中）查詢到該篇博客並進行返回。

### Form Data

在Web應用中，用戶除了訪問Web應用獲取信息，還會主動創造信息，這種交互是通過HTML表單來完成的。在瀏覽器用戶填寫表單，通過HTTP通訊協議傳送到服務器，相應的Servlet獲取數據並處理，最後將結果返回給瀏覽器。

表單標籤（`<form>`），在HTML中負責接受用戶輸入，比如用戶註冊的文本框、密碼框，以及填寫信息時需要的數字、郵件、選擇框等等。

用戶登入的表單HTML代碼如下：
```
<form class="form-signin" action="./login" method="post">
  <h2 class="form-signin-heading">user login</h2>
  <input type="text" name="username" class="form-control" placeholder="user name" required autofocus>
  <input type="password" name="password" class="form-control" placeholder="password" required>
  <div class="checkbox">
    <label>
      <input type="checkbox" name="remember-me" value="on"> Remember me
    </label>
  </div>
  <button class="btn btn-primary btn-block" type="submit">submit</button>
</form>
```

Servlet處理表單請求：
```
String username = req.getParameter("username");
String password = req.getParameter("password");
String[] values = req.getParameterValues("remember-me");

if (vakyes != null && values[0].isEmpty()) {
    // do remember-me
} 
```

## Servlet Life Cycle

1. init()：當Servlet第一次被容器加載進入內存後調用，一般用於載入一些特定的資源和配置
2. service()：處理HTTP請求的邏輯（`doGet()`，`doPost()`）
3. destroy()：Servlet被銷毀時調用，一般用來釋放、清理資源

## Servlet is Singleton

**為什麼Servlet是單例的**

瀏覽器多次對Servlet的請求，一般情況下，*服務器只創建一個Servlet對象*，也就是說，Servlet對象一旦創建了，就會駐留在內存中，為後續的請求做服務，知道服務器關閉。

<br>

**每次訪問請求對象和響應對象都是新的**

對於每次訪問請求，Servlet引擎都會創建一個新的`HttpServletRequest`請求對象和一個新的`HttpServletResponse`響應對象，然後將這兩個對象作為參數傳遞給它調用的Servlet的`service()方法`，
`service()`方法再根據請求方式分別調用`doGet()`、`doPost()`方法。

<br>

**線程安全問題**

當多個用戶訪問Servlet的時候，*服務器會為每個用戶創建一個線程*。當多個用戶並發訪問Servlet共享資源的時候，就會出現線程安全問題。

(待查證是否singleton)

- [java - Is servlet the singleton? - Stack Overflow](https://stackoverflow.com/questions/11820840/is-servlet-the-singleton){:target="_blank"}
- [java - Is HttpServlet Singleton? - Stack Overflow](https://stackoverflow.com/questions/39940844/is-httpservlet-singleton){:target="_blank"}
- [Interview question: can you make a servlet as singleton? (Servlets forum at Coderanch)](https://coderanch.com/t/550362/java/Interview-servlet-singleton){:target="_blank"}
- [Are servlets Singleton classes? - Quora](https://www.quora.com/Are-servlets-Singleton-classes){:target="_blank"}

## Servlet URL Mapping

- 路徑關聯: [Web App Server Path](https://hauchenglee.github.io/it/java/2019/11/25/web-app-server-path.html)
- forward & redirect: [你知道Forward和Redirect的原理和區別嗎 - 每日頭條](https://kknews.cc/code/zgebapg.html){:target="_blank"}

## ServletConfig

通過此對象可以讀取`web.xml`中配置的初始化參數。

現在問題來了，為什麼我們要把參數信息放到`web.xml`文件中呢？我們可以直接在程序中都可以定義參數信息，搞到`web.xml`文件中又有什麼好處呢？

因為能夠讓你的程序更加靈活，更換需求，更改配置文件`web.xml`即可，程序代碼不用改。

## ServletContext

當Tomcat啟動的時候，就會創建一個ServletContext對象，它代表著當前web站點。

ServletContext的用途？
1. ServletContext既然代表著當前web站點，那麼**所有Servlet都共享一個Servlet對象**，所以**Servlet之間可以通過ServletContext實現通訊**
2. ServletConfig獲取的是配置單個Servlet的參數信息，**ServletContext可以獲取的是配置整個web站點的參數信息**
3. **利用ServletContext讀取web站點的資源文件**
4. 實現Servlet的轉發（用ServletContext轉發不多，主要用request轉發）

- 這是Demo2的代碼

```
// get ServletContext object
ServletContext servletContext = this.getServletContext();

String value = "peter";

// use myName as keyword and use value to save the data (like map collection)
servletContext.setAttribute("myName", value);
```

- 這是Demo3的代碼

```
ServletContext servletContext = this.getServletContext();

// get value by keyworld
String value = (String) servletContext.getAttribute("myName");

System.out.println(value)
```

訪問Demo3可以獲取Demo存儲的信息，從而實現多個Servlet之間通訊

---