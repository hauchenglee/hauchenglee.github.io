---
layout: post
title: Network - Ethernet & MAC
category: it-operations
tags: [network]
---

## Ethernet

What is Ethernet:
1. 以太網是一個**傳輸技術**
2. 它取代過往的**局域網**傳輸技術，例如Token Ring、FDDI、ARCNET
3. 它是目前應用最普遍的**局域網**技術

How it works:
- 使用设备：电缆，网卡，路由器，集线器，交换机或网关
- 检查线路是否正在使用传输，如果高速公路上有数据包，则要发送的设备会延迟几千分之一秒再试一次，直到可以发送为止
- 如果知道收件人，则直接发送，否则使用广播方式，其他设备检查该数据包以查看它们是否为收件人

Ethernet vs Internet:
1. Ethernet: 是一個傳輸技術標準
2. Internet: 是一個網絡區域範圍

Ref:
- [Ethernet - Wikipedia](https://en.wikipedia.org/wiki/Ethernet){:target="_blank"}
- [The Fundamentals of an Ethernet LAN, Explained](https://www.lifewire.com/what-is-ethernet-3426740){:target="_blank"}

### Ethernet Frame

[Brief History](https://www.ibm.com/support/pages/ethernet-version-2-versus-ieee-8023-ethernet){:target="_blank"}:
1. 1980-81: DIX 1.0 (or Ethernet I)
2. 1982: DIX 2.0 (or Ethernet II)
3. 1983: IEEE 802.3

以太类型（EtherType）是一个在以太网帧中的占用两字节的字段，这一字段代表了在以太网帧中封装了何种协议。
该字段首次出现在以太网II帧（Ethernet II framing）中，并在后来由IEEE制定为IEEE 802.3以太网标准。

![](https://hauchenglee.github.io/assets/images/network/799px-Ethernet_Type_II_Frame_format.svg.png)

p.s. CCNA 考試不會考

Ref: [Ethernet frame - Wikipedia](https://en.wikipedia.org/wiki/Ethernet_frame){:target="_blank"}

## MAC

What is MAC:
- 媒體訪問控制地址（MAC地址）是一個唯一的標識符分配給網絡接口控制器（NIC）（也就是網卡）
- 以太网规定，连入网络的所有设备，都必须具有”网卡”接口。数据包必须是从一块网卡，传送到另一块网卡。网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址

MAC vs IP:
- OSI層不同
- 是否可以更改不同
- 使用協議不同
- 使用功能不同

### MAC Finder

```console
> ipconfig /all


Window IP configuration
   Physical Address . . . . . . . . : B4-2E-99-14-3D-31
```

Go to [MAC Address Finder](https://macvendors.com/){:target="_blank"}

Return:

> GA-BYTE TECHNOLOGY CO.,LTD.

### MAC Frame

MAC地址格式（MAC ADDRESS FORMAT）：

> 每块网卡出厂的时候，都有一个全世界独一无二的MAC地址，长度是48个二进制位，通常用12个十六进制数表示。

![](https://hauchenglee.github.io/assets/images/network/mac-address-format.png)

1. 第一組代表：廠商編號
2. 第二組代表：廠商的網卡流水號

一些知名製造商的[OUI](http://standards-oui.ieee.org/oui/oui.txt){:target="_blank"}：
1. CC:46:D6 - Cisco 
2. 3C:5A:B4 - Google, Inc.
3. 3C:D9:2B - Hewlett Packard
4. 00:9A:CD - HUAWEI TECHNOLOGIES CO.,LTD
5. 08-00-07 - Apple, Inc.

## Network Area

<table>
    <thead>
        <tr>
            <th></th>
            <th>LAN</th>
            <th>WAN</th>
            <th>WLAN</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Range</td>
            <td>是一個有限區域內的互連的計算機網絡</td>
            <td>是延伸一個大型地理區域的計算機網絡，WAN连接多个LAN</td>
            <td>使用鏈接兩個或多個設備的無線通信，形成一個局域網在有限的區域內（LAN）</td>
        </tr>
        <tr>
            <td>Example</td>
            <td>通常在同一建筑物，如住宅、学校、实验室</td>
            <td>可能限于企业（公司或组织）或公众可以访问</td>
            <td>可大可小</td>
        </tr>
        <tr>
            <td>Tech</td>
            <td>比较便宜的Ethernet & Wi-Fi</td>
            <td>更为安全的Ethernet</td>
            <td>based on IEEE 802.11 standards and are marketed under the Wi-Fi</td>
        </tr>
        <tr>
            <td>Layer</td>
            <td>可以同时使用第1层和第2层设备</td>
            <td>使用第3层设备运行</td>
            <td></td>
        </tr>
    </tbody>
</table>

局域網和廣域網是區域網絡的兩個主要和最知名的類別，而其他類別隨著技術的發展而出現。
類似類型的網絡是個人區域網絡（PAN），局域網（LAN），園區網絡（CAN）或城域網（MAN），它們通常分別限於房間，建築物，校園或特定的城域。

![](https://hauchenglee.github.io/assets/images/network/Data_Networks_classification_by_spatial_scope.png)

Ref: [File:Data Networks classification by spatial scope.png - Wikipedia](https://en.wikipedia.org/wiki/File:Data_Networks_classification_by_spatial_scope.png){:target="_blank"}

<br>

![](https://hauchenglee.github.io/assets/images/network/network-lan-wan.png)

Ref: [LANs, WANs, and Other Area Networks Explained](https://www.lifewire.com/lans-wans-and-other-area-networks-817376){:target="_blank"}

---