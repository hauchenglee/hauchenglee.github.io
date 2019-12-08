---
layout: post
title: Web Server & App Server Compare 兩者比較
category: tech
tags: [server]
---

## Compare between Web Server and Application Server

In fact, this is Religious War: different between web server and app server.

<table>
    <thead>
        <tr>
            <th></th>
            <th>Web Server</th>
            <th>Application</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>protocol</td>
            <td>HTTP</td>
            <td>not limited to HTTP</td>
        </tr>
        <tr>
            <td>static or dynamic</td>
            <td>static</td>
            <td>dynamic</td>
        </tr>
        <tr>
            <td>main job</td>
            <td>handle web requests (http)</td>
            <td>in charge of the logic</td>
        </tr>
    </tbody>
</table>

<br>

For example:
- Web Server:
   - Apache HTTP Server
   - Nginx
- Application Server:
   - Apache Tomcat
   - Oracle WebLogic Server
   - JBoss (now WildFly)
   - GlassFish

<br>

我比較認同的文章：
> - [What is the difference between application server and web server? - Stack Overflow](https://stackoverflow.com/questions/936197/what-is-the-difference-between-application-server-and-web-server){:target="_blank"}

另一個觀點的文章（主要以能不能實現EJB支持當作分別）：
> - [5 Difference between Application Server and Web Server in Java](https://javarevisited.blogspot.com/2012/05/5-difference-between-application-server.html){:target="_blank"}

<br>

web container：
> - [java ee - Difference between web server, web container and application server - Stack Overflow](https://stackoverflow.com/questions/12689910/difference-between-web-server-web-container-and-application-server){:target="_blank"}

```
servlet container = web container <= web server != application server
```

<br>

## Tomcat is Web Server or Application Server?

如果依照上面[第一個](https://stackoverflow.com/questions/936197/what-is-the-difference-between-application-server-and-web-server){:target="_blank"}的結論，
tomcat應該歸類在app server，這是以功能為分類依據（因為可以運行應用程序服務器），當然也有人認為應該算是web server，因為tomcat沒有嚴格符合J2EE標準規範（無法部署ear文件）

<br>

不過這篇對於tomcat server有更清楚的論述：

> - [Is Tomcat An Application Server Or A Web Server?](https://javapipe.com/blog/tomcat-application-server/){:target="_blank"}

上述文章所說：

> Going by the above-mentioned details, it follows that strictly speaking Tomcat should be referred to as Tomcat web server or a JavaServer/Servlet container
> since there are certain conditions and services of a commercial JEE Application server that it doesn’t offer its users.
> 
> However, it does cover for these faults by including the most widely used services and supports add-ons as well as plug-ins which make server enhancement quite easy.
> The main advantage of this server, therefore, lies in its architecture which allows users to leave out what they don’t need, use what they need and install what they may be lacking.
>
> Because of this, Tomcat is often used as an application server for strictly web-based applications
> even though it doesn’t include the entire suite of capabilities that a standard Java EE application would have on offer.

<br>

> 通过上述细节，可以得出结论，严格来说，Tomcat应该被称为Tomcat Web服务器或JavaServer / Servlet容器，因为商业JEE应用服务器在某些条件和服务下没有为其用户提供服务。
>
> 但是，它通过包括使用最广泛的服务来覆盖这些故障，并支持使服务器增强非常容易的附加组件和插件。
> 因此，该服务器的主要优点在于其体系结构，该体系结构允许用户忽略不需要的内容，使用所需的内容并安装可能缺少的内容。
>
> 因此，Tomcat通常被用作严格基于Web的应用程序的应用程序服务器，尽管它不包括标准Java EE应用程序所提供的全部功能。

<br>

> Whichever the case, the truth though is that even though Tomcat cannot be technically defined as an application
> server, it is continuously and successfully being used as an application server for millions of mission-critical
> applications on a daily basis. The jury is still out there.

> 无论哪种情况，事实是，即使从技术上不能将Tomcat定义为应用程序服务器，它仍每天被连续且成功地用作数百万个
> 关键任务应用程序的应用程序服务器。陪审团还在那里。

<br>

提供這個鏈接（[Why is Tomcat a Webserver and not an Application Server? - DZone Java](https://dzone.com/articles/why-tomcat-webserver-and-not){:target="_blank"}），這篇文章的**留言**部分，
 我比較認同以下留言的觀點：

- > 米哈尔·科蒂：
  ><br>
  > 我不同意你的解释。应用程序服务器是可以运行您的应用程序的服务器。就这么简单。

- > 萨钦·瓦里亚（Sachin Walia）：
  ><br>
  > 哇！这是最愚蠢的文章。作者甚至都不在乎定义他认为应用服务器与Web服务器之间的关系。我们还活在2001年吗？不敢相信这是在2014年编写的。这是公司近来使用的一些应用服务器：
  >
  > 播放框架，Akka + Spray，Tomcat 8，Netty。这些不是应用程序服务器吗？JEE是陈旧的。人们不再关心它，而转向Spring和所有现代框架。

- > 迈克尔·迈耶：
  ><br>
  > Tomcat可以同时充当Web和应用程序服务器。本文对此进行了很好的解释：
  ><br>
  >https://javapipe.com/tomcat-application-server

<br>

補充額外知識：

> - [关于J2EE和Spring目前到底是怎样的关系，以及未来这两者的发展是怎样的，是否存在竞争市场的情况？ - 知乎](https://www.zhihu.com/question/268742981/answer/341770209){:target="_blank"}
> - [Understanding Web Applications, Servlets, and JSPs](https://docs.oracle.com/cd/E14571_01/web.1111/e13712/basics.htm#WBAPP117){:target="_blank"} - 對於servlet container的官方說明

---