---
title: kafka-build-with-docker-compose
permalink: kafka-build-with-docker-compose
categories:
- program
tags: 
- MQ
- kafka
comments: true
date: 2018-10-05
updated: 2018-10-05
---

## Build Kafka with docker-compose

[Kafka](http://kafka.apache.org/)是由Apache软件基金会开发的一个开源流处理平台，由Scala和Java编写。Kafka为处理实时数据提供一个统一、高吞吐、低延迟的平台。其持久化层本质上是一个“按照分布式事务日志架构的大规模发布/订阅消息队列”，这使它作为企业级基础设施来处理流式数据非常有价值。此外，Kafka可以通过Kafka Connect连接到外部系统（用于数据输入/输出），并提供了Kafka Streams——一个Java流式处理库 (计算机)

Kafka是一个分布式的、高吞吐量、高可扩展性的消息系统。Kafka 基于发布/订阅模式，通过消息解耦，使生产者和消费者异步交互，无需彼此等待。Ckafka 具有数据压缩、同时支持离线和实时数据处理等优点，适用于日志压缩收集、监控数据聚合等场景

### precondition

- kafka

- kafka-manager

- zookeeper

  `本次测试 docker 已安装kafka zookeeper kafaka-manager,不讲述具体安装流程，如何安装查看结尾docker-compose.yml既可`

### 关键名词

- broker：kafka集群包含一个或者多个服务器，服务器就称作broker
- producer：负责发布消息到broker
- consumer：消费者，从broker获取消息
- topic：发布到kafka集群的消息类别。
- partition：每个topic划分为多个partition。
- group：每个partition分为多个group

### 可用性测试

   后续bash: --zookeeper ZookeeperName ： ZookeeperName指的是本地zookeeper的名字

1. 进入指定kafka容器

   ```bash
   docker-compose exec kafka bash
   ```

2. 创建topic

   ```bash
   # cd KAFKA_HOME/bin/
   cd /opt/kafka_2.11-2.0.1/bin/
   
   kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic X
   # Created topic "send-register-sms".
   
   ```

3. 查看创建的topic

   ```bash
   # 查看zookeeper topics
   kafka-topics.sh --list --zookeeper zookeeper:2181
   # 查看某个topic
   kafka-topics.sh --zookeeper zookeeper:2181 --describe --topic send-register-sms
   ```

4. 发送信息

   ```bash
   kafka-console-producer.sh --broker-list localhost:9092 --topic=x
   
   {"phone":"17626041111"}
   ```

5. 新窗口接收信息

   ```bash
   # cd KAFKA_HOME/bin/
   cd /opt/kafka_2.11-2.0.1/bin/
   
   kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --from-beginning --topic x
   
   # 当出现发送的消息  success~
   {"phone":"17626041111"}
   ```

    这个时候每次断开，再次bash进入消费的时候，会拉取所有的消息，而我们需要获取从断开点之后创建的信息

   ```bash
   kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --from-beginning --topic test --consumer-property group.id=group1
   ```

   `消费组`

### reference

- [使用Docker快速搭建Kafka开发环境](https://tomoyadeng.github.io/blog/2018/06/02/kafka-cluster-in-docker/index.html)
- [kafka-python重复消费的问题](http://qimingyu.com/2018/06/30/Kafka%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9%E7%9A%84%E9%97%AE%E9%A2%98/)
- [kafka系列-进阶篇之消费组](https://blog.csdn.net/camel84/article/details/82861053)
- [Python操作分布式流处理系统Kafka](http://www.uml.org.cn/python/201801092.asp)
- 

### QAQ

1. WARN [Producer clientId=console-producer] Connection to node -1 could not be established. Broker may not be available. (org.apache.kafka.clients.NetworkClient)

   `docker-compose.yml kafka设置的 KAFKA_ADVERTISED_HOST_NAME 问题，改成 bash: ipconfig getifaddr en0  显示的ip值即可`

   ```bash
   REAL_IP = ipconfig getifaddr en0
   kafka 
   	KAFKA_ADVERTISED_HOST_NAME: REAL_IP
   ```

2. 修改配置后 rebuild kafka 显示 kafka uses an image, skipping  (已经把zookeeper kafka-manager 关闭)

   ```bash
   ## force create
   docker-compose up -d --force-recreate kafka
   docker-compose up -d --force-recreate zookeeper
   
   docker-compose up -d kafka zookeeper
   ```

3. 本地连接不用的wifi 导致ip变化

   ```bash
   docker-compose stop kafka zookeeper
   docker-compose rm kafka zookeeper
   
   docker-compose up -d --force-recreate kafka
   docker-compose up -d --force-recreate zookeeper
   docker-compose up -d kafka zookeeper
   ```

4. WARN [Consumer clientId=consumer-1, groupId=group1] 1 partitions have leader brokers without a matching listener, including [test-0] (org.apache.kafka.clients.NetworkClient)


### Remarks

- group_id 不需要配置，用户确定名称即可

- docker-compose.yml

  ```bash
  ### Zookeeper ################################################
      zookeeper:
        image: wurstmeister/zookeeper
        container_name: zookeeper
        restart: always
        ports:
          - "2181:2181"
  
  ### Kafka ################################################    
      kafka:
        image: wurstmeister/kafka
        container_name: kafka
        ports:
          - "9092:9092"
        environment:
          KAFKA_ADVERTISED_HOST_NAME: 192.168.1.112
          KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock
  
  ### Kafka-manager ################################################ 
      kafka-manager:
        image: sheepkiller/kafka-manager                
        ports:  
          - "9003:9000"     
        environment:
          ZK_HOSTS: zookeeper:2181
          APPLICATION_SECRET: "random-secret"
          KAFKA_MANAGER_AUTH_ENABLED: "true"
          KAFKA_MANAGER_USERNAME: "admin"
          KAFKA_MANAGER_PASSWORD: "secret"
  ```



drwxr-xr-x@  26 ashe  staff   832B Dec  6 20:40 cache