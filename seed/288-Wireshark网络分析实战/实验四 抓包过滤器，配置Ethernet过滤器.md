---
show: step
version: 0.1
enable_checker: true
---

#第二章 抓包过滤器的用法（一）

## 一、课程简介

本节涵盖以下内容：

- 抓包过滤器简介
- 配置Ethernet过滤器；

第1章介绍了如何安装Wireshark软件，如何配置该软件以行使其基本功能，以及该软件在网络中的部署（或安装）位置。本章及下一章将讨论Wireshark抓包过滤器和显示过滤器的用法。

抓包过滤器和显示过滤器的重要区别如下所列。
>  抓包过滤器配置于抓包之前，一经应用，Wireshark将只抓取经过抓包过滤器过滤的数据（包或数据帧），其余数据一概不抓。本章将介绍这一过滤器。
>
>  显示过滤器配置于抓包之后，此时，Wireshark已抓得所有数据。网管人员会利用显示过滤器，让Wireshark只显示自己心仪的数据。这种过滤器将在下一章介绍。


>**注意**	
> 抓包过滤器的配置语法派生自libpcap/WinPcap库中tcpdump的语法，而显示过滤器的配置语法则在若干年后定义。因此，两种过滤器的配置语法并不相同。	 

在某些情况下，可能只想让Wireshark抓取某块网卡所接收到的部分数据，举例如下。
>  只想让Wireshark从某条数据量极大的受监控链路上，抓取必要的数据包时。
>
>  只想让Wireshark从某个受监控的VLAN内，抓取发往/来自某指定服务器的数据包时。
>
>  只想让Wireshark抓取由某种或某几种应用（程序）生成的数据包时（比如，若网管人员怀疑网络中存在的故障与DNS有关，便配置了抓包过滤器，让Wireshark只抓取进出Internet的DNS查询及响应消息）。

除了上面例举的几种情况之外，还有很多时候，也只需让Wireshark从网络中抓取特定而非全部数据包。抓包过滤器一经配置并加以应用，Wireshark将只会抓取经过其过滤的数据包，其余数据包一概不抓，网管人员可借此来采集自己“心仪的”数据包。


>**注意**	
>使用抓包过滤器时，请务必小心。在许多情况下，运行于网络中的某些应用程序会与某些意想不到的东西（比如，某台服务器）之间有着微妙的关联。因此，在使用Wireshark排除网络故障时，若开启了抓包过滤器，请确保未过滤掉某些看似不想关的数据包，否则的话，将发现不了导致故障的真正原因。现举一个常见的简单示例，若故障的表象为HTTP应答缓慢，但罪魁祸首却是DNS服务器不响应DNS查询，那么配置抓包过滤器，让Wireshark只抓取发往/来自TCP 80端口的流量，再怎么分析抓包文件都将是徒劳无功。	 

本章会讲解几种抓包过滤器的配置方法。

##二、配置抓包过滤器

配置抓包过滤器之前，请读者务必考虑以下两个问题：需要让Wireshark抓取什么样的数据包；配置抓包过滤器的目的是什么。请别忘了，通不过抓包过滤器过滤检查的数据包，将会被Wireshark丢弃。

读者既可以使用Wireshark自带的预定义抓包过滤器，也可以根据本章内容自行定义抓包过滤器。

###2.1  配置准备   
打开Wireshark软件，按本章内容行事。

###2.2  配置方法

