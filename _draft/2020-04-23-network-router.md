---
layout: post
title: Network - Router
category: tech
tags: [tech]
---

## Definition

1. HUB: 將網線集結起來的作用，並對接收到的信號進行再生放大，以擴大網絡的傳輸距離。
2. Switch: 智能集線器，可以分析目的地位置（MAC），用來隔離衝突域，連接的所有設備同屬於一個廣播域（子網），負責子網內部通信。
3. Router: 用來隔離廣播域（子網），連接的設備分屬不同子網，工作範圍是多個子網之間，負責網絡與網絡之間通信。

![](https://hauchenglee.github.io/assets/images/tech/network-hub-switch-router.jpg)

Ref: [Hub vs Switch vs Router: What’s the Difference?](http://www.fiberopticshare.com/hub-vs-switch-vs-router-whats-difference.html){:target="_blank"}

## HUB & Switch & Router

### Connection

小A與小B想要互相通信，因此他使用網線接在彼此的端口，實現了兩台電腦的互連。

![](https://hauchenglee.github.io/assets/images/tech/network-connection.png)

### HUB

現在小C也想加入通信，但因為每台電腦只有一個網口，所以需要另一台【微型機器】，將彼此的網線集中分發實現最初級的網絡互通。

![](https://hauchenglee.github.io/assets/images/tech/network-connection-hub.png)

<br>

集線器是最簡單、最便宜、最基礎的設備，因此被廣泛使用，但多年來與交換機（Switch）的價格日漸縮小、相差無幾，以至於時至今日運用範圍越來越小。

集線器運作方式也很簡單，運作在物理層，它會將收到的信息發送給其它的所有端口，無論目的地是哪台計算機，集線器本身對傳輸的數據一無所知。
因此，所有計算機都可以接收該消息，並且計算機本身需要決定是否接受該消息。

從運作方式也可以看出來，在集線器線路上的所有設備都可以看到網絡上的所有流量，因此不是很安全。

### Switch

如今，越來越多機器加入互相通信的行列，但是集線器有一個缺點，在集線器發送與接收信息的過程中，會屏蔽禁止其他人的通信的行為，否則信息會產生碰撞，引發錯誤。
而且，上述也提到集線器【會將收到的信息發送給其它的所有端口，無論目的地是哪台計算機，集線器本身對傳輸的數據一無所知】，這樣行為導致效率過低，造成不必要的干擾

如果在傳輸信息時就依據位置來傳送，就可以大大提高傳送速度，也解決了信息衝突的問題。

由於交換機（Switch）是根據網卡地址傳送數據，比網線直連多了一個步驟，因此交換機（Switch）位於數據鏈接層（Data Link）。

交換機本質上是一個智能集線器，具有學習的行為，只要接受過第一次傳輸的記錄後，就會了解並學習，從而提高傳輸速度。

![](https://hauchenglee.github.io/assets/images/tech/network-connection-switch.png)

【工作原理】：

- [集线器、网桥、交换机、路由器、网关大解析](https://www.tianmaying.com/tutorial/NetWorkInstrument#10){:target="_blank"}

### Router

在小型的網絡範圍中，上述設備運作良好，但如果今天A村與B村想跟C村通信，卻發現彼此的操作系統都不一致，導致信息間傳送的不相容。

解決方式就是統一度量衡，另外設立一個機器，負責將信息經過相同的協議（TCP/IP）加工成統一形式，
並且依據【DHCP】與【NAT】分派【IP】位置尋址，傳送到對方相同的設備，這就是路由器（Router）的功能。

路由器是前兩者最高階、最複雜、最智能的機器設備，它在OSI通信模型的第3層上運行。路由器旨在將多個網絡連接在一起。

![](https://hauchenglee.github.io/assets/images/tech/network-connection-router.png)

【工作原理】：

- [集线器、网桥、交换机、路由器、网关大解析](https://www.tianmaying.com/tutorial/NetWorkInstrument#20){:target="_blank"}

## Collision Domain & Broadcast Domain

衝突域（Collision Domain）：
- 成因：兩個設備在共享的網段同時發送數據包，就會發生衝突
- 解決：撤換集線器，使用交換機或是路由器

![](https://hauchenglee.github.io/assets/images/tech/collision_domains.jpg)

<br>

廣播域（Broadcast Domain）：
- 成因：路由器上所有的端口都位於不同的廣播域中，彼此獨立存在不互相影響。在同一個廣播域中，所有的設備都必須聆聽其中一個設備的廣播消息，造成LAN阻塞。
- 解決：廣播域的數量越多，網絡為所有用戶提供更好的帶寬的效率就越高。

![](https://hauchenglee.github.io/assets/images/tech/broadcast_domains.jpg)

<br>

![](https://hauchenglee.github.io/assets/images/tech/Network-Broadcast-Domain-Collision.png)




## Switch

### Spanning Tree


### store-and-forward

### switched network


## Router

### Gateway

### store-and-forward

## Routing

### topology

### multiplexing vs de-multiplexing

### FDM vs TDM

### Statistical multiplexing


## Router & DHCP v.s. NAT 

### DHCP

> DHCP — Dynamic Host Configuration Protocol — is how dynamic IP addresses are assigned. When it first connects to the network, 
a device asks for an IP address to be assigned to it, and a DHCP server responds with an IP address assignment. 
A router connected to your ISP-provided internet connection will ask your ISP’s server for an IP address; this will be your IP address on the internet. 
Your local computers, on the other hand, will ask the router for an IP address, and these addresses are local to your network.
>
> Ref: [What's the Difference Between a Hub, a Switch, and a Router? - Ask Leo!](https://askleo.com/whats_the_difference_between_a_hub_a_switch_and_a_router/){:target="_blank"}

DHCP —動態主機配置協議 —動態IP地址的分配方式。首次連接到網絡時，設備會要求分配IP地址，而DHCP服務器會以IP地址分配作為響應。
連接到您的ISP提供的Internet連接的路由器將詢問您的ISP的服務器IP地址。這將是您在互聯網上的IP地址。另一方面，您的本地計算機將向路由器詢問IP地址，這些地址對於您的網絡是本地的。

![](https://hauchenglee.github.io/assets/images/tech/network-router-dhcp.png)

### NAT

網絡地址轉換（Network address translation）:

![](https://hauchenglee.github.io/assets/images/tech/nat-removebg.png)

Ref: [What is Network Address Translation (NAT)?](https://whatismyipaddress.com/nat){:target="_blank"}

<br>

> NAT — Network Address Translation- – is the way the router translates the IP addresses of packets that cross the internet/local network boundary. 
When computer “A” sends a packet, the IP address that it’s “from” is that of computer “A” — 192.168.0.1, in the example above. 
When the router passes that on to the internet, it replaces the local IP address with the internet IP address assigned by the ISP — 1.2.3.4, in the example. 
It also keeps track, so if there’s a response the router knows to do the translation in reverse, replacing the internet IP address with the local IP address 
for machine “A”, and then sending that response packet on to machine “A”.
>
> A side effect of NAT is that machines on the internet cannot initiate communications to local machines; 
they can only respond to communications initiated by them. This means that the router also acts as an effective firewall.
>
> Ref: [What's the Difference Between a Hub, a Switch, and a Router? - Ask Leo!](https://askleo.com/whats_the_difference_between_a_hub_a_switch_and_a_router/){:target="_blank"}

NAT（網絡地址轉換）是路由器轉換跨越Internet / local網絡邊界的數據包的IP地址的方式。在上面的示例中，當計算機"A"發送數據包時，其來自的IP地址就是計算機"A"的IP地址-192.168.0.1。
當路由器將其傳遞到Internet時，它將用ISP分配的Internet IP地址替換本地IP地址：在此示例中為1.2.3.4。
它也保持跟踪，因此，如果有響應，則路由器知道進行反向轉換，用機器"A"的本地IP地址替換Internet IP地址，然後將該響應數據包發送到機器"A"。

NAT的副作用是Internet上的計算機無法啟動與本地計算機的通信。他們只能響應他們發起的通信。這意味著路由器還可以充當有效的防火牆。

<br>

總結：
1. 將一個IP地址空間重新映射為另一個IP地址空間的方法，也就是將內部私人網段分配內部網域的計算機的過程，並且公開一個公共IP位置來訪問外部資源（Internet）
1. 具有防火墻的功能
1. 限制組織或公司必須使用的公共IP地址的數量，可以節省IPv4的地址空間

![](https://hauchenglee.github.io/assets/images/tech/network-router-firewall.png)

## Compare

路由谋短，交换求快。

<table>
    <thead>
        <tr>
            <th></th>
            <th>HUB</th>
            <th>Switch</th>
            <th>Router</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>OSI Layer</td>
            <td>Physical layer</td>
            <td>Data link layer</td>
            <td>Network layer</td>
        </tr>
        <tr>
            <td>Function</td>
            <td>To connect a network of personal computers together, they can be joined through a central hub</td>
            <td>Allow connections to multiple devices, manage ports, manage VLAN security settings</td>
            <td>Direct data in a network</td>
        </tr>
        <tr>
            <td>Data <br>Transmission <br>form</td>
            <td>electrical signal or bits</td>
            <td>frame & packet</td>
            <td>packet</td>
        </tr>
        <tr>
            <td>Port</td>
            <td>4/12 ports</td>
            <td>multi-port, usually between 4 and 48</td>
            <td>2/4/5/8 ports</td>
        </tr>
        <tr>
            <td>Transmission <br>type</td>
            <td>Frame flooding, unicast, multicast or broadcast</td>
            <td>First broadcast, then unicast and/or multicast depends on the need</td>
            <td>At initial Level Broadcast then Uni-cast and multicast</td>
        </tr>
        <tr>
            <td>Device type</td>
            <td>Non-intelligent device</td>
            <td>Intelligent device</td>
            <td>Intelligent device</td>
        </tr>
        <tr>
            <td>Used in <br>(LAN, MAN, WAN)</td>
            <td>LAN</td>
            <td>LAN</td>
            <td>LAN, MAN, WAN</td>
        </tr>
        <tr>
            <td>Transmission <br>mode</td>
            <td>Half duplex</td>
            <td>Half/Full duplex</td>
            <td>Full duplex</td>
        </tr>
        <tr>
            <td>Speed</td>
            <td>10Mbps</td>
            <td>10/100Mbps, 1Gbps</td>
            <td>1-100Mbps (wireless); <br>100Mbps-1Gbps (wired)</td>
        </tr>
        <tr>
            <td>address used for <br>data <br>transmission</td>
            <td>MAC address</td>
            <td>MAC address</td>
            <td>IP address</td>
        </tr>
    </tbody>
</table>

Ref: [What's the Difference? Hub vs Switch vs Router \| FS Community](https://bit.ly/2W1swnN){:target="_blank"}

## Reference

Definition & Compare:

- [What's the Difference Between a Hub, a Switch, and a Router? - Ask Leo!](https://askleo.com/whats_the_difference_between_a_hub_a_switch_and_a_router/){:target="_blank"}
- [如何跟小白解释路由器和交换机的区别？并且家用路由器充当了路由器和交换机的功能吗？ - 知乎](https://www.zhihu.com/question/22007235){:target="_blank"}
- [路由器和交换机的区别，太经典了_网络_alpha_love的博客-CSDN博客](https://blog.csdn.net/alpha_love/article/details/76695411){:target="_blank"}

Collision Domain & Broadcast Domain:

- [Collision & broadcast domain](https://study-ccna.com/collision-broadcast-domain/){:target="_blank"}
- [Collision Domain and Broadcast Domain in Computer Network - GeeksforGeeks](https://www.geeksforgeeks.org/collision-domain-and-broadcast-domain-in-computer-network/){:target="_blank"}
- [如何形象地理解冲突域和广播域？ - 知乎](https://www.zhihu.com/question/27723724){:target="_blank"}
- [冲突域和广播域的区分 - CloudDeveloper - 博客园如何形象地理解冲突域和广播域？ - 知乎](https://www.cnblogs.com/bakari/archive/2012/09/08/2677086.html){:target="_blank"}

DHCP v.s. DNS:

- [DHCP and DNS: What Are They, What’s Their Difference? \| FS Community](https://community.fs.com/blog/dhcp-and-dns-difference.html){:target="_blank"}

---