---
layout: post
title: Servlet筆記-Servlet
category: tech
tags: [servlet]
---

## Servlet 處理機制



> 在Java世界里，Servlet技术用来创建Web应用程序——本质上来说，Servlet是运行于服务器端的Java
> 程序，它能够接受客户端发起的HTTP请求并动态地生成页面内容。Servlet最初是对任意客户端-服务
> 端通讯协议的一层抽象，但在Web技术蓬勃发展的互联网时代，它几乎已经完全和HTTP通讯协议绑定
> 在一起使用，所以我们常用的术语Servlet——是"HTTP Servlet"的缩写。开发者可以基于Servlet在
> Java平台上开发动态Web应用程序，基于收到的HTTP请求生成响应内容，HTTP响应内容可以是纯文
> 本、HTML、XML、JSON格式的数据。
>
> Servlet API是Java EE规范的一部分，是Web开发中最常用到的部分。通常Servlet会应用在如下场景
> 中：
> - 处理浏览器页面中提交的HTML表单数据
> - 根据HTTP请求信息，动态生成HTTP响应内容
> - 使用Cookie或URL重写技术在无状态的HTTP协议之上实现客户端状态的管理
>
> -- [关于Servlet/JSP实战教程 | 天码营 - 实战开发技术学习服务平台](https://course.tianmaying.com/servlet-and-jsp+servlet#0){:target="_blank"}

---