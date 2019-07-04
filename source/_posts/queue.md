---
title: queue
permalink: queue
categories:
- program
tags: 
- queue
comments: true
date: 2018-08-20
updated: 2018-08-20
---
Briefly introduce the principle of queues, the implementation of common programming languages

## Queue

### FIFO结构

像栈一样，队列（queue）也是一种线性表，它的特性是先进先出，插入在一端，删除在另一端。就像排队一样，刚来的人入队（push）要排在队尾(rear)，每次出队(pop)的都是队首(front)

队列（Queue）与栈一样，是一种线性存储结构，它具有如下特点：

1. 队列中的数据元素遵循 	`先进先出` （First In First Out）的原则，简称FIFO结构
2. 在队尾添加元素，在队头删除元素

### 相关概念

- 入队：队列的插入操作
- 出队：队列的删除操作

### 相关用途

队列可以很好地异步处理数据传送和存储，当你频繁地向数据库中插入数据、频繁地向搜索引擎提交数据，就可采取队列来异步插入。另外，还可以将较慢的处理逻辑、有并发数量限制的处理逻辑，通过消息队列放在后台处理，例如FLV视频转换、发送手机短信、发送电子邮件等。

### 图解

1. `入队` 我们有一个存储整型元素的队列，我们依次入对：{1，2，3}

![image.png](https://upload-images.jianshu.io/upload_images/3625649-63e03420d223945f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

2. 出对

   ![image.png](https://upload-images.jianshu.io/upload_images/3625649-6e6ed1b3d3c6a339.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)