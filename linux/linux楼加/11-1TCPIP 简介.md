---
show: step
version: 1.0
enable_checker: true
---
# TCP/IP 简介 

## 1 实验介绍

#### 1.1 实验内容

本节实验主要是介绍 `TCP/IP` 协议，在讲解之前先简单了解一些网络基本术语，以及比较重要的 **OSI 七层网络模型**，然后通过讲解 `TCP/IP` 协议栈引出 `TCP/IP` **三次握手和四次挥手**的具体过程。最后为大家讲解了网络层协议中相当重要的 IP 协议的具体内容，和一些常见的网络层其他协议。

#### 1.2 实验知识点

+ 网络预备知识

+ OSI 七层网络模型

+ TCP/IP 协议

+ TCP/IP 三次握手和四次挥手

+ 网络层协议

## 2 网络基础知识

下面我们将会学习网络基础知识。

### 2.1 RFC

`RFC` 是一系列以编号排定的文件。收集了有关因特网相关资讯，以及 UNIX 和因特网社群的软件文件。目前 RFC 文件是由 Internet Society（ISOC）所赞助发行。一共有 4000 多个协议的定义，基本的互联网通信协议都有在RFC文件内详细说明，有兴趣的同学可以参考 `RFC` 文档结合实验内容来学习。

所有的 `RFC` 文档都可以从网络上找到，[官网为IETF](http://www.ietf.org/)。在网站上面可以通过分类以及搜索快速找到目标协议的 `RFC` 文档。目前在 `IETF` 网站上面的 `RFC` 文档有数千个，但是我们不需要全部掌握，在工作或学习中如果遇到可以找到对应的解释，理论与实际结合会有更好地效果，单纯阅读 `RFC` 的效果一般。

### 2.2 网络分类

以网络的覆盖范围分为：

+ `局域网（Local Area Network，LAN）`：覆盖在较小范围内的工作站或者校园等等，小于 1 km 的范围左右

+ `城域网（Metropolitan Area Network，MAN）`：覆盖在一个城市范围左右的网络，大概的范围在 5~50 公里

+ `广域网（Wide Area Network，WAN）`：覆盖一个国家或者是跨越国家的范围，通常为几十到几千公里的范围

以网络的传输介质分为：

+ `有线网络`

+ `无线网络`

以网络的使用者分为：

+ `公用网（public network）`：就是所有愿意按相关公司的规定交纳费用的人都可以使用这种网络。因此公用网也可称为公众网，如 CHINANET。

+ `专用网（private network）`：是某个部门为本单位的特殊业务工作的需要而建造的网络。这种网络不向本单位以外的人提供服务。例如，军队、铁路、电力等系统均有本系统的专用网。

### 2.3 网关

网关（Gateway）通常指一种能使两种不同类型的网络系统或软件可以进行通信的软件或硬件接口。

按照不同的分类标准，网关可以分为很多种。TCP/IP 协议里的网关是最常用的，通常所讲的“网关”均指 TCP/IP 协议下的网关。所有网关实质上是一个网络通向其他网络的IP地址。

默认网关是指一台主机如果找不到可用的网关，就把数据包发给默认指定的网关，由这个网关来处理数据包。现在主机使用的网关，一般指的是默认网关。

可以简单查看一下本机的网关：`ip route show`

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510388865673.png/wm)

### 2.4 MTU

**最大传输单元**（Maximum Transmission Unit，MTU）是指一种通信协议的某一层上面所能通过的最大数据单元的大小（以字节为单位）。最大传输单元这个参数通常与通信接口有关（网络接口卡、串口等）。

链路层具有最大传输单元 MTU 这个特性，它限制了数据帧的最大长度，不同的网络类型都有一个上限值。以太网 MTU 是 1500，你可以用 `netstat -i` 命令查看这个值。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510046311846.png/wm)

**常见 MTU 值**

根据宽带连接方式的不同，MTU 可能不尽相同，如下所示：

1.PPPoE/ADSL: `1360-1492`
2.PPTP VPN: `1400-1460`
3.L2TP VPN: `1400-1460`
4.Fixed IP: `1400-1500`
5.DHCP: `1400-1492`

## 3 OSI 七层网络

下面我们将会学习OSI 七层网络。

