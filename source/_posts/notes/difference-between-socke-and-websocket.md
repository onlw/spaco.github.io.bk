---
title: difference between socket and websocket 
permalink: difference-between-socke-and-websocket 
categories:
- program
tags: 
- socket & websocket
comments: true
date: 2019-07-23
updated: 2019-07-23
---

Difference between socket and websocket 



## Socket

> 套接字（socket）是一个抽象层，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作。套接字允许应用程序将I/O插入到网络中，并与网络中的其他应用程序进行通信。网络套接字是IP地址与端口的组合。
>
> ——百度百科

Socket是一套API接口，是建立在TCP和应用程序之间的一个抽象层，就是我们之前说的“面向抽象编程”的这个抽象，Socket这个抽象层帮我们封装了TCP/UDP等网络协议的操作，让我们可以通过Socket提供的接口进行网络通信，可以通过一个图来说明这个抽象的功能



## Websocket

[wiki.websocket](https://en.wikipedia.org/wiki/WebSocket)

> **WebSocket**是一种[通信协议](https://zh.wikipedia.org/wiki/网络传输协议)，可在单个[TCP](https://zh.wikipedia.org/wiki/传输控制协议)连接上进行[全双工](https://zh.wikipedia.org/wiki/全雙工)通信。WebSocket协议在2011年由[IETF](https://zh.wikipedia.org/wiki/互联网工程任务组)标准化为[RFC 6455](https://tools.ietf.org/html/rfc6455)，后由[RFC 7936](https://tools.ietf.org/html/rfc7936)补充规范。[Web IDL](https://zh.wikipedia.org/w/index.php?title=Web_IDL&action=edit&redlink=1)中的WebSocket API由[W3C](https://zh.wikipedia.org/wiki/万维网联盟)标准化。
>
> WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就可以创建持久性的连接，并进行双向数据传输。

### 简介

> WebSocket是一种与[HTTP](https://zh.wikipedia.org/wiki/超文本传输协议)不同的协议。两者都位于[OSI模型](https://zh.wikipedia.org/wiki/OSI模型)的[应用层](https://zh.wikipedia.org/wiki/应用层)，并且都依赖于[传输层](https://zh.wikipedia.org/wiki/传输层)的TCP协议。 虽然它们不同，但RFC 6455规定：“WebSocket设计为通过80和443端口工作，以及支持HTTP代理和中介”，从而使其与HTTP协议兼容。 为了实现兼容性，WebSocket握手使用HTTP Upgrade头[[1\]](https://zh.wikipedia.org/wiki/WebSocket#cite_note-1)从HTTP协议更改为WebSocket协议。
>
> WebSocket协议支持Web[浏览器](https://zh.wikipedia.org/wiki/浏览器)（或其他客户端应用程序）与Web[服务器](https://zh.wikipedia.org/wiki/服务器)之间的交互，具有较低的开销，便于实现客户端与服务器的实时数据传输。 服务器可以通过标准化的方式来实现，而无需客户端首先请求内容，并允许消息在保持连接打开的同时来回传递。通过这种方式，可以在客户端和服务器之间进行双向持续对话。 通信通过TCP端口80或443完成，这在防火墙阻止非Web网络连接的环境下是有益的。另外，[Comet](https://zh.wikipedia.org/wiki/Comet_(web技术))之类的技术以非标准化的方式实现了类似的双向通信。
>
> 大多数浏览器都支持该协议，包括[Google Chrome](https://zh.wikipedia.org/wiki/Google_Chrome)、[Firefox](https://zh.wikipedia.org/wiki/Firefox)、[Safari](https://zh.wikipedia.org/wiki/Safari)、[Microsoft Edge](https://zh.wikipedia.org/wiki/Microsoft_Edge)、[Internet Explorer](https://zh.wikipedia.org/wiki/Internet_Explorer)和[Opera](https://zh.wikipedia.org/wiki/Opera瀏覽器)。
>
> 与HTTP不同，WebSocket提供全双工通信。此外，WebSocket还可以在TCP之上启用消息流。TCP单独处理字节流，没有固有的消息概念。 在WebSocket之前，使用Comet可以实现全双工通信。但是Comet存在TCP握手和HTTP头的开销，因此对于小消息来说效率很低。WebSocket协议旨在解决这些问题。
>
> WebSocket协议规范将`ws`（WebSocket）和`wss`（WebSocket Secure）定义为两个新的[统一资源标识符](https://zh.wikipedia.org/wiki/統一資源標識符)（URI）方案，分别对应明文和加密连接。除了方案名称和片段ID（不支持`#`）之外，其余的URI组件都被定义为此URI的通用语法。
>
> 使用浏览器开发人员工具，开发人员可以检查WebSocket握手以及WebSocket框架。

### 背景

> 现在，很多网站为了实现[推送技术](https://zh.wikipedia.org/wiki/推送技术)，所用的技术都是[轮询](https://zh.wikipedia.org/wiki/輪詢)。轮询是在特定的的时间间隔（如每秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的[头部](https://zh.wikipedia.org/wiki/HTTP头字段列表)，其中真正有效的数据可能只是很小的一部分，显然这样会消耗很多的带宽资源。
>
> 比较新的轮询技术是[Comet](https://zh.wikipedia.org/wiki/Comet_(web技术))。这种技术虽然可以实现双向通信，但仍然需要反复发出请求。而且在Comet中普遍采用的[HTTP长连接](https://zh.wikipedia.org/wiki/HTTP持久链接)也会消耗服务器资源。
>
> 在这种情况下，[HTML5](https://zh.wikipedia.org/wiki/HTML5)定义了WebSocket协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。
>
> Websocket使用`ws`或`wss`的[统一资源标志符](https://zh.wikipedia.org/wiki/统一资源标志符)，类似于[HTTPS](https://zh.wikipedia.org/wiki/HTTPS)。其中`wss`表示使用了[TLS](https://zh.wikipedia.org/wiki/TLS)的Websocket。如：
>
> ```
> ws://example.com/wsapi
> wss://secure.example.com/wsapi
> ```
>
> Websocket与HTTP和HTTPS使用相同的TCP[端口](https://zh.wikipedia.org/wiki/TCP/UDP端口列表)，可以绕过大多数[防火墙](https://zh.wikipedia.org/wiki/防火墙)的限制。默认情况下，Websocket协议使用80端口；运行在TLS之上时，默认使用443端口。

### 优点

> - 较少的控制开销。在连接创建后，服务器和客户端之间交换数据时，用于协议控制的数据包头部相对较小。在不包含扩展的情况下，对于服务器到客户端的内容，此头部大小只有2至10[字节](https://zh.wikipedia.org/wiki/字节)（和数据包长度有关）；对于客户端到服务器的内容，此头部还需要加上额外的4字节的[掩码](https://zh.wikipedia.org/wiki/掩码)。相对于HTTP请求每次都要携带完整的头部，此项开销显著减少了。
>
> - 更强的实时性。由于协议是全双工的，所以服务器可以随时主动给客户端下发数据。相对于HTTP请求需要等待客户端发起请求服务端才能响应，延迟明显更少；即使是和Comet等类似的[长轮询](https://zh.wikipedia.org/w/index.php?title=长轮询&action=edit&redlink=1)比较，其也能在短时间内更多次地传递数据。
>
> - 保持连接状态。与HTTP不同的是，Websocket需要先创建连接，这就使得其成为一种有状态的协议，之后通信时可以省略部分状态信息。而HTTP请求可能需要在每个请求都携带状态信息（如身份认证等）。
>
> - 更好的二进制支持。Websocket定义了[二进制](https://zh.wikipedia.org/wiki/二进制)帧，相对HTTP，可以更轻松地处理二进制内容。
>
> - 可以支持扩展。Websocket定义了扩展，用户可以扩展协议、实现部分自定义的子协议。如部分浏览器支持[压缩](https://zh.wikipedia.org/wiki/数据压缩)等。
>
> - 更好的压缩效果。相对于[HTTP压缩](https://zh.wikipedia.org/wiki/HTTP压缩)，Websocket在适当的扩展支持下，可以沿用之前内容的[上下文](https://zh.wikipedia.org/wiki/上下文)，在传递类似的数据时，可以显著地提高压缩率。

### 握手协议

> WebSocket 是独立的、创建在 TCP 上的协议。
>
> Websocket 通过 [HTTP](https://zh.wikipedia.org/wiki/HTTP)/1.1 协议的101[状态码](https://zh.wikipedia.org/wiki/HTTP状态码)进行握手。
>
> 为了创建Websocket连接，需要通过浏览器发出请求，之后服务器进行回应，这个过程通常称为“[握手](https://zh.wikipedia.org/wiki/握手_(技术))”（handshaking）。

## Summary

Websocket是一种应用层协议，Socket是封装了网络层操作的抽象API接口



## References

[维基WebSocket](https://zh.wikipedia.org/wiki/WebSocket)

