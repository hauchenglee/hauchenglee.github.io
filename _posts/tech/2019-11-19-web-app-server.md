---
layout: post
title: Web Server and Application Server
category: tech
tags: [server]
---

[What is the difference between application server and web server? - Stack Overflow](https://stackoverflow.com/questions/936197/what-is-the-difference-between-application-server-and-web-server)

Most of the times these terms Web Server and Application Server are used interchangeably.

Following are some fo the key differences in features of Web Server and Application Server:

- Web Server is designed to server HTTP Content. App Server can also server HTTP Content but is not limited to just HTTP. It can be provided other protocol support such as RMI/RPC.
- Web Server is mostly designed to server static content, though most Web Servers have plugins to support scripting languages like Perl, PHP, ASP, JSP etc. through which these servers can generate
 dynamic HTTP content.
- Most of the application servers have Web Server as integral part of them, that means App Server can do whatever Web Server is capable of. Additionally App Server have components and features to
 support Application level services such as Connection Pooling, Object Pooling, Transaction Support, Messaging services etc.
- As web servers are well suited for static connect and app servers for dynamic content, most of the production environments have web server acting as reverse proxy to app server. That means while
 servicing a page request, static contents (such as images / static HTML) are served by web server that interprets the request. Using some kind of filtering technique (mostly extension of requested
 resource) web server identifies dynamic content request and transparently forwards to app server.

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


For example:
- Web Server:
   - Apache HTTP Server
   - Nginx
- Application Server:
   - Apache Tomcat
   - Oracle WebLogic Server
   - JBoss (now WildFly)
   - GlassFish

---