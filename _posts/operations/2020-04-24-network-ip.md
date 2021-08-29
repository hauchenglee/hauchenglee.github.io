---
layout: post
title: Network - IP
category: operations
tags: [network]
comments: true
toc: true
---

## Definition

Internet Protocol Address (IP address) Function:

> An IP address serves two principal functions. It identifies the host, or more specifically its network interface, and it 
provides the location of the host in the network, and thus the capability of establishing a path to that host. Its role has 
been characterized as follows: "A bane indicates what we seek. An address indicates where it is. A route indicates how 
to get there." The header of each IP packet contains the IP address of the sending host, and that of the destination 
host.
>
> Ref: [IP address - Wikipedia](https://en.wikipedia.org/wiki/IP_address#Function){:target="_blank"}

IP地址具有兩個主要功能。它標識主機，或更具體地說，標識其網絡接口，並提供主機在網絡中的位置，並因此提供建立到該主機的路徑的能力。
其角色的特徵如下："名稱表示我們要尋找的東西。地址表示我們在哪裡。路線表示如何到達那裡。"每個IP數據包的標頭包含發送主機的IP地址和目標主機的IP地址。

## IP vs MAC

why we need ip instead of mac?

hierarchical

## Classful Addressing

Historical classful network architecture:

計算網段 & 主機地址數量 -- 以Class A為例子：
- 網段：理論上為2^8，但需要減去Leading Bits，而A類Leading Bits共有1位，故總共的網段有2^7
- 主機地址：理論上為2^24，但需要減去127（代表自己）&255（代表廣播），故可使用的主機地址有2^24-2
- 綜上所述，A類總共可用地址有2^7 * (2^24-2)

<table>
    <thead>
        <tr>
            <th>Class</th>
            <th>Leading bits</th>
            <th>Number of networks</th>
            <th>Number of addresses <br>per network</th>
            <th>Range</th>
            <th>Remark</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>A Class</td>
            <td>0</td>
            <td>128 (2^7)<br> 2^(8 - leading bits) = 2^7</td>
            <td>16,777,216 (2^24) - 2</td>
            <td>1.0.0.1<br>127.255.255.254</td>
            <td>0 reserved for special purpose<br>127 reserved for loopback IP</td>
        </tr>
        <tr>
            <td>B Class</td>
            <td>10</td>
            <td>16,384 (2^14)</td>
            <td>65,536 (2^16) - 2</td>
            <td>128.1.0.1<br>191.255.255.254</td>
            <td></td>
        </tr>
        <tr>
            <td>C Class</td>
            <td>110</td>
            <td>2,097,152 (2^21)</td>
            <td>256 (2^8) - 2</td>
            <td>192.0.1.1<br>223.255.255.254</td>
            <td></td>
        </tr>
        <tr>
            <td>D Class</td>
            <td>1110</td>
            <td colspan="4">reserved for multicast</td>
        </tr>
        <tr>
            <td>E Class</td>
            <td>1111</td>
            <td colspan="5">255 reserved for broadcast<br>reserved and cannot be used on the public Internet</td>
        </tr>
    </tbody>
</table>

<br>

依據[Wiki](https://en.wikipedia.org/wiki/IP_address#Subnetting_history){:target="_blank"}的說明，
這套系統已經過時，缺乏可擴展性，並於1993年被[Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing){:target="_blank"}所取代。

> Today, remnants of classful network concepts function only in a limited scope as the default configuration parameters 
of some network software and hardware components (e.g. netmask), and in the technical jargon used in network administrators' discussions.

如今，分類網絡概念的殘餘僅在有限範圍內用作某些網絡軟件和硬件組件（例如，網絡掩碼）的默認配置參數，並且在網絡管理員的討論中使用了技術術語。

可以參考其他說明：[Classful network - Wikipedia](https://en.wikipedia.org/wiki/Classful_network){:target="_blank"}

## Classless Addressing 

### Identify

IP位址可以分為兩部分：Network Identifier (網絡編號) & Host Identifier (主機編號)

而若要分割成子網絡時，又會將主機編號（Host Identifier）再切分成子網路編號（Subnet Identifier）和主機編號（Host Identifier）

![](https://www.hauchenglee.com/assets/images/operations/network-ip-subnetwork.png)

<br>

這裡有幾個注意與條件限制：
1. Network Identifier在廣域網的環境中必須是唯一的；Host Identifier在局域網的環境中也必須是唯一的
2. 兩者搭配的情況下，不能全部相同。例如：
   1. 若現在電話有001+123456，那麼其它區域不能使用001的號碼，但可以在其它區號使用123456
   2. 相同地，如果今天電話有002+654321，那麼在002的區號裡不能有重複的電話號碼（654321）
3. Host Identifier在二進制表示中不可以同時為0，或是同時為1，不能將任何網絡或子網中的第一個和最後一個地址分配給任何單獨的主機。
   1. Host Identifier --> 0000,0000: 網絡本身識別碼
   2. Host Identifier --> 1111,1111: 廣播位址（代表的是該網絡上的所有主機）
4. Network Identifier: 127（二進制: 01111111）：是保留給本機回路測試使用的，它不可以被運用於實際的網路中，其中的127.0.0.1則代表任何一台IP主機本身。

reason:
- 減緩IPv4位址的耗盡速度。
- 更安全，更好管理

### Subnet Mask

子網路遮罩（子网掩码）：

Subnet Mask用處是切分那些是Network Identifier，那些是Host Identifier，就可以知道負責決定IP是否位於同一個廣播域裡。

例如今天有人叫作初音未來，我們無法直接地區分這個名字的姓與名的位置，所以今天加上了1100一組數字，1和0的分界就是姓與名的分界。

IP Example:

<table>
    <thead>
        <tr>
            <th></th>
            <th>Decimal</th>
            <th>Binary</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Network</td>
            <td>192.168.1.119</td>
            <td><span style="font-weight: bold;">11000000.10101000.00000001</span>.11000111</td>
        </tr>
        <tr>
            <td>Subnet Mask</td>
            <td>255.255.255.0</td>
            <td><span style="font-weight: bold;">11111111.11111111.11111111</span>.00000000</td>
        </tr>
    </tbody>
</table>

依據Subnet Mask左邊1為網絡位，右邊0為主機位，因此可以判斷：
1. 192.168.1為網絡位（11000000.10101000.00000001）
2. 119為此IP的主機位（11000111）
3. 廣播域範圍：192.168.1.0 ~ 192.168.1.254
4. 廣播地址：192.168.1.255

### CIDR

另一種子網掩碼的表達方式（Classless Inter-Domain Routing）:

IP / how many network bits in IP

```xxx.xxx.xxx.xxx/n```

Thus, 32 bits - n = network mask

example:

```192.168.12.0/23``` -> ```11000000.10101000.00001100.00000000```

1. network mask: 11111111.11111111.11111110.00000000 -> 255.255.254.0
2. network range: 192.168.12.0 ~ 192.168.13.255

/30 & /31 & /32：
- /30 subnet has 2 ip for hosts (example 192.168.1.0/30)
   - 2^2 = 4
   - 192.168.1.0 = network address
   - 192.168.1.1 = odd address
   - 192.168.1.2 = even address
   - 192.168.1.3 = broadcast address
   - Tips: Lower is odd ; higher is even
- /31 subnet has 2 ip for hosts (example 192.168.1.0/31)
   - this applied to point-to-point link only
   - 192.168.1.0 = even address
   - 192.168.1.1 = odd address
   - Tips: Lower is even ; higher is odd
   
- /32 (255.255.255.255): means a host

### Subnet zero

It is the first subnet obtained when subnetting the network address.

Cisco has turned this command on by default staring with Cisco IOS version 12.x

```console
Router#sh running-config
Building configuration...
Current configuration : 827 bytes
!
hostname Pod1R1
!
ip subnet=zero
```

## Private address & NAT

Reserved private IPv4 network ranges (RFC 1918):

<table>
    <thead>
        <tr>
            <th>Class</th>
            <th>Name</th>
            <th>CIDR block</th>
            <th>Address range</th>
            <th>Number of address</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>A</td>
            <td>24-bit block</td>
            <td>10.0.0.0/8</td>
            <td>10.0.0.0 - 10.255.255.255</td>
            <td>16,777,216</td>
        </tr>
        <tr>
            <td>B</td>
            <td>20-bit block</td>
            <td>172.16.0.0/12</td>
            <td>172.16.0.0 - 172.31.255.255</td>
            <td>1,048,576</td>
        </tr>
        <tr>
            <td>C</td>
            <td>16-bit block</td>
            <td>192.168.0.0/16</td>
            <td>192.168.0.0 - 192.168.255.255</td>
            <td>65,536</td>
        </tr>
    </tbody>
</table>

除了這三個IP地址段為私有IP地址外，其它的都為公網IP。

1. 私有位址（Private IP）的路由資訊不能對外散播
2. 使用私有位址作為來源或目的位址的封包，不能透過Internet來轉送
3. 關於私有位址的參考紀錄，只能限於內部網路使用

因為Private IP在Internet上無法被路由（Routing），因此用來架設企業、團體等內部網路，具有在安全性的幫助。

連線到外網的解決辦法：代理伺服器（proxy）或IP轉換（Network Address Translation）以取得新的IP位址。 

Example about NAT:

1. 作用：內網與外網IP之間的相互轉換與映射，可以使大量的內網IP位址轉換為一個或少量的公網IP位址，減少對公網IP位址的占用。
2. 家庭網絡普遍使用埠映射的方式，NAT的核心是一張映射表（源IP位址，源埠，目的IP位址，目的埠），將內網源IP位址和埠映射到同一個公網地址的不同埠。

原文網址：[https://kknews.cc/news/kv5vjaq.html](https://kknews.cc/news/kv5vjaq.html){:target="_blank"}

<table>
    <thead>
        <tr>
            <th>LAN IP</th>
            <th>Internet IP</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>192.168.1.55:5566</td>
            <td>219.152.168.222:9200</td>
        </tr>
        <tr>
            <td>192.168.1.59:80</td>
            <td>219.152.168.222:9201</td>
        </tr>
        <tr>
            <td>192.168.1.59:4465</td>
            <td>219.152.168.222:9202</td>
        </tr>
    </tbody>
</table>

## Network addressing and routing

【運作過程】

![](https://www.hauchenglee.com/assets/images/operations/network-addressing-and-routing.png)

1. 查詢目標IP地址：依據發送的封包Header IP Address來查閱並決定發送位址
2. 查詢標的主機所在的網域：如果是同一個子網（Subnet），則主機A會直接透過LAN功能發送數據給目標主機
3. 送出封包至路由器（Router）：如果主機A與主機B在不同的網域，因此A將IP封包送到路由器，路由器收到封包後會分析封包的資訊，一路傳送到正確的中介路由及目標主機B的位址。

## Packet structure

![](https://www.hauchenglee.com/assets/images/operations/ipv4_packet_header.jpg)

1. 第一行：
   1. IP Version: IP的版本號，目前IPv4為第四版，所以是0100（十六進制）
   2. Internet Header Length (IHL): 標頭長度。一列長度是32Bit（4byte），如果有五列（最後的Option不包含），則有20byte，換算十六進制就是0x14。這是最小值的情況
   3. Type of Service (TOS): 服務類型。代表不同的優先權與封包情況。例如：000 -> 設定IP順序﹐預設為0﹐否則數值越高越優先
   4. Total Length (TL): 封包總長度，以byte為單位，數值包含標頭與數據（Data）的總和
2. 第二行：
   1. Identification (ID): 封包的身份標識，如果封包被拆分，則以ID判別是否同一個原始封包
   2. Flag (FL): 如果封包太大，以flags告訴是否可以被分割分段
   3. Fragment Offset (FO): 標識此片段在原始IP數據包中的確切位置
3. 第三行：
   1. Time To Live (TTL): 存活時間。為了避免在網絡上有過多無用的數據，或因為其它情況無法送達目的地，如果數值為0時，則丟棄此封包。
   2. Protocol (PROT): 告訴目標主機的網絡層（Network）此數據包所屬的協議，即下一層協議。例如，ICMP的協議號為1，TCP為6，UDP為17
   3. Header Checksum (HC): 標頭檢驗值，檢查封包是否正確無誤收到，接收端並用此來檢驗後續的封包傳送情況
4. 第四行：
   1. Source IP Address (SA): 來源IP位址
   2. Destination IP Address (DA): 目的地IP位址
5. 第五行：
   1. Options & Padding: 這是可選字段，如果IHL的值大於5，則將使用這些字段。這些選項可能包含諸如安全性，記錄路由，時間戳等選項的值。此欄位很少使用到，一般是特殊情況或是特定的控制下，才會使用此字段

## IPv4 & IPv6

IP 組成是 32 bits 的數值，也就是由 32 個 0 與 1 組成的一連串數字。在把 32 bits 的 IP 分成四小段，每段含有 8 個 bits（不滿八位數則補0到滿），將 8 個 bits 計算成為十進位，並且每一段中間以小數點隔開，就成了所熟悉的 IP，

例如： 

- 00000000.00000000.00000000.00000000 => 0.0.0.0
- 11111111.11111111.11111111.11111111 => 255.255.255.255 
- 11000000.10101000.00101010.00000001 => 192.168.42.1

IPv4 Total can use: 4,294,967,296 (2^32)

IPv6 Total can use: 340,282,366,920,938,463,463,374,607,431,768,211,456 (2^128) (approximately 3.4×10^38 or 340 billion billion billion billion)

## Reference

Wiki:

- [IP address - Wikipedia](https://en.wikipedia.org/wiki/IP_address){:target="_blank"}
- [Subnetwork - Wikipedia](https://en.wikipedia.org/wiki/Subnetwork){:target="_blank"}
- [IPv6 - Wikipedia](https://en.wikipedia.org/wiki/IPv6){:target="_blank"}

IP & Subnetwork:

- [IP位址](http://dns-learning.twnic.net.tw/internet/intro7.html){:target="_blank"}
- [IP位址分五種等級](http://kevin.hwai.edu.tw/~kevin/material/EAssistant/IP_Class.htm){:target="_blank"}
- [IP Addressing - Tutorial](https://www.vskills.in/certification/tutorial/basic-network-support/ip-addressing/){:target="_blank"}
- [IP Networking Basics  \[Support\] - Cisco Systems](https://www.cisco.com/en/US/docs/security/vpn5000/manager/reference/guide/appA.html){:target="_blank"}

CIDR:

- [Classless Inter-Domain Routing Information](https://www.lifewire.com/cidr-classless-domain-routing-818375){:target="_blank"}

Subnet Mask:

- [了解 TCP/IP 定址及子網路基本概念](https://support.microsoft.com/zh-tw/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics){:target="_blank"}
- [如何理解子网掩码？ - 知乎](https://www.zhihu.com/question/56895036){:target="_blank"}

Packet structure:

- [IPv4 Packet Structure - The Third Internet](https://thirdinternet.com/ipv4-packet-structure/){:target="_blank"}
- [IPv4 - Packet Structure - Tutorialspoint](https://www.tutorialspoint.com/ipv4/ipv4_packet_structure.htm){:target="_blank"}
- [\[課業\] IP表頭格式介紹 @ 正Man's World :: 痞客邦 ::](https://joy0626.pixnet.net/blog/post/2109687){:target="_blank"}

Tools:

- [IP to Binary Converter](https://www.browserling.com/tools/ip-to-bin){:target="_blank"}
- [Binary to IP Converter](https://www.browserling.com/tools/bin-to-ip){:target="_blank"}

---