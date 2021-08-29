---
layout: post
title: Network - Broadcast 廣播
category: operations
tags: [network]
comments: true
toc: true
---

## Definition

![](https://www.hauchenglee.com/assets/images/operations/network-broadcasting.png)

<br>

> In computer networking, telecommunication and information theory **broadcasting** is a method of transferring a message to 
all recipients simultaneously. Broadcasting can be performed as a high-level operation in a program, for example, broadcasting 
in Message Passing Interface, or it may be a low-level networking operation, for example broadcasting on Ethernet.
>
> All-to-all communication is a computer communication method in which each sender transmits messages to all receivers 
within a group. In networking this can be accomplished using broadcast or multicast. This is in contrast with the point-to-point 
method in which each sender communicates with one receiver.
>
> Ref: [Broadcasting (networking) - Wikipedia](https://en.wikipedia.org/wiki/Broadcasting_(networking)){:target="_blank"}

在計算機網絡，電信和信息論中，廣播是一種同時向所有收件人傳輸消息的方法。
廣播可以作為程序中的高級操作（例如，在消息傳遞接口中進行廣播）執行，也可以是低級網絡操作，例如，在以太網上進行廣播。

全部通信是一種計算機通信方法，其中，每個發送方將消息發送到組中的所有接收方。
在聯網中，可以使用廣播或多播來實現。這與每個發送方與一個接收方進行通信的點對點方法形成對比。

![](https://www.hauchenglee.com/assets/images/operations/800px-Broadcast.svg.png)

<br>

> When a host broadcast a packet, it specifies the address of intended recipient in the address field of the packet. 
Now as the packet is broadcasted it is received by all the other hosts in the network. After receiving the packet, 
each host checks the address field of the packet. If the packet has an address of receiving host, it is processed by the 
receiving host. Else the packet is ignored.
>
> Ref: [Difference Between Broadcast and Multicast (with Comparison Chart) - Tech Differences](https://techdifferences.com/difference-between-broadcast-and-multicast.html){:target="_blank"}

主機廣播數據包時，會在數據包的地址字段中指定目標收件人的地址。
現在，在廣播數據包時，網絡中的所有其他主機都將其接收。接收到數據包後，每個主機都會檢查數據包的地址字段。
如果數據包具有接收主機的地址，則由接收主機處理。否則，該數據包將被忽略。

<br>

Example:

假設您正在一個有50名學生的教室裡講課。在這之間，您叫出一個學生"詹姆斯站起來"。
儘管課堂上所有學生都在聽，但只有詹姆斯會回答，其他人只會忽略此消息。

## Layer 2 broadcast vs Layer 3 broadcast

<table>
    <thead>
        <tr>
            <th></th>
            <th>Layer 2</th>
            <th>Layer 3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>目的地</td>
            <td>ff:ff:ff:ff:ff:ff</td>
            <td>xxx.xxx.xxx.255</td>
        </tr>
        <tr>
            <td>媒介</td>
            <td>switch</td>
            <td>router</td>
        </tr>
        <tr>
            <td>廣播區域</td>
            <td>本地网段 VLAN<br>every network interface card (NIC) will receive and read the frame</td>
            <td>该子网的所有IP地址都可以接收到它</td>
        </tr>
    </tbody>
</table>

Ref: [layer 2 broadcast layer 3 broadcast](https://bit.ly/2Jov1y7){:target="_blank"}

## Unicast & Broadcast & Multicast

<table>
    <thead>
        <tr>
            <th></th>
            <th>Unicast 單播</th>
            <th>Broadcast 廣播</th>
            <th>Multicast 組播</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>sender</td>
            <td>one</td>
            <td>one</td>
            <td>one / many</td>
        </tr>
        <tr>
            <td>recipient</td>
            <td>one</td>
            <td>many</td>
            <td>one / many</td>
        </tr>
        <tr>
            <td>destination</td>
            <td>one</td>
            <td>one</td>
            <td>many</td>
        </tr>
    </tbody>
</table>

<br>

![](https://www.hauchenglee.com/assets/images/operations/800px-Unicast.svg.png)

▲ Unicast

<br>

![](https://www.hauchenglee.com/assets/images/operations/800px-Multicast.svg.png)

▲ Multicast

## Reference

- [Difference between Unicast, Broadcast and Multicast in Computer Network - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-unicast-broadcast-and-multicast-in-computer-network/){:target="_blank"}
- [Difference Between Broadcast and Multicast (with Comparison Chart) - Tech Differences](https://techdifferences.com/difference-between-broadcast-and-multicast.html){:target="_blank"}
- [Network Broadcast](http://www.firewall.cx/networking-topics/general-networking/109-network-broadcast.html){:target="_blank"}

---