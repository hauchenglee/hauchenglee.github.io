---
layout: post
title: Network Layer 網絡七層模型
category: network
tags: [network]
---

## Network Layer

**OSI(Open System Interconnection)**:

<table>
    <thead>
        <tr>
            <th>TCP/IP</th>
            <th>OSI</th>
            <th>Function</th>
            <th>Protocol</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">應用層 Application</td>
            <td>應用層 Application</td>
            <td>文件傳輸，電子郵件，文件服務，虛擬終端</td>
            <td rowspan="3">DHCP，DNS，FTP，HTTP，SNMP，SMTP，SSH，Telnet</td>
        </tr>
        <tr>
            <td>表示層 Presentation</td>
            <td>數據格式化，代碼轉換，數據加密<br>解決不同系統之間的通信。e.g. Linux下的QQ和Windows下QQ可以通信</td>
        </tr>
        <tr>
            <td>會話層 <br>Session</td>
            <td>解除或建立與其他接點的聯繫</td>
        </tr>
        <tr>
            <td colspan="2">傳輸層 <br>Transport</td>
            <td>提供端對端的接口</td>
            <td>TCP，UDP</td>
        </tr>
        <tr>
            <td colspan="2">網絡層 <br>Network</td>
            <td>為數據包選擇路由</td>
            <td>IP，ICMP，IGMP，Router</td>
        </tr>
        <tr>
            <td colspan="2">數據鏈接層 <br>Data Link</td>
            <td>傳輸有地址的幀，錯誤檢測功能</td>
            <td>ARP，Ethernet，MAC，LLC，Switch，Bridge</td>
        </tr>
        <tr>
            <td colspan="2">物理層 <br>Physical</td>
            <td>以二進制數據形式在物理媒體上傳輸數據</td>
            <td>IEEE 802，Fiber，Wireless，Hub</td>
        </tr>
    </tbody>
</table>

<br>

![](https://hauchenglee.github.io/assets/images/network/osi-7-layer-model.png)

Ref: [OSI (Open System Interconnection) 7 Layer Model](http://www.howtocisco.com/ccna/ccna2.htm){:target="_blank"}

<br>

口訣：
1. All People Seem To Need Data Process（所有的人看來都需要資料處理）
1. All People Seem To Need Domino's Pizza（所有的人看來都需要達美樂比薩）    
1. Please Do Not Trust Sales People Always

Ref: [OSI七層參考模組.PDF](https://bit.ly/2VQpWRt){:target="_blank"}

Others:
- [互联网协议入门（一） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html){:target="_blank"}
- [What Is The OSI Model? \| Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/){:target="_blank"}

---