### 3.1 OSI 七层网络模型

**OSI** 是 `Open System Interconnect` 的缩写，译为开放式系统互联，主要是把网络通信的工作分为**7层**,分别是**物理层,数据链路层,网络层,传输层,会话层,表示层和应用层。**

**OSI 七层网络模型**

|层次|功能说明|设备|对应协议|
|-|-|-|-|
|`应用层(Application)`|提供应用程序的接口|网关|TFTP/HTTP/SNMP/FTP/SMTP/DNS/Telnet
|`表示层(Presentation)`|将上层数据进行转换和编译为标准的文件（数据格式化、代码转换、数据加密）|网关|无
|`会话层(Session)`|会话的建立和结束|网关|无|
|`传输层(Transport)`|提供端对端的接口|网关|TCP/UDP|
|`网络层(Network)`|为数据包选择路由、寻址|路由器|IP/ICMP/RIP/OSPF/BGP/IGMP
|`数据链路层(Date Link)`|保证无差错的数据链路，传输有地址的帧以及错误检测功能|交换机、网桥、网卡|PPP/ARP/RARP|
|`物理层(Physical)`|传输比特流，以二进制数据形式在物理媒介上传输数据|集线器、中继器|ISO2110/IEEE802/IEEE802.2|


层模型的每一层都具有清晰的特征:

+ 第七至第四层(应用层->表示层->会话层->传输层)处理数据源和数据目的地之间的端到端通信;
+ 第三至第一层（网络层->数据链路层->物理层）处理网络设备间的通信。

### 3.2 OSI 数据通信过程

OSI 按照功能性进行划分，分成了上面七层的模型，那么用户数据在这样一个网络模型中是按照怎样方式进行数据通信的呢，下面用几张图片为大家一一讲解这个过程。

如图所示，这里有两个应用（`Application1` 和 `Application2`）相互间进行通信，中间会有路由器等网络设备进行连接。

