---
layout: post
title: Networking - Application HTTP
category: tech
tags: [tech]
---

## Overview

Status:
- 無連接（connectionless）：客戶端和服務器僅在當前請求和響應期間相互了解
- 無狀態（stateless）：連接後忘記彼此，無法在跨網頁的不同請求之間保留信息
- 與媒體無關（media independent）：可以依據content-type發送任何類型的數據

## URL

URI, URL, URN:
- URI (Uniform Resource Identifier): itself
- URL (Uniform Resource Locator): address
- URN (Uniform Resource Name): name

<table>
    <thead>
        <tr>
            <th>scheme</th>
            <th colspan="4">authority</th>
            <th>path</th>
            <th>query</th>
            <th>fragment</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>https:</td>
            <td>//</td>
            <td>userinfo@</td>
            <td>localhost</td>
            <td>:8080</td>
            <td>/folder/path/</td>
            <td>?name=val<br>&name=val</td>
            <td>#top</td>
        </tr>
    </tbody>
</table>

- getRequestURL()：http://localhost:8080/project/folder/file.do
- getScheme(): http
- getServerName()：localhost
- **getContextPath()**：/project
- **getServletPath()**：/folder/file.do
- **getPathInfo()**: /file.do
- getRequestURI()：/project/folder/file.do
- getQueryString()：arg1=value1&name=snoopy
- getRemoteAddr()：return client’ IP
- getHeader(‘referer’)：return request header

<br>

服务器角度，【/】代表web应用程序的名称（web content）
- forward（因为是在服务器端解析的）
- web.xml（因为是在服务器端解析的）
- jsp type（因为jsp是服务器端程序）

浏览器角度，【/】代表于url根目录，后面追加应用名称来区分属于站点下的哪个应用
- `response.sendRedirect()`（因为只要和页面打交道的，“/”都代表站点名）
- html type（因为所有的html页面中的相对地址都是相对于服务器根目录）
- `<form action=””>`（理由同上）

服务器角度例外：
```
//绝对路径
request.getRequestDispatcher("/WEB-INF/jsp/first.jsp")
.forward(request, response);

//相对路径
request.getRequestDispatcher("WEB-INF/jsp/first.jsp")
.forward(request, response);
```

Ref:
- [Servlet的相对路径与绝对路径的详解 - 重心开始，重新开始 - CSDN博客](https://blog.csdn.net/qq_33642117/article/details/51851433){:target="_blank"}
- [JAVA 取得当前目录的路径/Servlet/class/文件路径/web路径/url地址 - 拂晓风起-Kenko](https://www.cnblogs.com/kenkofox/archive/2011/04/13/2014419.html){:target="_blank"}

## Non-Persistent and Persistent Connections

### Operation

**HTTP 1.0**:

Under HTTP 1.0, connections are not considered persistent unless a keep-alive header is included, although there is no official specification for how keepalive operates. 
It was, in essence, added to an existing protocol. If the client supports keep-alive, it adds an additional header to the request:

```
Connection: keep-alive
```

Then, when the server receives this request and generates a response, it also adds a header to the response:

```
Connection: keep-alive
```

Following this, the connection is not dropped, but is instead kept open. When the client sends another request, it uses the same connection. 
This will continue until either the client or the server decides that the conversation is over, and one of them drops the connection.

**HTTP 1.1**:

In HTTP 1.1, all connections are considered persistent unless declared otherwise. 
The HTTP persistent connections do not use separate keepalive messages, they just allow multiple requests to use a single connection.

### Advantages and Disadvantages

Advantages:
- 减少程序延迟：Reduced latency in subsequent requests (no handshaking).
- 减少系统消耗：Reduced CPU usage and round-trips because of fewer new connections and TLS handshakes.
- 减少网络拥塞：Reduced network congestion (fewer TCP connections).
- 具有流水线性质的传输：Enables HTTP pipelining of requests and responses.
- 可以回报错误而无需关闭连接：Errors can be reported without the penalty of closing the TCP connection.

Disadvantages:<br>
If the client does not close the connection when all of the data it needs has been received, the resources needed to keep the connection open on the server will be unavailable for other clients. 
How much this affects the server's availability and how long the resources are unavailable depend on the server's architecture and configuration.

### RTT

![](http://www.hauchenglee.com/assets/images/tech/300px-HTTP_persistent_connection.svg)

RTT (Round-Trip Time): RTT指的是，一个短分组从客户端到服务器，然后再返回客户端所用的时间。

RTT stands for the round-trip time taken for an object request and then its retrieval.
In other words, it is the time taken to request the object from the client to the server and then retrieve it from the server back to the client.

因为客户端和服务器建立TCP连接的时候，会通过一个三次握手的过程来交换传输控制信息。
三次握手的前两次占用了一个RTT，客户结合第三次握手通行会通过该连接发送一个HTTP请求报文，一旦该分组到达服务器，服务器便开始使用TCP传输HTML对象。
因此，粗略地说，响应时间是两个RTT加上传输HTML的时间（不是传播）

Ref: [HTTP persistent connection - Wikipedia](https://en.wikipedia.org/wiki/HTTP_persistent_connection){:target="_blank"}

## HTTP Message Format



## User-Server Interaction: Cookies



## Web Caching


## Ref

- [计算机网络之一：网络架构_ITPUB博客](http://blog.itpub.net/28624388/viewspace-2213969/){:target="_blank"}
- [学习笔记_王文萱的博客-CSDN博客](https://bit.ly/3os4DDr){:target="_blank"}
- [](){:target="_blank"}

---