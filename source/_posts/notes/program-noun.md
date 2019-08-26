---
title: programming noun
permalink: programming-noun
categories:
- program
tags: 
- programming-noun
comments: true
date: 2019-07-29
updated: 2019-07-29
---
List commonly used programming nouns
# Abbreviation

## Common

### HTTP
 [**HTTP**](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)（**H**yper**T**ext **T**ransfer **P**rotocol）：超文本传输协议
是一种用于分布式、协作式和[超媒体](https://zh.wikipedia.org/wiki/超媒體)信息系统的[应用层](https://zh.wikipedia.org/wiki/应用层)[协议](https://zh.wikipedia.org/wiki/网络传输协议)。

### CAP

**[CAP定理](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)**（CAP theorem），又被称作**布鲁尔定理**（Brewer's theorem），在[理论计算机科学](https://zh.wikipedia.org/wiki/理論計算機科學)中，它指出对于一个[分布式计算系统](https://zh.wikipedia.org/wiki/分布式计算)来说，不可能同时满足以下三点：

- 一致性（**C**onsistency） （等同于所有节点访问同一份最新的数据副本）
- [可用性](https://zh.wikipedia.org/wiki/可用性)（**A**vailability）（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）
- [分区容错性](https://zh.wikipedia.org/w/index.php?title=网络分区&action=edit&redlink=1)（**P**artition tolerance）（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择[[3\]](https://zh.wikipedia.org/wiki/CAP定理#cite_note-3)。）

根据定理，分布式系统只能满足三项中的两项而不可能满足全部三项[[4\]](https://zh.wikipedia.org/wiki/CAP定理#cite_note-4)。理解CAP理论的最简单方式是想象两个节点分处分区两侧。允许至少一个节点更新状态会导致数据不一致，即丧失了C性质。如果为了保证数据一致性，将分区一侧的节点设置为不可用，那么又丧失了A性质。除非两个节点可以互相通信，才能既保证C又保证A，这又会导致丧失P性质。

### ACID

**[ACID](https://zh.wikipedia.org/wiki/ACID)**，是指[数据库管理系统](https://zh.wikipedia.org/wiki/数据库管理系统)（[DBMS](https://zh.wikipedia.org/wiki/DBMS)）在写入或更新资料的过程中，为保证[事务](https://zh.wikipedia.org/wiki/数据库事务)（transaction）是正确可靠的，所必须具备的四个特性：[原子性](https://zh.wikipedia.org/w/index.php?title=原子性&action=edit&redlink=1)（atomicity，或称不可分割性）、[一致性](https://zh.wikipedia.org/w/index.php?title=一致性_(数据库系统)&action=edit&redlink=1)（consistency）、[隔离性](https://zh.wikipedia.org/wiki/隔離性)（isolation，又称独立性）、[持久性](https://zh.wikipedia.org/w/index.php?title=持久性&action=edit&redlink=1)（durability）

### RPC

**[RPC](https://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)**（Remote Procedure Call）：远程过程调用
是一个计算机通信[协议](https://zh.wikipedia.org/wiki/網絡傳輸協議)。该协议允许运行于一台计算机的[程序](https://zh.wikipedia.org/wiki/程序)调用另一台计算机的[子程序](https://zh.wikipedia.org/wiki/子程序)，而程序员无需额外地为这个交互作用编程。如果涉及的软件采用[面向对象编程](https://zh.wikipedia.org/wiki/面向对象编程)，那么远程过程调用亦可称作**远程调用**或**远程方法调用**，例：[Java RMI](https://zh.wikipedia.org/wiki/Java_RMI)。

### ORM

**[ORM](https://zh.wikipedia.org/wiki/%E5%AF%B9%E8%B1%A1%E5%85%B3%E7%B3%BB%E6%98%A0%E5%B0%84)**（Object Relational Mapping）：对象关系映射
是一种[程序设计](https://zh.wikipedia.org/wiki/程式設計)技术，用于实现[面向对象](https://zh.wikipedia.org/wiki/物件導向)编程语言里不同[类型系统](https://zh.wikipedia.org/wiki/類型系統)的数据之间的转换。从效果上说，它其实是创建了一个可在编程语言里使用的“虚拟[对象数据库](https://zh.wikipedia.org/wiki/物件資料庫)”。

### MVC

**[MVC](https://zh.wikipedia.org/wiki/MVC)**（Model–view–controller）：MVC模式
是[软件工程](https://zh.wikipedia.org/wiki/软件工程)中的一种[软件架构](https://zh.wikipedia.org/wiki/软件架构)模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

  - 控制器（Controller）- 负责转发请求，对请求进行处理。
  - 视图（View） - 界面设计人员进行图形界面设计。
  - 模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。

### GC

**[GC](https://zh.wikipedia.org/wiki/垃圾回收_(計算機科學))**（Garbage Collection）: 垃圾回收

在计算机科学中是一种自动的[存储器管理](https://zh.wikipedia.org/wiki/記憶體管理)机制。当一个计算机上的动态存储器不再需要时，就应该予以释放，以让出存储器，这种存储器资源管理，称为**垃圾回收**。垃圾回收器可以让程序员减轻许多负担，也减少程序员犯错的机会。垃圾回收最早起源于[LISP](https://zh.wikipedia.org/wiki/LISP)语言。当前许多语言如[Smalltalk](https://zh.wikipedia.org/wiki/Smalltalk)、[Java](https://zh.wikipedia.org/wiki/Java)、[C#](https://zh.wikipedia.org/wiki/C_Sharp)和[D语言](https://zh.wikipedia.org/wiki/D语言)都支持垃圾回收器。

### OOP

**[OOP](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)**（Object-oriented programming）：面向对象程序设计

是种具有[对象](https://zh.wikipedia.org/wiki/对象_(计算机科学))概念的[程序编程典范](https://zh.wikipedia.org/wiki/编程范型)，同时也是一种程序开发的抽象方针。

### AOP

**[AOP](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%9A%84%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)**（Aspect-oriented programming）：面向切面编程

是[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中的一种[程序设计思想](https://zh.wikipedia.org/wiki/编程范型)，旨在将**横切关注点**与业务主体进行进一步分离，以提高程序代码的模块化程度。通过在现有代码基础上增加额外的**通知**（Advice）机制，能够对被声明为“**切点**（Pointcut）”的代码块进行统一管理与装饰，如“对所有方法名以‘set*’开头的方法添加后台日志”。该思想使得开发人员能够将与代码核心业务逻辑关系不那么密切的功能（如日志功能）添加至程序中，同时又不降低业务代码的可读性。面向切面的程序设计思想也是面向切面软件开发的基础。

### REST

**[REST](https://zh.wikipedia.org/wiki/%E8%A1%A8%E7%8E%B0%E5%B1%82%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2)**（REpresentational State Transfer）：表现层状态转换

是一种万维网软件架构风格，目的是便于不同软件/程序在网络（例如互联网）中互相传递信息。

**URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。**

### SOAP

**[SOAP](https://zh.wikipedia.org/wiki/%E7%AE%80%E5%8D%95%E5%AF%B9%E8%B1%A1%E8%AE%BF%E9%97%AE%E5%8D%8F%E8%AE%AE)**（Simple Object Access Protocol）：简单对象访问协议
是交换数据的一种协议规范，使用在计算机网络Web服务（web service）中，交换带结构信息。
SOAP使用因特网应用层协议作为其传输协议。[SMTP](https://zh.wikipedia.org/wiki/SMTP)以及[HTTP](https://zh.wikipedia.org/wiki/HTTP)协议都可以用来传输SOAP消息，但是由于HTTP在如今的因特网结构中工作得很好，特别是在网络防火墙下仍然正常工作，所以被广泛采纳。SOAP亦可以在[HTTPS](https://zh.wikipedia.org/wiki/HTTPS)上传输。
SOAP的消息格式采用[XML](https://zh.wikipedia.org/wiki/XML)。
实际上计算机网络上交换基于XML的消息的协议，通常是用HTTP。

[SOAP 与 REST 的区别]()

### SOA

**[SOA](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E6%9C%8D%E5%8A%A1%E7%9A%84%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84)**（service-oriented architecture）：面向服务的体系结构

并不特指一种技术，而是一种分布式运算的软件设计方法

### API

**[API](https://zh.wikipedia.org/wiki/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E6%8E%A5%E5%8F%A3)**（）：

应用程序接口（英语：Application Programming Interface，缩写：**API**；又称为应用程序编程接口）是软件系统不同组成部分衔接的约定。 由於近年來软件的规模日益庞大，常常需要把复杂的系统划分成小的组成部分，编程接口的设计十分重要。

### [AJAX](https://zh.wikipedia.org/wiki/AJAX)

AJAX（Asynchronous JavaScript and XML）：异步的[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)与[XML](https://zh.wikipedia.org/wiki/XML)技术

指的是一套综合了多项技术的[浏览器](https://zh.wikipedia.org/wiki/瀏覽器)端[网页](https://zh.wikipedia.org/wiki/網頁)开发技术

### JSON

**[JSON](https://zh.wikipedia.org/wiki/JSON)**（J**ava**S**cript **O**bject **N**otation）：JavaScript对象表示法

是一种由轻量级的[数据交换语言](https://zh.wikipedia.org/wiki/数据交换)，该语言以易于让人阅读的文字为基础，用来传输由属性值或者序列性的值组成的数据对象。尽管JSON是[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)的一个子集，但JSON是独立于语言的[文本格式](https://zh.wikipedia.org/wiki/文本文件)。

JSON 数据格式与语言无关，脱胎自[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)，但当前很多[编程语言](https://zh.wikipedia.org/wiki/编程语言)都支持 JSON 格式数据的生成和[解析](https://zh.wikipedia.org/wiki/语法分析器)。JSON 的官方 [MIME 类型](https://zh.wikipedia.org/wiki/互联网媒体类型)是 `application/json`，文件扩展名是 `.json`。

### XML

**[XML](https://zh.wikipedia.org/wiki/XML)**（E**x**tensible **M**arkup **L**anguage）：可扩展标记语言

**可扩展标记语言**（英语：E**x**tensible **M**arkup **L**anguage，简称：**XML**）是一种[标记语言](https://zh.wikipedia.org/wiki/标记语言)。标记指[计算机](https://zh.wikipedia.org/wiki/计算机)所能理解的信息符号，通过此种标记，[计算机](https://zh.wikipedia.org/wiki/電腦)之间可以处理包含各种信息的文章等

### DI

Dependency Injection，依赖注入。在软件工程中，依赖注入是种实现控制反转用于解决依赖性设计模式。一个依赖关系指的是可被利用的一种对象（即服务提供端） 。依赖注入是将所依赖的传递给将使用的从属对象（即客户端）。该服务是将会变成客户端的状态的一部分。 传递服务给客户端，而非允许客户端来建立或寻找服务，是本设计模式的基本要求。

### IoC

控制反转（Inversion of Control，缩写为IoC），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做依赖注入（Dependency Injection，简称DI），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。

### DNS

**[DNS](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F)**（D**omain **N**ame **S**ystem）：域名系统

是[互联网](https://zh.wikipedia.org/wiki/互联网)的一项服务。它作为将[域名](https://zh.wikipedia.org/wiki/域名)和[IP地址](https://zh.wikipedia.org/wiki/IP地址)相互[映射](https://zh.wikipedia.org/wiki/映射)的一个[分布式数据库](https://zh.wikipedia.org/wiki/分布式数据库)，能够使人更方便地访问[互联网](https://zh.wikipedia.org/wiki/互联网)。

### GUI

**[GUI](https://zh.wikipedia.org/wiki/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2)**（G**raphical **U**ser **I**nterface）：图形用户界面

是指采用图形方式显示的[计算机](https://zh.wikipedia.org/wiki/计算机)操作[用户界面](https://zh.wikipedia.org/wiki/用户界面)。与早期计算机使用的[命令行界面](https://zh.wikipedia.org/wiki/命令行界面)相比，图形界面对于用户来说在[视觉](https://zh.wikipedia.org/wiki/视觉)上更易于接受。

### JWT

**[JWT](https://zh.wikipedia.org/wiki/JWT)**（JSON Web Token）：

一种用以产生访问令牌的开源标准，适用于分布式站点的单点登录（SSO）场景。



## Java

### JPA

**[JPA](https://zh.wikipedia.org/wiki/Java持久化API)**（Java Persistence API）：Java 持久化 API

是一个 [Java](https://zh.wikipedia.org/wiki/Java) [应用程序接口](https://zh.wikipedia.org/wiki/应用程序接口) 规范，描述了使用 [Java标准版平台](https://zh.wikipedia.org/wiki/Java_SE)（Java SE） 和 [Java企业版平台](https://zh.wikipedia.org/wiki/Jakarta_EE)（Java EE）的应用中的 [关系数据](https://zh.wikipedia.org/wiki/Jakarta_EE) 的管理。

[持久化](https://en.wikipedia.org/wiki/Persistence_(computer_science))，在这里包括三个层面的意思：

- [API](https://zh.wikipedia.org/wiki/应用程序接口) 本身，定义在 javax.persistence 包`内`
- Java持久化查询语言 (JPQL)
- 对象/关系 元数据

### JPQL

**[JPQL](https://zh.wikipedia.org/wiki/JPQL)**（Java Persistence Query Language）：Java持久化查询语言

对存储在关系数据库中的实体进行查询。查询在语法上类似于SQL查询，但是操作的是实体对象而不是直接对数据库表进行操作。

### EJB

**[EJB](https://zh.wikipedia.org/wiki/EJB)**（Enterprise JavaBean）：企业级JavaBean

是一个用来构筑企业级应用的服务器端可被管理组件。 Java企业版API（Java Enterprise Edition）中提供了对**EJB**的规范。 **EJB**是一个封装有某个应用程序之业务逻辑服务器端组件。

### POJO

**POJO**（Plain Ordinary Java Object）：简单的Java对象

实际就是普通JavaBeans，是为了避免和EJB混淆所创造的简称。 使用**POJO**名称是为了避免和EJB混淆起来, 而且简称比较直接.其中有一些属性及其 getter setter 方法的类，没有业务逻辑，有时可以作为VO(Value Object) 或 DTO(Data Transform Object) 来使用。当然，如果你有一个简单的运算属性也是可以的。




## PHP

### PHP

  **PHP**（全称：**P**HP：**H**ypertext **P**reprocessor，即“PHP：超文本预处理器”）是一种[开源](https://zh.wikipedia.org/wiki/开源)的通用[计算机](https://zh.wikipedia.org/wiki/计算机)[脚本语言](https://zh.wikipedia.org/wiki/脚本语言)

## DataBase

### SQL

**[SQL](https://zh.wikipedia.org/wiki/SQL)**（Structured Query Language）：结构化查询语言

是一种[特定目的编程语言](https://zh.wikipedia.org/wiki/特定目的程式语言)，用于管理[关系数据库管理系统](https://zh.wikipedia.org/wiki/关系数据库管理系统)（RDBMS），或在[关系流数据管理系统](https://zh.wikipedia.org/wiki/关系流数据管理系统)（RDSMS）中进行流处理。

### NoSQL

**[NoSQL](https://zh.wikipedia.org/wiki/NoSQL)**（）：

是对不同于传统的[关系数据库](https://zh.wikipedia.org/wiki/關聯式資料庫)的[数据库管理系统](https://zh.wikipedia.org/wiki/数据库管理系统)的统称。两者存在许多显著的不同点，其中最重要的是NoSQL不使用SQL作为查询语言。其数据存储可以不需要固定的表格模式，也经常会避免使用SQL的[JOIN](https://zh.wikipedia.org/wiki/连接_(SQL))操作，一般有[水平可扩展性](https://zh.wikipedia.org/w/index.php?title=水平可扩展性&action=edit&redlink=1)的特征。



## Front end

### SPA
单页 Web 应用（single page web application），就是只有一张 Web 页面的应用，是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的 Web 应用程序。



  


