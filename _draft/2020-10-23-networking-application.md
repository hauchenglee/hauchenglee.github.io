---
layout: post
title: Networking - Application
category: tech
tags: [tech]
---

## Overview

Status:
- 無連接（connectionless）：客戶端和服務器僅在當前請求和響應期間相互了解
- 無狀態（stateless）：連接後忘記彼此，無法在跨網頁的不同請求之間保留信息
- 與媒體無關（media independent）：可以依據content-type發送任何類型的數據


URL:

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
            <td>schema</td>
            <td>//</td>
            <td>userinfo@</td>
            <td>host</td>
            <td>:port</td>
            <td>path</td>
            <td>?query</td>
            <td>#fragment</td>
        </tr>
        <tr>
            <td>https:</td>
            <td>//</td>
            <td>john.doe@</td>
            <td>www.example.com</td>
            <td>:8080</td>
            <td>/forum/questions/</td>
            <td>?name=val&name=val</td>
            <td>#top</td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

Request:

一個請求 URI 是由三個部份所組成，可以使用 HttpServletRequest 的 getRequestURI() 來取得：

```
requestURI = contextPath + servletPath + pathInfo
```



### Non-Persistent and Persistent Connections

### HTTP Message Format

### User-Server Interaction: Cookies

### Web Caching

### 

## Web and HTTP



## FTP



## SMTP



## DNS



## P2P



---