抓包过滤器应在抓包之前配置，配置步骤如下所列。
>1．要配置抓包过滤器，请点击工具条上左边第二个Show the capture options…按钮，如图所示。
>![4-2.2-1](https://dn-simplecloud.shiyanlou.com/uid/596222/1525745197322.png-wm)
>
>2．Wireshark：Capture Options窗口会立刻弹出，如图所示。
>![4-2.2-2](https://dn-simplecloud.shiyanlou.com/5962221525745231814-wm)
>
>3．选择一块网卡，并双击之，有待配置的抓包过滤器将会生效于这块网卡（可按第1章所述来验证哪块网卡为有效的抓包网卡）。
>
>4．Edit Interface Settings窗口会立刻弹出，如图所示。
>![4-2.2-3](https://dn-simplecloud.shiyanlou.com/uid/596222/1525745451658.png-wm)
>
>5．现在，可在Capture Filter文本框内输入抓包过滤器所含字串，或点击Capture Filter按钮，在弹出的Capture Filter窗口中输入抓包过滤器所含字串和过滤器名称，如图所示。
>![4-2.2-4](https://dn-simplecloud.shiyanlou.com/uid/596222/1525745629797.png-wm) 


###2.3  幕后原理   

可凭籍Edit Interface Settings窗口中的Capture Filter文本框，并基于伯克利数据包过滤器（Berkeley Packet Filter，BPF）的语法，来配置抓包过滤器。在填写完抓包过滤器所含字符串之后，点击Compile BPF按钮，BPF编译器将会检查所填字符串的语法，若通不过检查，在弹出的Complie Results窗口中，会看到“syntax error（语法有误）”的字样。

除此以外，在Capture Filter文本框内输入抓包过滤器所含的字符串时，若语法正确，该文本框会变绿，否则将会变红。

伯克利数据包过滤器（BPF）只会对输入进Capture Filter文本框内的字符串的语法进行检查，不会检查其条件是否满足。比方说，若在Capture Filter文本框内只输入host不加任何参数，该文本框将会变红，且通不过BPF编译器的检查；但若输入的是host 192.168.1.1000，该文本框将会变绿，且能够通过BPF编译器的检查。


>**注 意**	
>
>BPF所遵循的语法，来源于Steven McCanne 和Van Jacobson于1992年在伯克利大学劳伦斯伯克利实验室所写论文The BSD Packet Filter: A New Architecture for User-level Packet Capture。

构成抓包过滤器的字符串名为过滤表达式。这一表达式决定了Wireshark对数据包的态度（是抓取还是放弃）。过滤表达式由一个或多个原词（primitive）构成。一般而言，每个原词都会包含一个标识符（名称或数字），这一标识符可能会位列一或多个限定符之后。限定符的种类有以下3种。
>  **type**（类型）限定符：标识符（其形式为名称或数字）所指代的事物。可能存在的类型限定符包括：主机名或主机地址标识符指代的host限定符、网络号标识符指代的net限定符、TCP/UDP端口号标识符指代的port限定符等。
>
>  **dir**（方向）限定符：指明了发往和/或来自某个标识符（所指代的主机）的数据包的具体流动方向。譬如，Dir限定符src和dst分别表示数据包源于/发往某个标识符（所指代的主机）。
>
>  **Proto**（协议类型）限定符：精确指明了数据包所匹配的协议类型。比方说，Proto限定符ether、ip和arp分别用来指明以太网帧、IP（Internet协议）数据包和ARP（地址解析协议）帧。

标识符是用来进行匹配的实际条件。标识符既可以是一个IP地址（比如，10.1.1.1），也可以是一个TCP/UDP端口号（比如53），还可以是一个IP网络地址（比如，用来表示IP网络192.168.1.0/24的192.168.1）。

请看下面这个抓包过滤器。

tcp dst port 135

其中，dst为dir限定符；port为type限定符；tcp为Proto限定符。

###2.4  拾遗补缺   

可配置Wireshark，让不同的抓包过滤器生效于不同的网卡，如图所示。
 ![4-2.4-1](https://dn-anything-about-doc.qbox.me/userid2418labid907time1429418055444?watermark/1/image/aHR0cDovL3N5bC1zdGF0aWMucWluaXVkbi5jb20vaW1nL3dhdGVybWFyay5wbmc=/dissolve/60/gravity/SouthEast/dx/0/dy/10)


当Wireshark主机安装了双网卡，且需让两块网卡分别抓包时，就有可能需要如此配置。

抓包过滤器（所含字符串）都保存在Wireshark安装目录下的cfilters文件内。该文件不但会保存预定义的抓包过滤器，还会保存用户手工配置的过滤器，可将该文件复制进其他的Wireshark主机。cfilters文件的具体位置要视Wireshark主机的操作系统及Wireshark软件的安装路径而定。

###2.5  进阶阅读   

>1．Wireshark抓包过滤器（的运作机制）基于tcpdump程序，读者可参考以下链接：http://www.tcpdump.org/tcpdump_man.html
>
>2．  读者可参阅Wireshark手册页（链接为 http://wiki.wireshark.org/CaptureFilters ）来获取更多与抓包过滤器有关的信息。

##三、配置Ethernet过滤器

本书提及的Ethernet过滤器所指为第二层过滤器，即根据MAC地址来行使过滤功能的抓包过滤器。本节会介绍这种过滤器及其配置方法。

###3.1  配置准备   

下面列出了一些简单的第二层过滤器。
>-  ether host <Ethernet host>：让Wireshark只抓取源于或发往由标识符Ethernet host所指定的以太网主机的以太网帧（即所抓以太网流量的源或目的MAC地址，与Ethernet host所定义的MAC地址相匹配）。
>
>-  ether dst <Ethernet host>:让Wireshark只抓取发往由标识符Ethernet host所指定的以太网主机的以太网帧（即所抓以太网流量的目的MAC地址，与Ethernet host所定义的MAC地址相匹配）。
>
>-  ether src <Ethernet host>:让Wireshark只抓取由标识符Ethernet host所指定的以太网主机发出的以太网帧（即所抓以太网流量的源MAC地址，与Ethernet host所定义的MAC地址相匹配）。
>
>-  ether broadcast: 让Wireshark只抓取所有以太网广播流量。
>
>-  ether multicast: 让Wireshark只抓取所有以太网多播流量。
>
>-  ether proto <protocol>:所抓以太网流量的以太网协议类型编号，与标识符<protocol>所定义的以太网协议类型编号相匹配。
>
>-  vlan <vlan_id>:让Wireshark只抓取由标识符<vlan_id>所指定的VLAN的流量。

要想让抓包过滤器中的字符串起反作用，需在原词之前添加关键字not或符号“!”。举例如下。

抓包过滤器Not ether host <Ethernet host> 或 ! Ether host <Ethernet host>的意思是：让Wireshark舍弃源自或发往由标识符Ethernet host所指定的以太网主机的以太网流量（即所抓以太网流量的源或目的MAC地址，与Ethernet host所定义的MAC地址不匹配）。

###3.2  配置方法   

请看图2.6所示的网络，该网络中的一台路由器、一台服务器外加多台PC都连接到了同一台LAN交换机上。此外，有一台安装了Wireshark的笔记本也接入了该LAN交换机。在LAN交换机上已开启了端口镜像功能，并将整个VLAN 1（该LAN交换机上的所有端口都隶属于VLAN 1）的流量都重定向给了那台Wireshark主机。

图中（紧随IP地址）的符号/24所指为该IP地址的24位子网掩码，其二进制和十进制的写法为：11111111.11111111.11111111.00000000和255.255.255.0。

下图所示的网络为基础，根据特定需求所配置的若干抓包过滤器。

![4-3.2-1](https://dn-anything-about-doc.qbox.me/userid2418labid907time1429418235848?watermark/1/image/aHR0cDovL3N5bC1zdGF0aWMucWluaXVkbi5jb20vaW1nL3dhdGVybWFyay5wbmc=/dissolve/60/gravity/SouthEast/dx/0/dy/10)

>1．要是只想让Wireshark抓取源于或发往某一具体MAC地址的流量，如源于或发往图中PC3的流量，抓包过滤器应如此配置：ether host 00:24:d6:ab:98:b6。
>
>2．要是只想让Wireshark抓取发往某一具体MAC地址的流量，如发往图中PC3的流量，抓包过滤器应如此配置：ether dst 00:24:d6:ab:98:b6。
>
>3．要是只想让Wireshark抓取源于某一具体MAC地址的流量，如源于图中PC3的流量，抓包过滤器应如此配置：ether src 00:24:d6:ab:98:b6。
>
>4．要是只想让Wireshark抓取以太网广播流量，抓包过滤器应如此配置：ether broadcast或ether dst ff:ff:ff:ff:ff:ff。
>
>5． 要是只想让Wireshark抓取以太网多播流量，抓包过滤器应如此配置：ether multicast。
>
>6．要是只想让Wireshark抓取特定以太网类型的流量（以太网类型用十六进制数表示），比如，只抓取以太网类型为0x0800的流量，抓包过滤器应如此配置ether proto 0800。

###3.3  幕后原理   

Wireshark Ethernet抓包过滤器的运作原理非常简单：Wireshark抓包引擎会先拿用户事先指定的源和/或目的主机MAC地址，与抓取的以太网流量的源和/或目的MAC地址相比较，再筛选出源和/或目的MAC地址相匹配的流量。

所谓以太网广播流量，是指目的MAC地址为广播地址（MAC地址为全1，其十六进制写法为ff:ff:ff:ff:ff:ff）的以太网流量。因此，只要启用了以太网广播过滤器，Wireshark只会抓取目的MAC地址为ff:ff:ff:ff:ff:ff的以太网流量。以下所列为常见的以太网广播流量。

>-  第三层IPv4广播流量，其所对应的第二层以太网帧为以太网广播帧；比方说，目的IP地址为192.168.1.255（此乃C类广播地址）的IPv4数据包，与其相对应的第二层以太网帧的目的MAC地址就是以太网广播地址ff:ff:ff:ff:ff:ff。
>-  有特殊用途的以太网广播流量，比如，IPv4 ARP（地址解析协议）流量，其目的MAC地址也是以太网广播地址ff:ff:ff:ff:ff:ff。


>**注 意**	
>
> 一般而言，有特殊用途的以太网广播流量对网络设备之间的“互通有无”来说必不可缺。除IPv4 ARP流量之外，此类流量还包括RIP路由协议流量等。	 

可利用多播过滤器，让Wireshark只抓取IPv4/IPv6 多播流量。
>-  但凡IPv4多播流量，其以太网帧的目的MAC地址必以01:00:5e打头。目的MAC地址以01:00:5e打头的所有以太网帧都将被视为以太网多播帧。
>-  但凡IPv6多播流量，其以太网帧的目的MAC地址均以33:33打头。目的MAC地址以33:33打头的所有以太网帧也将被视为以太网多播帧。

以太网类型所指为以太网帧帧头的ETHER-TYPE字段，其值用来表示由以太网帧帧头所封装的高层协议流量的协议类型。当ETHER-TYPE字段值为0x0800、0x86dd以及0x0806时，则以太网帧帧头所封装的分别是IPv4、IPv6以及ARP流量。

###3.4  拾遗补缺   

>-  要想让Wireshark只抓取某一特定VLAN的流量，抓包过滤器的语法应为：vlan <vlan number>。
>-  要想让Wireshark只抓取某几个VLAN的流量，抓包过滤器的语法应为：vlan <vlan number> and vlan <vlan number> and vlan <vlan number>…。


版权声明
>
> Copyright © Packt Publishing 2013. First published in the English language under the title Network Analysis Using Wireshark Cookbook.
>
> All Rights Reserved.
>
> 本书由英国Packt Publishing公司授权人民邮电出版社出版。未经出版者书面许可，对本书的任何部分不得以任何方式或任何手段复制和传播。
>
> 版权所有，侵权必究。


