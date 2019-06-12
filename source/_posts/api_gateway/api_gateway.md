---
title: api-gateway-introduction
permalink: api-gateway-introduction
categories:
- program
tags:
- api gateway
comments: true
date: 2018-10-23
updated: 2018-10-23
---


## API Gateway

### Foreword

在非技术术语中，“网关或门是进入一个由墙围住的封闭空间的入口点。”同理，API网关是指位于防火墙或互联网后面
的服务的入口点。在微服务的世界中，网关坐镇于API前面，直接面向客户并进行反向代理。

### Pros and cons of using API gateways

1. 好处：降低构建微服务的复杂性；微服务模拟与虚拟化
2. 弊端：在架构上需要额外考虑更多编排与管理；路由逻辑配置要进行统一的管理
   - 网关会为端到端响应时间带来额外的延迟。
   - 潜在的性能瓶颈
   - 如果没有明智地选择网关，将会增加额外的运营开销和成本




### Common Gateways

Zuul

Kong