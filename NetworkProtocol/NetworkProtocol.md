[趣谈网络协议](https://time.geekbang.org/column/intro/85)笔记
-----



## 0-开始



学习知识的方法和态度：**看似最枯燥、最基础的东西往往具有最长久的生命力。**



## 1-为什么要学习网络协议？



**只有通过这种协议，计算机才知道我们想让它做什么。**



### 协议三要素

这种协议还是更接近人类语言，机器不能直接读懂，需要进行翻译，翻译的工作教给编译器（compile）。

![](images/01-01-compile.png)

计算机语言作为程序员控制一台计算机工作的协议，具备了协议的三要素。

**语法**，就是这一段内容要符合一定的**规则**和**格式**。例如，括号要成对，结束要使用分号等。 

**语义**，就是这一段内容要代表某种意义。例如数字减去数字是有意义的，数字减去文本一般来说就没有意义。 

**顺序**，就是先干啥，后干啥。例如，可以先加上某个数值，然后再减去某个数值。 



**只有通过网络协议，才能使一大片机器互相协作、共同完成一件事。**



用网易考拉的HTTP一段，来描述协议的三要素：

```
HTTP/1.1 200 OK
Date: Tue, 27 Mar 2018 16:50:26 GMT
Content-Type: text/html;charset=UTF-8
Content-Language: zh-CN
 
<!DOCTYPE html>
<html>
<head>
<base href="https://pages.kaola.com/" />
<meta charset="utf-8"/> <title> 网易考拉 3 周年主会场 </title>
```

首先，符合语法，也就是说，只有按照上面那个格式来，浏览器才认。例如，上来是**状态**，然后是**首部**，然后是**内容**。 

第二，符合语义，就是要按照约定的意思来。例如，状态 200，表述的意思是网页成功返回。如果不成功，就是我们常见的“404”。 

第三，符合顺序，你一点浏览器，就是发送出一个 HTTP 请求，然后才有上面那一串 HTTP 返回的东西。 



### 常用的网络协议

**DNS** 	地址簿协议

**HTTPDNS** 	更加精准的地址簿查找协议

**HTTP**

**HTTPS**

**UDP**

**TCP**

**IP**

**DHCP**



![](https://ws2.sinaimg.cn/large/006tNc79gy1fz8kt46apfj30do08gjsb.jpg)



![image-20190116180012059](https://ws3.sinaimg.cn/large/006tNc79gy1fz8kwrnp1wj30t40jmqbl.jpg)



极客时间，网友

**mac地址是唯一的，为什么可以修改?**  

想想身份证，身份证号是唯一的，不能改变的，但是可以造假。mac地址全球唯一，它是固化在网卡里的。网卡毕竟是个硬件，需要软件支持，既操作系统识别。重点来了，操作系统识别出来的mac地址是可以更改的，它只不过是一个字符串。我们常说的修改mac指的是修改电脑中记录的既注册表中的记录。



**有了mac地址为什么还要有ip地址。**  

举个例子，身份证号是你的唯一标识，不会重复，一落户就有（网卡一出厂就有mac）。现在我要和你通信（写信给你），地址用你的姓名+身份证，信能送到你手上吗?明显不能！身份证号前六位能定位你出生的县。mac地址前几位也可以定位生产厂家。但是你出生后会离开这个县（哪怕在这个县，也不能具体找到你）。所以一般写个人信息就要有出生地和现居地址了。



## 2-网络分层的真实含义是什么？



计算机网络**不仅需要背诵，而且特别需要将原理烂熟于胸的学科**。



TCP 在进行三次握手的时候，IP 层和 MAC 层对应都有什么操作呢？



### **网络为什么要分层？**



因为，是个复杂的程序都要分层。



理解计算机网络中的概念，一个很好的角度是，想象网络包就是一段 Buffer，或者一块内存，是有格式的。同时，想象自己是一个处理网络包的程序，而且这个程序可以跑在电脑上，可以跑在服务器上，可以跑在交换机上，也可以跑在路由器上。你想象自己有很多的网口，从某个口拿进一个网络包来，用自己的程序处理一下，再从另一个网口发送出去。



### **程序是如何工作的？**



![](https://ws2.sinaimg.cn/large/006tNc79gy1fz8npx8xr6j30u70u0ag0.jpg)



如果 IP 地址不是自己的，那就应该转发出去；如果 IP 地址是自己的，那就是发给自己的。



在四层的头里面有端口号，不同的应用监听不同的端口号。



只要 Buffer 里面的内容完整，就可以从网口发出去了，你作为一个程序的任务就算告一段落了。



### **揭秘层与层之间的关系**



**所有不能表示出层层封装含义的比喻，都是不恰当的。**



**只要是在网络上跑的包，都是完整的。可以有下层没上层，绝对不可能有上层没下层。**



## 3- ifconfig：最熟悉又陌生的命令行

`ifconfig`   

`ipconfig`

`ip addr`





```shell
root@test:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:c7:79:75 brd ff:ff:ff:ff:ff:ff
    inet 10.100.122.2/24 brd 10.100.122.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::f816:3eff:fec7:7975/64 scope link 
       valid_lft forever preferred_lft forever
```



IP 地址是一个网卡在网络世界的通讯地址，相当于我们现实世界的门牌号码。



![](https://ws4.sinaimg.cn/large/006tNc79gy1fz8o4xva50j30ua0cqwfg.jpg)





![](https://ws4.sinaimg.cn/large/006tNc79gy1fz8o5unh82j31080aadib.jpg)



### **无类型域间选路**



无类型域间选路(Classless Inter-Domain Routing)，简称**CIDR**



`10.100.122.2/24`   后面 24 的意思是，32 位中，前 24 位是网络号，后 8 位是主机号。



伴随着 CIDR 存在的，一个是**广播地址**，10.100.122.255。如果发送这个地址，所有 10.100.122 网络里面的机器都可以收到。另一个是**子网掩码**，255.255.255.0。



### **公有 IP 地址和私有 iP 地址**



![](https://ws2.sinaimg.cn/large/006tNc79gy1fz8ob2jnpkj31080aagoh.jpg)



192.168.0.1，往往就是你这个**私有网络的出口地址**， **192.168.0.255** 就是广播地址。



D 类是**组播地址**



**scope** 



lo 全称是**loopback**，又称**环回接口**，往往会被分配到 127.0.0.1 这个地址。这个地址用于本机通信，经过内核处理后直接返回，不会在任何网络中出现。



### **MAC 地址**



`link/ether fa:16:3e:c7:79:75 brd ff:ff:ff:ff:ff:ff`



### **网络设备的状态标识**



`<BROADCAST,MULTICAST,UP,LOWER_UP>`  叫作**net_device flags**，**网络设备的状态标识**。



**UP** 表示网卡处于启动的状态；**BROADCAST** 表示这个网卡有广播地址，可以发送广播包；**MULTICAST** 表示网卡可以发送多播包；**LOWER_UP** 表示 L1 是启动的，也即网线插着呢。

MTU1500 是指最大传输单元 MTU 为 1500，这是以太网的默认值。MTU 是二层 MAC 层的概念。

以太网规定连 MAC 头带正文合起来，不允许超过 1500 个字节。正文里面有 IP 的头、TCP 的头、HTTP 的头。如果放不下，就需要分片来传输。



qdisc 全称是**queueing discipline**，中文叫**排队规则**。



三个波段（band）的优先级也不相同。band 0 的优先级最高，band 2 的最低。如果 band 0 里面有数据包，系统就不会处理 band 1 里面的数据包，band 1 和 band 2 之间也是一样。 



数据包是按照服务类型（**Type of Service，TOS**）被分配到三个波段（band）里面的。TOS 是 IP 头里面的一个字段，代表了当前的包是高优先级的，还是低优先级的。 



## 4-DHCP与PXE：IP是怎么来的，又是怎么没的？