![Networking_7layer.png](https://doc.shiyanlou.com/document-uid113508labid1timestamp1474438806339.png/wm)

原始数据不会直接传输到目的应用程序中，因为最底层只能传输 `1\0` 的比特流，所以用户的原始数据需要经过一定的加工处理。数据从应用程序中出来，进入 OSI 网络模型的协议簇，数据在协议层次当中自顶向下通过每一层，**每一层都会对数据增加一些首部或尾部信息**，这样的信息称之为协议数据单元（`Protocol Data Unit`，缩写为`PDU`），在分层协议系统里，在指定的协议层上传送的数据单元，包含了该层的协议控制信息和用户信息。(这样在数据的头部或者尾部添加信息的举动称之为**封装**。)

**封装示意图**

![encapsulation.jpg](https://doc.shiyanlou.com/document-uid113508labid1timestamp1474440381767.png/wm)

每层添加的协议数据单元(`PUD`)名称：

+ 物理层 ：**数据位（Bit）**
+ 数据链路层 ：**数据帧（Frame）**
+ 网络层 ：**数据包（Packet）**
+ 传输层 ：**数据段（Segment）**
+ 五层以上 ：**数据（Data）**

**Application2** 所在的计算机在接收到比特流之后，再进行层层的转化（**比特流-->数据帧-->数据包-->数据段-->数据**），也就是将前面所加的 `PDU `一层一层的去掉，还原出 **Application1** 所发出原始的数据。

![osi_header_trailer.jpg](https://doc.shiyanlou.com/document-uid113508labid1timestamp1474443033814.png/wm)

如图所示，这就是数据在 OSI 模型中的传输的过程。简单来说就是从模型的顶部到底部一层层的封装，然后再在目的端一层层的分离，最终获取到原始的数据。

## 4 TCP/IP 协议

下面我们将会学习TCP/IP 协议。

### 4.1 概述

`TCP/IP 协议簇`是一个协议集合,里面包括了`IP`协议，`IMCP`协议，`TCP`协议，以及熟悉的`http`、`ftp`、`pop3`协议等等，这些协议统称为`TCP/IP`。`TCP/IP` 协议族中有一个重要的概念是分层，TCP/IP 协议按照层次分为以下四层:**应用层、传输层、网络层、网络接口层。**

### 4.2 TCP/IP 四层模型

**四层模型**

| 层次     | 功能说明           | 协议                |
| -------- | ------------------ | ------------------- |
| `应用层` | 应用程序间沟通的层 | FTP/HTTP/DNS/TELNET |
|`传输层`|提供应用程序间的通信|TCP/UDP
|`网络层`|负责提供基本的数据封包传送功能，让每一块数据包都能够到达目的主机(但不检查是否被正确接收)|IP/ARP/RARP/ICMP
|`网络接口层`|包括操作系统中的设备驱动程序和计算机中对应的网络接口卡|TCP/IP 协议的基层|

### 4.3 网络层协议

#### 4.3.1 TCP

`TCP`（Transmission Control Protocol 传输控制协议）是**一种面向连接的、可靠的、基于字节流的传输层通信协议**，由 IETF 的 RFC 793 定义。

**TCP 特点：**

+ TCP 是**面向连接的**， TCP 进行通信时需要先建立连接，类似于打电话，需要先拨号，对方接通了才建立连接。

+ TCP 是**可靠的交付服务**，正因为面向连接，所以数据包需要得到接收方确认才会继续发送。

+ TCP 提供**全双工通信**，允许通信双方任何时候都能发送数据，因为 TCP 连接的两端都设有发送缓存和接收缓存。（相关的还有单工、半双工。单工就是单车道的单行道，只有一条路并且只能发送方发给接受方，接收方不能发送给发送方；半双工就是双向单车道，这条道可以过去也可以过来，但是在同一时间只能一辆车通过，同一时间只能 A 发给 B，或者 B 发给 A；全双工就是双向双车道。）

+ TCP 是**面向字节流的**，也就是 TCP 将上层传输来的消息看成一连串的无结构的字节流。

**TCP 报文段首部**

> TCP 传输的数据单元是它的报文段，包括首部和数据，在数据传输过程中，报文都会进行封装后再进行传输。

![tcp_header1.png](https://doc.shiyanlou.com/document-uid113508labid1timestamp1474939565982.png/wm)

TCP 的首部使用 `20` 个字节

第一行: 源端口号，目的端口号，主要用于建立连接时，确定源端口（本机）和目的主机的端口号。

第二行: 序号，用来表示发送端到接受端的数据字节流。

第三行: 确认序号，表示下一次所期望收到的数据的序列号，只有 ACK 标志为 1 时，确认号字段才有效。一旦建立连接，ACK 标志总被设置为 1。

第四行：数据偏移、保留、`TCP 的标志位`、窗口

第五行：校验和、紧急指针

第六行：参数

**补充：**

标志位：共 6 个，即 `URG`、`ACK`、`PSH`、`RST`、`SYN`、`FIN`，具体含义如下：

+ URG：紧急指针（urgent pointer）有效
+ ACK：确认序号有效
+ PSH：接收方应该尽快将这个报文交给应用层
+ RST：重置连接
+ SYN：同步序号用来发起了一个新连接
+ FIN：释放一个连接

#### 4.3.2 UDP

UDP（User Datagram Protocol）用户数据报协议，UDP 提供的是**面向无连接的服务**。和 TCP 协议处于一个分层中，属于传输层协议，但是与 TCP 协议不同，UDP 协议并不提供超时重传，出错重传等功能，所以是不可靠的协议。

**UDP 特点：**

+ UDP 是**无连接的**，发送数据之前不用建立连接，所以时延小。

+ UDP 是**面向报文的**，也就是 UDP 并不差分，也不合并报文，上层传输过来的数据不会有其他的处理，保留报文的边界。

+ UDP **没有拥塞控制**，因为没有缓冲，数据直接添加首部就传输出去，实时性，发送效率高。

**UDP 报文段首部**

![UDP header](https://doc.shiyanlou.com/document-uid113508labid1timestamp1474939505177.png/wm)

UDP 的首部开销小，只有 `8` 个字节。
包括：`源端口`、`目的端口`、`长度`、`校验`。

### 4.4 TCP/IP 三次握手和四次挥手

#### 4.4.1 三次握手

三次握手(Three-way Handshake)，是指建立一个 TCP 连接时，需要客户端和服务器总共发送 3 个包。

三次握手的目的是连接服务器指定端口，建立 TCP 连接,并同步连接双方的序列号和确认号，同时交换 TCP 窗口大小信息。（在 socket 编程中，客户端执行 connect() 时，就将触发三次握手。） 

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510045204298.png/wm)

**三次握手：Client 主动打开 TCP 传输，Server 被动打开。**

**第一次握手：Client 发送 SYN = 1，seq = x 给 Server**

说明：Client 的 TCP 向 Server 发出连接请求报文段，将其首部中的同步位置 1，即 SYN = 1，并产生序号 seq = x，表明传送数据时的第一个数据字节的序号是 x ，Client 进入 SYN_SENT 状态，等待 Server 确认。

**第二次握手：Server 发送 SYN = 1，ACK = 1，seq = y，ack = x + 1 给 Client**

说明：服务器的 TCP 收到连接请求报文段后，如同意，则发回确认。 Server 在确认报文段中应将 SYN 和 ACK 都置 1 ，即 SYN = 1， ACK = 1，其确认号 ack = x + 1，产生一个自己的序号 seq = y，将其发回给 Client 确认连接请求，Server 进入 SYN_RCVD 状态。

**第三次握手：Client 发送 ACK = 1，seq = x + 1，ack = y + 1 给 Server**

说明：Client 收到此报文段后向 Server 给出确认，检查 ACK 是否为 1 。若正确，Client 发送连接建立的确认包，ACK = 1，ack = y + 1 。Server 收到确认包后和 Client 进入 ESTABLISHED 状态。

然后，Client 的 TCP 通知上层应用进程，连接已经建立。 Server 的 TCP 收到 Client 的确认后，也通知其上层应用进程：TCP 连接已经建立。

#### 4.4.2 四次挥手

TCP 连接的解除同样需要发送四个包，因此被称为四次挥手(four-way handshake)。Client 或 Server 均可主动发起挥手动作，在 socket 编程中，任何一方执行 close() 操作即可产生挥手操作。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510045389556.png/wm)

Client 先向其 TCP 发出连接释放报文段，并停止再发送数据，主动关闭 TCP 连接。

**第一次挥手：Client 发送 FIN = 1，seq = u 给 Server**                           

说明：Client 把连接释放报文段首部的 FIN 置 1 ，关闭 Client 到 Server 的数据传送，即 FIN = 1，其产生的序号 seq = u，Client 进入 `FIN_WAIT_1` 状态，等待 Server 的确认。 

**第二次挥手： Server 发送 ACK = 1，seq = v，ack = u + 1 给 Client**                         

说明： Server 收到 FIN 后发出确认，确认号 ack = u（seq） + 1，而这个报文段自己的序号 seq = v。Server 进入 CLOSE_WAIT 状态。从 Client 到 Server 这个方向的连接就释放了，TCP 连接处于半关闭状态。（Server 若发送数据，客户仍要接收。）  

**第三次挥手：Server 发送 FIN = 1，ACK = 1，seq = w，ack = u + 1 给 Client**

说明：Server 发送一个 FIN ，用来关闭 Server 到 Client 的数据传输。Server 进入LAST_ACK 状态。若 Server 已经没有要向 Client 发送的数据，其应用进程就通知 TCP 释放连接。

**第四次挥手： Client 发送 ACK = 1，seq = u + 1，ack = w + 1 给 Server**       

说明： Client 收到连接释放报文段后，必须发出确认。在确认报文段中 ACK = 1，确认号 ack = w（seq） + 1。自己的序号 seq = u + 1。 随之服务器 TCP 关闭。

> 为什么建立连接是三次握手，而关闭连接却是四次挥手呢？

  > 因为服务端在 LISTEN 状态下，收到建立连接请求的 SYN 报文后，把 ACK 和 SYN 放在一个报文里发送给客户端。而关闭连接时，当收到对方的 FIN 报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即 close，也可以发送一些数据给对方后，再发送 FIN 报文给对方来表示同意现在关闭连接，因此，己方 ACK 和 FIN 一般都会分开发送。

#### 4.4.3 超时重传

超时重传是 TCP 协议保证数据可靠性的一个重要机制，其原理是在发送某一个数据以后就开启一个计时器，在一定时间内如果没有得到发送的数据报的 ACK 报文，那么就重新发送数据，直到发送成功为止。

这里我们通过图来了解一下超时重传的机制：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510045422148.png/wm)

实现超时间重传需要注意：

+ 发送一个报文段后，暂时保存该报文段的副本，为发生超时重传时使用，收到确认报文后删除该报文段。

+ 确认报文段也需要序号，才能明确是发出去的那个数据报得到了确认。

+ 超时计时器比传输往返时间略长，但具体值是不确定的，根据网络情况而变。

### 4.5 TCP/IP 协议栈

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510046086498.png/wm)

## 5 网络协议

网络层定义了`端对端`的包传输，同时定义了能够表示所有节点的逻辑地址，以及一套相应的寻址系统，和路由实现的方式。

每台计算机都会有两种地址： `MAC 地址`和`网络地址`，两个地址间没有联系。 MAC 地址绑定在网卡上，而以太网协议是依靠 MAC 地址且采用广播的方式来发送数据，同时确定子网络的目标网卡；网络地址是由管理源来进行分配，帮助我们确定计算机所在的子网络。

### 5.1 MAC 地址

MAC 地址（`Media Access Control Address`）：**媒体访问控制地址，或称为硬件地址，是用来定义网络设备的位置的**。在 OSI 模型中，第三层网络层负责 IP 地址，第二层数据链结层则负责 MAC 地址。一个主机会有一个 IP 地址，而每个网络位置会有一个专属于它的 MAC 地址。  

**格式**  

MAC 地址共 48 位（6 个字节），以十六进制表示。前 24 位由 IEEE 决定如何分配，后 24 位由实际生产该网络设备的厂商自行指定。

eg：

`ff:ff:ff:ff:ff:ff`  作为广播地址

`01:xx:xx:xx:xx:xx`  多播地址

`01:00:5e:xx:xx:xx`  IPv4 多播地址

### 5.2 IP 协议

规定网络地址的协议被称为 IP 协议（Internet Protocol：网际协议），所定义的地址称为 IP 地址。
IP 是网络层中的核心协议，也是 TCP/IP 协议簇中的最核心的协议之一。

 Internet Protocol 需要注意几件事：

+ IP 协议是`无连接`的类型
+ IP 协议采用的是一种尽力而为的服务，不能保证丢包与乱序的情况不会发生

+ **面向连接**：在面向连接的方法中，网络负责顺序发送分组报文分组并且以一种可靠的方法检测丢失和冲突。
+ **无连接**：在无连接的方法中，网络只需要将报文分组发送到接收点，检测与数据流控制由发送方和接收方处理。这种方法被称作“最佳工作(best-effort)”或“无应答(unacknowledged)”的传输协议所使用。

#### 5.2.1 IP 地址

**IP 地址就是互联网协议地址**，是分配给每台连接到使用 Internet 协议进行通信的计算机网络的设备的地址编号。 IP 地址服务的两个主要功能：主机或网络接口识别和位置寻址。

IP 地址是一个 `32` 位的二进制数，通常被分割为 4 个“8位二进制数”（也就是 4 个字节）。IP 地址通常用“点分十进制”表示成（a.b.c.d）的形式，其中，a,b,c,d 都是 `0~255 ` 之间的十进制整数。例如：`192.168.42.3`。

在 Linux 系统中，可以用 `ifconfig ` 命令来查看自己的 IP 地址：

```
$ ifconfig
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510045646186.png/wm)

图中可以看到自己的 IP 地址是：`192.168.42.3`。

**IP 命令**

IP 命令主要用来显示或操纵 Linux 主机的路由、网络设备、策略路由和隧道。

命令格式

```
ip [option][object][command][arguments]
```

| object      | describe             |
| ----------- | -------------------- |
| `link`      | 网络设备             |
| `address`   | 设备的协议地址       |
| `route`     | 路由表条目           |
| `neighbour` | ARP/NDISC 缓冲区条目 |

`COMMAND`：设置对指定对象执行的操作，IP 支持对象的增加(add)、删除(delete)和展示(show 或者 list)。

**IP 命令实例**

1.显示网络设备的运行状态

```
$ ip link list
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510390457151.png/wm)

