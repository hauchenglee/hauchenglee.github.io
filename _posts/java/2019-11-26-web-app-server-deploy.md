---
layout: post
title: Web App Server Deploy 部署
category: java
tags: [server]
---

## jar and war

- JAR: Java ARchive
- WAR: Web application ARchive

![](http://www.hauchenglee.com/assets/images/java/jar-war-ear.png)

<br>

From [Java Tips: Difference between ear jar and war files](https://www.java-tips.org/java-ee-tips-100042/17-enterprise-java-beans/1994-difference-between-ear-jar-and-war-files.html){:target="_blank"}:

These files are simply zipped files using java jar tool. These files are created for different purpose. 
Here is the description of those files:
- .jar files: The .jar files **contain the libraries, resources and accessories files** like property files.
- .war files: The war file **contains the web application** that can be deployed on the any servlet/jsp container.
The .war file **contains jsp, html, javascript** and other files for necessary for the development of web applications.
- .ear files: The .ear file contains the EJB modules of the applications.

Official Sun/Oracle description:
> - [The J2EE Tutorial: Web Application Archives](https://web.archive.org/web/20120626020019/http://java.sun.com/j2ee/tutorial/1_3-fcs/doc/WCC3.html){:target="_blank"}
> - [The Java Archive (JAR) File Format: The Basics](https://web.archive.org/web/20120626012843/http://java.sun.com/developer/Books/javaprogramming/JAR/basics){:target="_blank"}

Reference:
> - [Difference between jar and war in Java - Stack Overflow](https://stackoverflow.com/questions/5871053/difference-between-jar-and-war-in-java){:target="_blank"}

<br>

> 簡單來說：基本上都是壓縮檔案。war用於具有特定目錄結構的Web應用程序。

<br>

## convert to zip

In summary, **EAR, JAR, and WAR files are all simply zip files** that contain the various images, XML files,
property files and pieces of Java code that make up a Java application. If the .ear, .war or .jar extension or any of 
these files is changed to .zip, it can be opened with any standard decompression tool.

Tip: Some files have unique folder structure and requirements (e.g. some stuff needs to be in a `WEB-INF` directory).

> - [What are the differences between EAR, JAR and WAR files?](https://www.theserverside.com/feature/What-are-the-differences-between-EAR-JAR-and-WAR-files){:target="_blank"}
> - [What's the difference between the .war and .zip file formats? - Super User](https://superuser.com/questions/274229/whats-the-difference-between-the-war-and-zip-file-formats){:target="_blank"}

## naming war file

For popular servlet containers (JBoss, Tomcat, Jetty), WAR naming convention can drive context paths.
Name of the war becomes the context path if no explicit context path is defined anywhere.

```
a.war > localhost:8080/a
b.war > localhost:8080/b
```

> - [java - Rename war file to change the context path of an webapplication - Stack Overflow](https://stackoverflow.com/questions/33958635/rename-war-file-to-change-the-context-path-of-an-webapplication){:target="_blank"}

<br>

- JBoss

> For web applications that are deployed outside and EAR file, the **context root** can be specified in two ways. First, 
> the context root can be specified in the `WEB-INF/jboss-web.xml` file. The following example shows what the `jboss-
> web.xml` file would look like if it weren't bundled inside of an EAR file.
>
> ```
> <jboss-web>
>   <context-root>bank</context-root>
> ```
>
> Finally, if no **context root** specification exists, the context root will be **the base name of the WAR file**. For `web-
> client.war`, the context root would default to `web-client`. The only special case to this naming special name **ROOT**.

> - [Setting the context root of a web application](https://docs.jboss.org/jbossas/guides/webguide/r2/en/html/ch06.html){:target="_blank"}

<br>

- Tomcat

> When `autoDeploy` or `deployOnStartup` operations are performed by a Host, **the name and context path of the web application are derived from the name(s) of the file(s) 
> that define(s) the web application**. Consequently, the context path **may not** be defined in a `META-INF/context.xml` embedded in the application and there is a **close 
> relation** between the *context name*, *context path*, *context version* and the *base file name* (the name minus any `.war` or `.xml` extension) of the file.

> - [Apache Tomcat 7 Configuration Reference (7.0.96) - The Context Container](https://tomcat.apache.org/tomcat-7.0-doc/config/context.html#Naming){:target="_blank"}

<br>

- Jetty

> - If the WAR file is named myapp.war, then the context will be deployed with a context path of /myapp
> - If the WAR file is named ROOT.WAR (or any case insensitive variation), then the context will be deployed with a context path of /
> - If the WAR file is named ROOT-foobar.war (or any case insensitive variation), then the context will be deployed with a context path of / 
>   and a virtual host of "foobar"

Reference: [Chapter 5. Configuring Contexts](http://www.eclipse.org/jetty/documentation/current/configuring-contexts.html){:target="_blank"}

## webapp directory

![](http://www.hauchenglee.com/assets/images/java/webapp-intellij-directory.png)

<br>

![](http://www.hauchenglee.com/assets/images/java/webapp-directory-structure.png)

> - [Advanced Tutorial on Tomcat 7](https://www.ntu.edu.sg/home/ehchua/programming/howto/Tomcat_More.html){:target="_blank"}

<br>

- `webapp`：工程發佈文件夾，每個war包都可以視為`webapp`的壓縮包
- `META-INF`：
   - 用於存放工程自身相關的一些信息、元文件信息，通常由開發工具環境自動生成（配置清单文件）
   - 若要執行**jar**檔的元數據文件，此為必須。如果將jar中的`META-INF`文件夾刪除，那麼jar文件裡邊就沒有`MANIFEST.MF`文件，java-jar就找不到main class
- `WEB-INF`：
   - Java web應用的安全目錄，客戶端無法訪問，只有服務端可以訪問的目錄
   - 此目錄包含一個層次結構，**是webapp的必要配置信息**，沒有它無法運行webapp，以及jsp、servlet、和類的所有文件（e.g. classes, libs）
- `/WEB/classes`：存放程序所需要的所有Java class文件
- `/WEB/lib`：存放程序所需要的所有jar文件
- `/WEB/web.xml`：web應用的部署配置文件。它是工程中最重要的配置文件，它描述了servlet和組成應用的其他組件，以及應用初始化參數、安全管理約束等

<br>

p.s. `META-INF`文件的相關資料：[META-INF文件夹是干啥的，META-INF文件夹的作用， META-INF文件夹能删吗 - 逃离沙漠 - 博客园](https://www.cnblogs.com/demingblog/p/5653844.html){:target="_blank"}

## maven

### maven directory

maven項目的目錄結構：

![](http://www.hauchenglee.com/assets/images/java/maven-dirctory-structure.png)

source: [Maven Directory Structure - Dinesh on Java](https://www.dineshonjava.com/maven-directory-structure/){:target="_blank"}

注意上面帶粗體的目錄名，maven項目採用"約定優於配置"的原則，
- `src/main/java` 約定用於存放源代碼
- `src/main/test` 約定用於存放單元測試代碼
- `src/target` 約定用於存放編譯、打包後的輸出文件

> - [maven学习（上）- 基本入门用法 - 菩提树下的杨过 - 博客园](https://www.cnblogs.com/yjmyzz/p/3495762.html){:target="_blank"}

### maven fields

- groupId：是項目建立團隊或組織的唯一標識符，通常是域名倒寫，與artifactId被統稱為**坐標**，保證了項目的唯一性
- artifactId：定義了依賴jar包在**存儲庫**的地址，是這些jar包的**坐標**唯一標識符，也是一個項目將要產生的文件
- packaging：artifactId打包的方式，表示項目最終產生何種extension的打包文件，如`jar`、`maven-plugin`、`ejb`、`war`、`ear`、`rar`。如果沒有聲明packaging，則預設包裝為`jar`

Reference:
> - [Maven – POM Reference](http://maven.apache.org/pom.html#Maven_Coordinates){:target="_blank"}
> - [介绍maven的作用、核心概念(Pom、Repositories、Artifact、Goal)、用法、常用参数和命令以及简单故障排除、扩展及配置](https://www.trinea.cn/android/maven/){:target="_blank"}

---