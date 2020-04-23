---
layout: post
title: Network - MAC (Media Access Control)
category: tech
tags: [tech]
---

## MAC Address Definition

> A **media access control address** (**MAC address**) is a unique identifier assigned to a network interface controller 
for use as a network address in communications within a network segment. This use is common in most IEEE 802 networking 
technologies, including Ethernet, Wi-Fi, and Bluetooth. Within the Open Systems Interconnection (OSI) network model, 
MAC addresses are used in the medium access control protocol sublayer or the data link layer. As typically represented, 
MAC addresses are recognizable as six groups of two hexadecimal digits, separated by hyphens, colons, or without a separator.
>
> Ref: [MAC address - Wikipedia](https://en.wikipedia.org/wiki/MAC_address){:target="_blank"}

媒體訪問控制地址（MAC地址）是一個唯一的標識符分配給網絡接口控制器（NIC）（也就是網卡），其用作網絡地址的網絡段內的通信。
這種用法在大多數IEEE 802網絡技術中很常見，包括以太網，Wi-Fi和藍牙。
在Open System Interconnection（OSI）網絡模型內，MAC地址用於數據鏈路層的媒體訪問控制協議子層中。
如通常所表示的，MAC地址可識別為兩組中的六組十六進制數字，用連字符，冒號分隔或不帶分隔符。

<br>

> A MAC address is a hardware identification number that uniquely identifies each device on a network. The MAC address is 
manufactured into every network card, such as an Ethernet card or Wi-Fi card, and therefore cannot be changed.
>
> Because there are millions of networkable devices in existence, and each device needs to have a unique MAC address, there 
must be a very wide range of possible addresses. For this reason, MAC addresses are made up of six two-digit hexadecimal 
numbers, separated by colons. For example, an Ethernet card may have a MAC address of 00:0d:83:b1:c0:8e.
>
> Ref: [MAC Address (Media Access Control Address) Definition](https://techterms.com/definition/macaddress){:target="_blank"}

MAC地址是唯一標識網絡上每個設備的硬件標識號。MAC地址是每個網卡（例如以太網卡或Wi-Fi卡）製造的，因此無法更改。

因為存在數百萬台可聯網的設備，並且每個設備都需要有一個唯一的MAC地址，所以必須有很多種可能的地址。
因此，MAC地址由六個兩位數的十六進制數字組成，並用冒號分隔。例如，以太網卡的MAC地址可以為00:0d:83:b1:c0:8e。

<br>

> 之前提到，以太网数据包的"标头"，包含了发送者和接受者的信息。那么，发送者和接受者是如何标识呢？
>
> 以太网规定，连入网络的所有设备，都必须具有"网卡"接口。数据包必须是从一块网卡，传送到另一块网卡。网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址。
>
> 有了MAC地址，就可以定位网卡和数据包的路径了。
>
> Ref: [互联网协议入门（一） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html){:target="_blank"}

<br>

![](http://www.hauchenglee.com/assets/images/tech/mac-address-hardware.jpg)

Ref: [What is a MAC Address? How to Find My MAC Address](https://learntomato.flashrouters.com/what-is-a-mac-address-how-to-find-my-mac-address/){:target="_blank"}

## MAC Frame

MAC地址格式（MAC ADDRESS FORMAT）：

> 每块网卡出厂的时候，都有一个全世界独一无二的MAC地址，长度是48个二进制位，通常用12个十六进制数表示。

![](http://www.hauchenglee.com/assets/images/tech/mac-address-format.png)

1. 第一組代表：廠商編號
2. 第二組代表：廠商的網卡流水號

一些知名製造商的[OUI](http://standards-oui.ieee.org/oui/oui.txt){:target="_blank"}：
1. CC:46:D6 - Cisco 
2. 3C:5A:B4 - Google, Inc.
3. 3C:D9:2B - Hewlett Packard
4. 00:9A:CD - HUAWEI TECHNOLOGIES CO.,LTD
5. 08-00-07 - Apple, Inc.

## MAC v.s. IP

1. OSI層不同
2. 是否可以更改不同
3. 使用協議不同
4. 使用功能不同

## Reference

- [Introduction of MAC Address in Computer Network - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-of-mac-address-in-computer-network/){:target="_blank"}
- [WHAT IS A MAC ADDRESS ? \| IP With Ease](https://ipwithease.com/what-is-a-mac-address/){:target="_blank"}

---