2.输出更为详细的网络信息

```
$ ip -s link list
```
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510390493789.png/wm)

3.显示邻居表(ARP 表)

```
$ ip neigh list
```
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid276733labid3926timestamp1510390599216.png/wm)

IP 地址查看操作视频：

`@
http://labfile.oss.aliyuncs.com/courses/980/week3/1-1.mp4
@`

#### 5.2.2 IPv4 与 IPv6

IP 地址是一种在 Internet 上的给主机编址的方式，也称为网络协议地址。**常见的 IP 地址，分为 IPv4 与 IPv6 两大类。**

**IPv4**

IPv4 中 IP 由 `32 位`组成，为了便于理解与记忆，采用了点分十进制的形式，也就是四个字节被分开用十进制表示，直接用点来分割开。因为每个字节是由 8 位组成，所以一个字节的最大值是 2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 2^8 - 1 = `255`（8位二进制数的最大值），所以**每个字节的取值范围只能在 0~255 之间。**

在最初 IP 地址被分为：`网络号和主机号`两个部分，随后在出现的分类网络中系统又定义了五个类别：`A、B、C、D 和 E` 类。A、B 和 C 类有不同长度的网络号，剩余的部分被用来识别网络内的主机，意味着每个网络类别有着不同的给主机编址的能力。D 类被用于[多播地址](https://en.wikipedia.org/wiki/Multicast_address)，E 类被留作将来使用。

分类格式如图所示：

![图片描述](https://dn-simplecloud.shiyanlou.com/uid/8797/1522136557712.png-wm)

| 类别 | 地址范围                    |
| ---- | --------------------------- |
| A类  | 1.0.0.0 ~ 127.255.255.255   |
| B类  | 128.0.0.0 ~ 191.255.255.255 |
| C类  | 192.0.0.0 ~ 223.255.255.255 |
| D类  | 224.0.0.0 ~ 239.255.255.255 |
| E类  | 240.0.0.0 ~ 255.255.255.254 |

其他特殊用途的 IP 地址：

> CIDR (Classless Inter-Domain Routing，无类别域间路由)，是用于无分类编址，废弃了 IP 地址的分类方法。下面的地址块正是采用 CIDR 的方法。

| CIDR 地址块     | 描述                         | 参考资料 |
| --------------- | ---------------------------- | -------- |
| 0.0.0.0/8       | 本网络（仅作为源地址时合法） | RFC 5735 |
| 10.0.0.0/8      | 专用网络                     | RFC 1918 |
| 127.0.0.0/8     | 环回                         | RFC 5735 |
| 172.16.0.0/12   | 专用网络                     | RFC 1918 |
| 192.168.0.0/16  | 专用网络                     | RFC 1918 |
| 255.255.255.255 | 广播                         | RFC 919  |

**IPv6**

IPv6 是以 16 进制值来书写的，被 7 个冒号分隔成 8 组，每组 2 个字节，所以 IPv6 的地址是由 8 组十六进制段来组成的，

例如：

```
2001:0DB8:0123:4567:89ABCD:B3FF:FE1E:8329
```

注意：

- 每个十六进制段的值是大小写无关
- 为了简化，可以忽略每组十六进制段中的前导 `0`，如上例子中的 `0DB8`，可以直接写成 `DB8`
- 双冒号可以代替连续的 `0`，但是每个地址中只能出现一次双冒号。如 `0000:0000:0000:0000:0000:0000:0000:0001` 这个地址的简化写法，可以直接写成 `::1`。例如 `::ffff:192.168.1.1`。

IPv6 的特性：

- 极大扩展的地址空间：地址格式从 32 比特扩展到 128 比特；
- 自动配置：就是无状态自动配置机制；
- 简化了报头格式：与 IPv4 报头相比，IPv6 报头更为简单，长度固定为 40 字节；
- 增强了选项与扩展报头的支持：IPv4 将选项集成到基本报头中，IPv6 则通过所谓的扩展报头来承载选项；
- 流标签的使用：让我们可以为数据包所属类型提供个性化的网络服务；
- 认证与私密性：IPv6 把 IPSec 作为必备协议，保证了网络层端到端通信的完整性和机密性；
- IPv6 在移动网络和实时通信方面有很多改进。

#### 5.2.3 子网划分

子网（Subnetwork）指的是将一个大型的网络切分为逻辑上的多个小的网络组。在最初引入这个概念的时候，IPv4 还未引入分类网络这个概念。而引入划分子网这个概念的目的是为了允许一个单一的站点能拥有多个局域网。即使在引入了分类网络号之后，这个概念仍然有它的用处，因为它减少了因特网路由表中的表项数量。此外它还带来了一个好处，那就是减少了网络开销，因为它将接收 IP 广播的区域划分成了若干部分（广播是将一条消息向本局域网内所有的节点发送）。

若是划分了子网，IP 地址中需要有位置来标识子网，那么 IP 的组成变成了三部分：`网络号`、`子网号`、`主机号`。

**网络掩码又叫子网掩码（subnet mask）主要是用来指明一个 IP 地址中网络号位置**，若是网络号位的长度与标准的不同，那么便表示子网的存在。

子网掩码的表示：
在 IP 地址的二进制表示法中，表示为`网络号`与`子网号`的位置。
在子网掩码的二进制则表示与之对应的位置都标识为 1。
也可以换算成十进制的点分四组表示。

例如：一个 A 类网络中的 IP 地址 `10.1.1.0`，因为前八位都是网络号位，所以子网掩码的二进制表示法应该是 `11111111  00000000  00000000  00000000`，换算为十进制便是 `255.0.0.0`，它的可变长子网掩码 VLSM 表示法是 `10.1.1.0/8`。

所以通常 A、B、C 三类地址还未划分子网时他们的标准子网掩码是：

| 地址类型 | 标准的子网掩码 | CIDR 表示 |
| -------- | -------------- | --------- |
| A 类     | 255.0.0.0      | ip/8      |
| B 类     | 255.255.0.0    | ip/16     |
| C 类     | 255.255.255.0  | ip/24     |

子网掩码的值与 `固定值位+网络号+子网号` 的位数相关（固定值位就如 A 类，开头占了 1 位，B 类开头占了 2 位，C 类开头占据了 3 位），若是没有划分子网，便没有子网号，此时 `子网掩码中标志为 1 的位数 = 固定值位 + 网络号位`，这是一个参考值，所以叫做标准子网掩码。若是划分了子网掩码，那么必然有子网号了，此时 `子网掩码中标志为 1 的位数 = 固定值位+网络号位+子网号位`，那么与标准的子网掩码不同，就可以推算出它划分了子网，子网号有几位。

所以没有划分子网的 A 类地址 固定值位 + 网络号 有 8 位，所以其标准子网掩码就是 `255.0.0.0`，二进制是 `11111111  00000000  00000000  00000000`，VLSM 的表示就是 ip/8，同理 B 类地址占据了 16 位，所以其标准的子网掩码是 `255.255.0.0`。

举例：

一个公司有一个 C 类的地址，假设 IP 地址所在的网段是 192.168.15.0，他有 10 家子公司，每个子公司都要独立的子网，此时我们该如何确定子网掩码，以及每个子网的范围呢？

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid457169labid4961timestamp1522734717303.png/wm)
这就是子网的划分，大家可以尝试一下能否通过子网掩码反推出当前的子网号所占据的位数，可以划分多少个子网，主机位占据了多少位，每个子网可以容纳多少台主机。

### 5.3 网络层常用的协议

- EIGRP（Enhanced Interior Gateway Routing Protocol）
- ICMP Internet Control Message Protocol
- IGMP Internet Group Management Protocol
- IGRP Interior Gateway Routing Protocol
- IPv4 Internet Protocol version 4
- IPv6 Internet Protocol version 6
- IPSec Internet Protocol Security
- IPX Internetwork Packet Exchange
- GRE Generic Routing Encapsulation for tunneling
- OSPF Open Shortest Path First
- RIP Routing Information Protocol

## 6 课后作业

一个 B 类地址：172.25.0.0，若是其子网掩码是 255.255.224.0。请计算其子网号有多少位，可以划分多少子网，每个子网中有多少台主机，每个子网的 IP 地址范围是多少？

## 7 总结

通过本节实验的学习，可以学习到 TCP/IP 协议的具体内容，以及十分重要的三次握手和四次挥手机制，同时对 OSI 七层模型体系结构有了深刻的理解。同时学习了相应的网络知识，IP 地址、子网划分、IPV4、网关等等。