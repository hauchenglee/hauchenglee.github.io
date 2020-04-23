---
layout: post
title: Network - OSI 網絡七層模型
category: tech
tags: [tech]
---

## Open System Interconnection

**OSI(Open System Interconnection)**:

OSI是一個開放性的通行系統互連參考模型，是一個協議規範。它把網絡協議從邏輯上分為了7層。每一層都有相關、相對應的物理設備。

OSI七層模型是一種框架性的設計方法 ，建立七層模型的主要目的是為解決異種網絡互連時所遇到的兼容性問題，其最主要的功能就是幫助不同類型的主機實現數據傳輸。

它的最大優點是將服務、接口和協議這三個概念明確地區分開來，通過七個層次化的結構模型使不同的系統不同的網絡之間實現可靠的通訊。

原文網址：[https://kknews.cc/tech/899lpbg.html](https://kknews.cc/tech/899lpbg.html){:target="_blank"}

<table>
    <thead>
        <tr>
            <th>OSI層</th>
            <th>功能</th>
            <th>TCP/IP協議</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>應用層 Application</td>
            <td>文件傳輸，電子郵件，文件服務，虛擬終端</td>
            <td>TFTP，HTTP，SNMP，FTP，SMTP，DNS，Telnet</td>
        </tr>
        <tr>
            <td>表示層 Presentation</td>
            <td>數據格式化，代碼轉換，數據加密</td>
            <td>沒有協議</td>
        </tr>
        <tr>
            <td>會話層 <br>Session</td>
            <td>解除或建立與其他接點的聯繫</td>
            <td>沒有協議</td>
        </tr>
        <tr>
            <td>傳輸層 <br>Transport</td>
            <td>提供端對端的接口</td>
            <td>TCP，UDP</td>
        </tr>
        <tr>
            <td>網絡層 <br>Network</td>
            <td>為數據包選擇路由</td>
            <td>IP，ICMP，RIP，OSPF，BGP，IGMP</td>
        </tr>
        <tr>
            <td>數據鏈接層 <br>Data Link</td>
            <td>傳輸有地址的幀，錯誤檢測功能</td>
            <td rowspan="2">IEEE 802</td>
        </tr>
        <tr>
            <td>物理層 <br>Physical</td>
            <td>以二進制數據形式在物理媒體上傳輸數據</td>
        </tr>
    </tbody>
</table>

![](http://www.hauchenglee.com/assets/images/tech/seven-layer-osi-model.png)

Ref: [seven-layer open systems interconnection (osi) model. this model of... \| Download Scientific Diagram](https://bit.ly/3arJykt){:target="_blank"}

![](http://www.hauchenglee.com/assets/images/tech/osi-7-layer-model.png)

Ref: [OSI (Open System Interconnection) 7 Layer Model](http://www.howtocisco.com/ccna/ccna2.htm){:target="_blank"}

口訣：
1. All People Seem To Need Data Process（所有的人看來都需要資料處理）
1. All People Seem To Need Domino's Pizza（所有的人看來都需要達美樂比薩）    
1. Please Do Not Trust Sales People Always

Ref:
- [OSI七層參考模組.PDF](https://bit.ly/2VQpWRt){:target="_blank"}
- [\[Day14\]OSI - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10205482){:target="_blank"}

### Physical

物理特性的鏈接，把傳輸的終端使用任何物理手段鏈接起來，例如光纜、電纜、無線波等方式。

*作用是負責傳送0、1的電信號。*

![](http://www.hauchenglee.com/assets/images/tech/physical-layer-connection.png)

### Data Link

單純的0和1沒有任何意義，必須規定解讀方式：多少個電信算一組？每個信號位有何意義？

*可以理解為：規定了0和1的分包形式，確定了網絡數據包的形式。*

此層存在兩個子層：
- Media Access Control (MAC): 负责控制网络中的设备如何获得对介质的访问以及传输数据的权限。
（responsible for controlling how devices in a network gain access to a medium and permission to transmit data.）
- Logical Link Control (LLC): 负责控制网络中的设备如何获得对介质的访问以及传输数据的权限
（responsible for identifying and encapsulating network layer protocols, and controls error checking and frame synchronization.）

<br>

【Ethernet】

- [Network - Ethernet](http://www.hauchenglee.com/tech/2020/04/19/network-ethernet.html){:target="_blank"} - 以太網

【MAC】

- [Network - MAC](http://www.hauchenglee.com/tech/2020/04/20/network-mac.html){:target="_blank"} - MAC

【Broadcast】

- [](){:target="_blank"}

【Switch】

- [](){:target="_blank"}

### Network

網絡層的由來：

> 以太网协议，依靠MAC地址发送数据。理论上，单单依靠MAC地址，上海的网卡就可以找到洛杉矶的网卡了，技术上是可以实现的。
>
> 但是，这样做有一个重大的缺点。以太网采用广播方式发送数据包，所有成员人手一"包"，不仅效率低，而且局限在发送者所在的子网络。也就是说，如果两台计算机不在同一个子网络，广播是传不过去的。这种设计是合理的，否则互联网上每一台计算机都会收到所有包，那会引起灾难。
>
> 互联网是无数子网络共同组成的一个巨型网络，很像想象上海和洛杉矶的电脑会在同一个子网络，这几乎是不可能的。

![](http://www.hauchenglee.com/assets/images/tech/network-layer-local-conntection.png)

> 因此，必须找到一种方法，能够区分哪些MAC地址属于同一个子网络，哪些不是。如果是同一个子网络，就采用广播方式发送，否则就采用"路由"方式发送。（"路由"的意思，就是指如何向不同的子网络分发数据包，这是一个很大的主题，本文不涉及。）遗憾的是，MAC地址本身无法做到这一点。它只与厂商有关，与所处网络无关。
>
> 这就导致了"网络层"的诞生。它的作用是引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个子网络。这套地址就叫做"网络地址"，简称"网址"。
>
> 于是，"网络层"出现以后，每台计算机有了两种地址，一种是MAC地址，另一种是网络地址。两种地址之间没有任何联系，MAC地址是绑定在网卡上的，网络地址则是管理员分配的，它们只是随机组合在一起。
>
> 网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包送到该子网络中的目标网卡。因此，从逻辑上可以推断，必定是先处理网络地址，然后再处理MAC地址。
>
> Ref: [互联网协议入门（一） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html){:target="_blank"}

<br>

*可以理解為：此處需要確定計算機的位置，怎麼確定？IPv4，IPv6！*

<br>

【IP】

- [](){:target="_blank"}

【Address Resolution Protocol】

- [](){:target="_blank"}

【Router】

- [](){:target="_blank"}

### Transport



*可以理解為：每一個應用程序都會在網卡註冊一個端口號，該層就是端口與端口的通信！常用的（TCP/IP）協議。*

<br>

【UDP】

- [](){:target="_blank"}

【TCP】

- [](){:target="_blank"}

### Session



*可以理解為：建立一個連接（自動的手機信息、自動的網絡尋址）。*



### Presentation



*可以理解為：解決不同系統之間的通信，eg: Linux下的QQ和Windows下QQ可以通信。*



### Application



*可以理解為：規定數據的傳輸協議。*

<br>

【HTTP】

- [](){:target="_blank"}

## Reference

- [网络基本功（一）：细说网络传输 \| 网络基本功系列](https://wizardforcel.gitbooks.io/network-basic/0.html){:target="_blank"}
- [互联网协议入门（一） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html){:target="_blank"}
- [What Is The OSI Model? \| Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/){:target="_blank"}
- [深入浅出－网络七层模型&&网络数据包 - 简书](https://www.jianshu.com/p/4b9d43c0571a){:target="_blank"}
- [](){:target="_blank"}

---