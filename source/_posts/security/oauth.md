---
title: oauth2.0
permalink: oauth2.0
categories:
- program
tags: 
- security
comments: true
date: 2019-08-01
updated: 2019-08-01
---
**开放授权**（[OAuth](https://oauth.net)）是一个[开放标准](https://zh.wikipedia.org/wiki/开放标准)，允许用户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和[密码](https://zh.wikipedia.org/wiki/密码)提供给第三方应用。

OAuth允许用户提供一个[令牌](https://zh.wikipedia.org/w/index.php?title=令牌&action=edit&redlink=1)，而不是用户名和密码来访问他们存放在特定服务提供者的数据。每一个令牌授权一个特定的网站（例如，视频编辑网站)在特定的时段（例如，接下来的2小时内）内访问特定的资源（例如仅仅是某一相册中的视频）。这样，OAuth让用户可以授权第三方网站访问他们存储在另外服务提供者的某些特定信息，而非所有内容。

OAuth是[OpenID](https://zh.wikipedia.org/wiki/OpenID)的一个补充，但是完全不同的服务。



## Why Oauth2.0



## 版本与历史

> The OAuth 1.0 protocol was published as [RFC 5849](https://tools.ietf.org/html/rfc5849), an informational [Request for Comments](https://en.wikipedia.org/wiki/Request_for_Comments), in April 2010.
>
> Since 31 August 2010, all third party Twitter applications have been required to use OAuth.[[6\]](https://en.wikipedia.org/wiki/OAuth#cite_note-6)
>
> The OAuth 2.0 framework was published as [RFC 6749](https://tools.ietf.org/html/rfc6749), and the Bearer Token Usage as [RFC 6750](https://tools.ietf.org/html/rfc6750), both standards track [Requests for Comments](https://en.wikipedia.org/wiki/Requests_for_Comments), in October 2012.
>
> — wiki



## 概念点

- **RFC**

  > **请求意见稿**（英语：**R**equest **f**or **C**omments，缩写：**RFC**）是由[互联网工程任务组](https://zh.wikipedia.org/wiki/互联网工程任务组)（IETF）发布的一系列[备忘录](https://zh.wikipedia.org/wiki/備忘錄)。文件收集了有关[互联网](https://zh.wikipedia.org/wiki/網際網路)相关信息，以及[UNIX](https://zh.wikipedia.org/wiki/UNIX)和互联网[社群](https://zh.wikipedia.org/wiki/社群)的软件文件，以编号排定。当前RFC文件是由[互联网协会](https://zh.wikipedia.org/wiki/互联网协会)（ISOC）[赞助](https://zh.wikipedia.org/wiki/贊助)发行

- **Resource Owner**

  资源所属者，即用户

- **Client | Third-party application** 

  客户端，指第三方应用，得到 `Resource Owner` 授权后便可以去访问 `Resource Owner`  的信息，如别名、手机号等等

- **Authorization Server**

  授权服务器， 认证  `Resource Owner` 身份，为 `Resource Owner` 提供授权审批流程，并最终颁发授权令牌(Access Token)

- **Resource Server**

  资源服务器，存储 `Resource Owner` 资源，并处理对资源的访问请求，`Client`拿到 授权令牌(Access Token)向资源服务器请求相关资源

- **User Agent**

  浏览器，chrome、safari等等

- **Access Token**

  授权令牌，一般来说，是有时效性的一段字符串，合法时间内，使用该令牌才可以访问具体资源

- **Authorization Code**

  授权码，用于获得`Access Token`的一个key

- **Authentication**

  认证，通过输入帐号和密码来认证用户

- **Authorization**

  授权，并非通过帐号和密码，而是授于第三方某权限

`Authorization Server `和 `Resource Server`有时候是同一台服务器




## Authorization Patterns

授权模式

### Authorization Code Grant  （授权码）

最常用的授权方式



### Implicit Grant （隐式）



### Resource Owner Password Credentials Grant（密码）



### Client Credentials Grant（客户端凭证）



## References

- 推荐：[授权模式RFC](https://tools.ietf.org/html/rfc6749#section-4.1)

- Security of mobile OAuth 2.0

- [sd](https://zhuanlan.zhihu.com/p/20913727)

- [简易图解】『 OAuth2.0』 猴子都能懂的图解](https://learnku.com/articles/20031)

- [授权模式](https://learnku.com/articles/20082)

  

  