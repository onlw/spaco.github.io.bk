---
title: HTTP2
permalink: HTTP2
categories:
- program
tags: 
- http
comments: true
date: 2019-08-19
updated: 2019-08-19
---
## HTTP/2

### History

> **HTTP/2**（超文本传输协议第2版，最初命名为**HTTP 2.0**），简称为**h2**（基于TLS/1.2或以上版本的加密连接）或**h2c**（非加密连接），是[HTTP](https://zh.wikipedia.org/wiki/HTTP)协议的的第二个主要版本，使用于[万维网](https://zh.wikipedia.org/wiki/全球資訊網)。
>
> HTTP/2是[HTTP](https://zh.wikipedia.org/wiki/HTTP)协议自1999年HTTP 1.1发布后的首个更新，主要基于[SPDY](https://zh.wikipedia.org/wiki/SPDY)协议。它由[互联网工程任务组](https://zh.wikipedia.org/wiki/互联网工程任务组)（IETF）的Hypertext Transfer Protocol Bis（httpbis）工作小组进行开发。该组织于2014年12月将HTTP/2标准提议递交至[IESG](https://zh.wikipedia.org/w/index.php?title=IESG&action=edit&redlink=1)进行讨论，于2015年2月17日被批准。
>
> HTTP/2标准于2015年5月以RFC 7540正式发表。HTTP/2的标准化工作由[Chrome](https://zh.wikipedia.org/wiki/Google_Chrome)、[Opera](https://zh.wikipedia.org/wiki/Opera瀏覽器)、[Firefox](https://zh.wikipedia.org/wiki/Firefox)、[Internet Explorer 11](https://zh.wikipedia.org/wiki/Internet_Explorer_11)、[Safari](https://zh.wikipedia.org/wiki/Safari)、[Amazon Silk](https://zh.wikipedia.org/w/index.php?title=Amazon_Silk&action=edit&redlink=1)及[Edge](https://zh.wikipedia.org/wiki/Microsoft_Edge)等浏览器提供支持。
>
> 多数主流浏览器已经在2015年底支持了该协议。此外，根据[W3Techs](https://zh.wikipedia.org/w/index.php?title=W3Techs&action=edit&redlink=1)的数据，截至2019年6月，全球有36.5%的网站支持了HTTP/2。
>
> —wiki

### Key Points

- **[SPDY](https://zh.wikipedia.org/wiki/HTTP/2)** ：**SPDY**（发音如英语：speedy），一种[开放](https://zh.wikipedia.org/wiki/開放原始碼)的[网络传输协议](https://zh.wikipedia.org/wiki/網路傳輸協定)，由[Google](https://zh.wikipedia.org/wiki/Google)开发，SPDY也就是[HTTP/2](https://zh.wikipedia.org/wiki/HTTP/2)的前身。设计SPDY的目的在于降低网页的加载时间。通过优先级和多路复用，SPDY使得只需要创建一个TCP连接即可传送网页内容及图片等资源。SPDY中广泛应用了TLS加密，传输内容也均以gzip或DEFLATE格式压缩（与HTTP不同，HTTP的头部并不会被压缩）。另外，除了像HTTP的网页服务器被动的等待浏览器发起请求外，SPDY的网页服务器还可以主动推送内容。

- **[多路复用]([https://zh.wikipedia.org/wiki/%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8](https://zh.wikipedia.org/wiki/多路复用))**：多路复用通常表示在一个[信道](https://zh.wikipedia.org/wiki/信道)上[传输](https://zh.wikipedia.org/wiki/传输)多路[信号](https://zh.wikipedia.org/wiki/信号)或[数据流](https://zh.wikipedia.org/w/index.php?title=数据流&action=edit&redlink=1)的过程和技术。因为多路复用能够将多个低速信道整合到一个高速信道进行传输，从而有效地利用了高速信道。通过使用多路复用

- **TCP连接**

- **流**

- **消息**

- **帧**

- **HPACK 算法**：

  HPACK算法是新引入HTTP/2的一个算法，用于对HTTP头部做压缩。其原理在于：

  - 客户端与服务端根据 [RFC 7541](https://tools.ietf.org/html/rfc7541) 的附录A，维护一份共同的静态字典（Static Table），其中包含了常见头部名及常见头部名称与值的组合的代码；
  - 客户端和服务端根据先入先出的原则，维护一份可动态添加内容的共同动态字典（Dynamic Table）；
  - 客户端和服务端根据 [RFC 7541](https://tools.ietf.org/html/rfc7541) 的附录B，支持基于该静态哈夫曼码表的哈夫曼编码（Huffman Coding）。

> 有别于HTTP/1.1在连接中的明文请求，HTTP/2与SPDY一样，将一个TCP连接分为若干个流（Stream），每个流中可以传输若干消息（Message），每个消息由若干最小的二进制帧（Frame）组成。这也是HTTP/1.1与HTTP/2最大的区别所在。 HTTP/2中，每个用户的操作行为被分配了一个**流编号**(stream ID)，这意味着用户与服务端之间创建了一个TCP通道；协议将每个请求分割为二进制的控制帧与数据帧部分，以便解析。这个举措在SPDY中的实践表明，相比HTTP/1.1，新页面加载可以加快11.81% 到 47.7%
>
> —wiki

### Why HTTP/2

先看一下案例

https://http1.golang.org/gophertiles

https://http2.akamai.com/demo



### Comparison

#### HTTP/2与HTTP/1.1比较
HTTP/2 相比 HTTP/1.1 的修改并不会破坏现有程序的工作，但是新的程序可以借由新特性得到更好的速度。
HTTP/2 保留了 HTTP/1.1 的大部分语义，例如请求方法、状态码乃至URI和绝大多数HTTP头部字段一致。而 HTTP/2 采用了新的方法来编码、传输 客户端<——>服务器 间的数据。

#### HTTP/1.1与SPDY的区别

SPDY (发音为"speedy") 是一个由 Google 主导的研究项目发明的HTTP替代协议。[13]SPDY一开始主要关注降低延迟，采用了TCP通道，但是使用了不同的协议来达到此目的。其与HTTP/1.1相比，主要的改变有：
- 实现无需先入先出的多路复用
- 为简化客户端和服务器开发的消息 ：帧机制
- 强制性压缩（包括HTTP头部）
- 优先级排序
- 双向通讯

#### HTTP/2与SPDY的比较
HTTP/2的开发基于SPDY进行跃进式改进。在诸多修改中，最显著的改进在于，HTTP/2使用了一份经过定制的压缩算法，基于霍夫曼编码，以此替代了SPDY的动态流压缩算法，以避免对协议的Oracle攻击——这一类攻击以CRIME为代表。此外，HTTP/2禁用了诸多加密包，以保证基于TLS的连接的前向安全。

### Browser support

截至2015年末，主要的浏览器的最新版本已经支持HTTP/2这一协议。其中：

Google Chrome、Mozilla Firefox、Microsoft Edge和Opera已支持HTTP/2，并默认启用。
Internet Explorer自IE 11开始支持HTTP/2，并预设激活。

### References

- [一文读懂 HTTP/2 特性](https://zhuanlan.zhihu.com/p/26559480)
- [维基百科：HTTP/2](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/HTTP/2)
- [http2讲解](https://link.zhihu.com/?target=https%3A//ye11ow.gitbooks.io/http2-explained/content/part2.html)





