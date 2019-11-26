---
layout: post
title: Web App Server Path 路徑關聯
category: tech
tags: [server]
---

## System server Path

<table>
    <thead>
        <tr>
            <th></th>
            <th>絕對路徑</th>
            <th>相對路徑</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>定義</td>
            <td>文件真正存在的路徑，以根目錄為基準，進行查找文件</td>
            <td>文件相對存在的路徑，以當前文件為基準，進行查找文件</td>
        </tr>
        <tr>
            <td>表示</td>
            <td>C:/User/Document</td>
            <td>../ 當前文件所在目錄的上級目錄</td>
        </tr>
        <tr>
            <td></td>
            <td>http://hauchenglee.com/tech</td>
            <td>./ 表示當前文件所在目錄（可以省略）</td>
        </tr>
        <tr>
            <td></td>
            <td>http://www.sun.com/index.htm</td>
            <td>/ 表示當前站點的根目錄</td>
        </tr>
    </tbody>
</table>

<br>

相對路徑example：

```
C:\AppServer\azole\ a.txt
C:\AppServer\azole\images\ b.txt
C:\AppServer\azole\auto\ c.txt
C:\AppServer\azole\auto\save\ d.txt
```

a 想要引用 b → 
- 前方文件夾資源，調用后方文件夾資源：相同層級直接寫，不管之後資料夾
- 后方文件夾資源，調用前方文件夾資源：相同層級點幾層，外加前檔資料夾

refer|path
---|---
a → b|images\ b.txt
a → c|auto\ c.txt
a → d|auto\save\ d.txt
b → a|..\ a.txt
b → c|..\auto\ c.txt
b → d|..\auto\save\ d.txt
c → a|..\ a.txt
c → b|..\images\ b.txt
c → d|save\ d.txt
d → a|..\..\ a.txt
d → b|..\..\images\ b.txt

<br>

> 相对虚拟目录
>
> 在这个例子里，background属性的值为“/img/bg.jpg”，注意在“img”前有一个“/”字符。这个“/”代表的是虚拟目录的根目录。
> 1. **E:\book\网页布局\代码**设为虚拟目录，那么“/img/bg.jpg”的真实路径为**E:\book\网页布局\代码**\img\bg.jpg”；
> 2. **E:\book\网页布局\代码\第2章**设为虚拟目录，那么“/img/bg.jpg”的真实路径为**E:\book\网页布局\代码\第2章**\img\bg.jpg”
>
> -- [相对路径和绝对路径的区别 - 夏雪冬日 - 博客园](https://www.cnblogs.com/heyonggang/archive/2013/03/01/2938984.html){:target="_blank"}

<br>

在路徑中使用通配符（Wildcard）：
- [使用通配符](https://www.ibm.com/support/knowledgecenter/zh/SSFKSJ_8.0.0/com.ibm.wmqfte.doc/wildcards.htm){:target="_blank"}
- [windows - Two asterisks in file path - Stack Overflow](https://stackoverflow.com/questions/8532929/two-asterisks-in-file-path){:target="_blank"}
- [Spring：/ **和/ *与路径的区别 - 代码日志](https://codeday.me/bug/20170909/69684.html){:target="_blank"}
- [/lib、/lib/**/ 两个路径有什么差别 · Ruby China](https://ruby-china.org/topics/6497){:target="_blank"}

## Web App path

一個請求URI是由三個部份所組成，可以使用`HttpServletRequest`的`getRequestURI()`來取得：

`requestURI = contextPath + servletPath + pathInfo`<br>

也就是：<br>
`http://hostname.com/contextPath/servletPath/pathInfo`

<br>

下圖筆記出自**吳東錚**【MVC的環境部署、程式轉移路徑和規則-筆記分享】

![](http://www.hauchenglee.com/assets/images/tech/dong-web-app-path.jpg)

### server path

一個重點：相對虛擬目錄`/`代表的就是應用web app的名稱！

有一種特殊情況，在`webapp`的`WEB-INF`文件下有jsp，然後servlet要跳轉到jsp，就一定要這樣寫（`test`是應用名）：

`request.getRequestDispatcher("WEB-INF/jsp/first.jsp").forward(request, response);`

或者

`request.getRequestDispatcher("/test/WEB-INF/jsp/first.jsp").forward(request, response);`

看以下代碼：

```
//内部跳转
//绝对路径
request.getRequestDispatcher("/WEB-INF/jsp/first.jsp").forward(request, response);

//相对路径
request.getRequestDispatcher("WEB-INF/jsp/first.jsp").forward(request, response);
```

### browser path

比如，我們的應用url是：`http://192.168.xxx.xxx:8080/test`

那站點的路徑就是：`http://192.168.xxx.xxx:8080/` -- 很明顯`test`是應用名

對客戶端的瀏覽器而言，它認識的只有站點，也就是說我們的頁面，`/`代表的是站點名，所以在`/`後需要添加上我們應用的名稱才能正確的訪問到。

理解：
<br>
一個tomcat下可以部署多個應用，就相當於一個站點多個應用，怎麼區別不同的應用呢，當然是用不同的應用名。
這個需要和上面的servlet內部配置路徑區分理解，因為因為servlet本身就部署在服務端，在站點內部了，內部處理肯定知道它是站點下的哪個應用內；
但對於外部而言，只知道一個站點IP。

頁面主要表現以下三個地方：

- form action: 
   - `<form action="/test/helloworld" method="post">`：應用名是`test`，`/`代表站點的url`http://192.168.xxx.xxx:8080/`，
     所以`/test`代表`http://192.168.xxx.xxx:8080/test`
   - `<form action="helloworld">`：這就是相對路徑，相對於`test`應用名

- refer resource: `<script type="text/javascript" src="${pageContext.request.contextPath}/pages/event/js/addevent.js"></script>`

- jsp/servlet redirect: 只要和頁面打交道的，`/`都代表站點名，需要在後面添加應用名以區分是哪個應用的請求

```
// 絕對路徑
request.getRequestDispatcher("/user/a.jsp);

// 相對路徑
request.getRequestDispatcher("a.jsp);
```

其絕對地址就是：`http://192.168.0.1/webappname/user/a.jsp`

Reference:
- [Servlet的相对路径与绝对路径的详解 - 重心开始，重新开始 - CSDN博客](https://blog.csdn.net/qq_33642117/article/details/51851433){:target="_blank"}
- [JAVA 取得当前目录的路径/Servlet/class/文件路径/web路径/url地址 - 拂晓风起-Kenko - 博客园](https://www.cnblogs.com/kenkofox/archive/2011/04/13/2014419.html){:target="_blank"}

### api

- getRequestURL()：http://localhost:8080/project/folder/file.do
- getRequestURI()：/project/folder/file.do
- getQueryString()：arg1=value1&name=snoopy
- getContextPath()：/project
- getServletPath()：/folder/file.do
- getServerName()：localhost
- getRemoteAddr()：request IP
- getHeader("referer")：return request header

---