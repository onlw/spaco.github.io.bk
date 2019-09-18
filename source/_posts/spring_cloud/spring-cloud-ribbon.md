---
title: Spring Cloud Ribbon
permalink: spring-cloud-ribbon
categories:
- program
tags: 
- spring-cloud
comments: true
date: 2019-09-18
updated: 2019-09-18
---

## Introduction

*Spring Cloud Ribbon* 是基于 [Netflix Ribbon](https://github.com/Netflix/ribbon) 实现的客户端负载均衡工具。
`Ribbon`是一个客户端负载均衡器，可以让您对HTTP和TCP客户端的行为进行大量控制。`Feign`已使用`Ribbon`，因此，如果您使用`@FeignClient`，此部分也适用。
Ribbon中的一个核心概念是指定客户端的概念。每个负载均衡器都是一组组件的一部分，这些组件一起工作以按需联系远程服务器，并且该集合具有您作为应用程序开发人员提供的名称（例如，通过使用`@FeignClient`注释）。

> Load Balance负载均衡是用于解决一台机器(一个进程)无法解决所有请求而产生的一种算法。像nginx可以使用负载均衡分配流量，ribbon为客户端提供负载均衡，dubbo服务调用里的负载均衡等等，很多地方都使用到了负载均衡。

> ribbon 客户端作为连接多个相同服务的中间层，通过 `ribbon`客户端调取需要使用负载均衡的服务，`ribbon`客户端通过具体的负载均衡策略实现负载均衡功能

## Advantage

- Load-balancing

- 


## Key Words

- IRule

  这是所有负载均衡策略的父接口，里边的核心方法就是choose方法，用来选择一个服务实例。

- AbstractLoadBalancerRule

  AbstractLoadBalancerRule是一个抽象类，里边主要定义了一个ILoadBalancer，就是我们上文所说的负载均衡器，负载均衡器的功能我们在上文已经说的很详细了，这里就不再赘述，这里定义它的目的主要是辅助负责均衡策略选取合适的服务端实例。

## Development Environment

- Spring Boot : 2.1.8
- gradle : 5.6
- Java : jdk1.8
- ribbon : 2.3.0
- intelliji

## Ribbon Load Balancer Strategies

![avatar](https://spaco.oss-cn-hangzhou.aliyuncs.com/banners/spring-cloud/ribbon_load_balancer_strategies.png)



## References

- 项目demo地址
- [ribbon guide](https://spring.io/guides/gs/client-side-load-balancing/)
- [官网文档](https://cloud.spring.io/spring-cloud-netflix/reference/html/#spring-cloud-ribbon) 
- 负载均衡策略
- [Spring Cloud源码分析（二）Ribbon](http://blog.didispace.com/springcloud-sourcecode-ribbon/)
- [Spring Cloud中的负载均衡策略](https://segmentfault.com/a/1190000011159573#articleHeader6)
- 