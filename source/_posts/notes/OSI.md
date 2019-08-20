---
title: OSI - Open System Interconnection Reference Model
permalink: OSI open-system-interconnection-reference-model
categories:
- program
tags: 
- OSI
comments: true
date: 2019-07-22
updated: 2019-07-22
---

**开放式系统互联通信参考模型**（英语：**O**pen **S**ystem **I**nterconnection Reference Model，缩写：OSI；简称为**OSI模型**）是一种[概念模型](https://zh.wikipedia.org/wiki/概念模型)，由[国际标准化组织](https://zh.wikipedia.org/wiki/国际标准化组织)提出，一个试图使各种计算机在世界范围内互连为网络的标准框架。定义于ISO/IEC 7498-1。

## 历史

在制定[计算机网络](https://zh.wikipedia.org/wiki/计算机网络)标准方面，起着重大作用的两大国际组织是：[国际电信联盟电信标准化部门](https://zh.wikipedia.org/wiki/國際電信聯盟電信標準化部門)，与[国际标准组织](https://zh.wikipedia.org/wiki/国际标准组织)（ISO），虽然它们工作领域不同，但随着科学技术的发展，通信与信息处理之间的界限开始变得比较模糊，这也成了[国际电信联盟电信标准化部门](https://zh.wikipedia.org/wiki/國際電信聯盟電信標準化部門)和ISO共同关心的领域。1984年，ISO发布了著名的***ISO/IEC 7498***标准，它定义了网络互联的7层框架，也就是开放式系统互联参考模型。



## 层次划分

根据建议[X.200](https://www.itu.int/rec/T-REC-X.200-199407-I)，OSI将计算机网络体系结构划分为以下七层，标有1～7，第1层在底部。 现“OSI/RM”是[英文](https://zh.wikipedia.org/wiki/英文)“Open Systems Interconnection Reference Model”的缩写。

### 第7层 应用层（Application Layer）

应用层（Application Layer）提供为应用软件而设的接口，以设置与另一应用软件之间的通信。例如: HTTP，HTTPS，FTP，TELNET，SSH，SMTP，POP3.HTML.等。

### 第6层 表达层（Presentation Layer）

表达层（Presentation Layer）把数据转换为能与接收者的系统格式兼容并适合传输的格式。

### 第5层 会话层（Session Layer）

会话层（Session Layer）负责在数据传输中设置和维护计算机网络中两台计算机之间的通信连接。

### 第4层 传输层（Transport Layer）

传输层（Transport Layer）把传输表头（TH）加至数据以形成数据包。传输表头包含了所使用的协议等发送信息。例如:传输控制协议（TCP）等。

### 第3层 网络层（Network Layer）

网络层（Network Layer）决定数据的路径选择和转寄，将网络表头（NH）加至数据包，以形成分组。网络表头包含了网络数据。例如:互联网协议（IP）等。

### 第2层 数据链路层（Data Link Layer）

数据链路层（Data Link Layer）负责网络寻址、错误侦测和改错。当表头和表尾被加至数据包时，会形成帧。数据链表头（DLH）是包含了物理地址和错误侦测及改错的方法。数据链表尾（DLT）是一串指示数据包末端的字符串。例如以太网、无线局域网（Wi-Fi）和通用分组无线服务（GPRS）等。

分为两个子层：逻辑链路控制（logical link control，LLC）子层和介质访问控制（Medium access control，MAC）子层。

### 第1层 物理层（Physical Layer）

物理层（Physical Layer）在局部局域网上传送[数据帧](https://zh.wikipedia.org/wiki/数据帧)（data frame），它负责管理计算机通信设备和网络媒体之间的互通。包括了针脚、电压、线缆规范、集线器、中继器、网卡、主机接口卡等。

## 影响

OSI是一个定义良好的协议规范集，并有许多可选部分完成类似的任务。它定义了开放系统的层次结构、层次之间的相互关系以及各层所包括的可能的任务，作为一个框架来协调和组织各层所提供的服务。

OSI参考模型并没有提供一个可以实现的方法，而是描述了一些概念，用来协调进程间通信标准的制定。即OSI参考模型并不是一个标准，而是一个在制定标准时所使用的概念性框架。



##References

- [DoD模型](https://zh.wikipedia.org/w/index.php?title=DoD模型&action=edit&redlink=1)（Department of Defense Model）四层是为了[ARPANET](https://zh.wikipedia.org/wiki/ARPANET)所开发出来的模型。
- [网络传输协议](https://zh.wikipedia.org/wiki/網路傳輸協議)
- [TCP/IP](https://zh.wikipedia.org/wiki/TCP/IP)，与OSI模型有类似结构的现行网络模型
- [OSI模型](https://zh.wikipedia.org/wiki/OSI模型)