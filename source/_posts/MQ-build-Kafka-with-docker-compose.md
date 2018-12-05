---
title: Build Kafka with docker-compose
categories: program
tags: MQ-kafka
comments: true
date: 2018-10-05
---


## Build Kafka with docker-compose

### precondition

- kafka

- kafka-manager

-  zookeeper

  `本次测试 docker 已安装kafka zookeeper kafaka-manager,不讲述具体安装流程，如何安装查看结尾docker-compose.yml既可`

### 可用性测试

   --zookeeper ZookeeperName ： ZookeeperName指的是本地zookeeper的名字

1. 进入指定kafka容器

   ```bash
   docker-compose exec kafka bash
   ```

2. 创建topic

   ```bash
   # cd KAFKA_HOME/bin/
   cd /opt/kafka_2.11-2.0.1/bin/
   
   kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic send-register-sms
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
   kafka-console-producer.sh --broker-list localhost:9092 --topic=send-register-sms
   
   {"phone":"17626041111"}
   ```

5. 新窗口接收信息

   ```bash
   # cd KAFKA_HOME/bin/
   cd /opt/kafka_2.11-2.0.1/bin/
   
   kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic send-register-sms
   
   # 当出现发送的消息  success~
   {"phone":"17626041111"}
   ```



### reference

- [使用Docker快速搭建Kafka开发环境](https://tomoyadeng.github.io/blog/2018/06/02/kafka-cluster-in-docker/index.html)

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
   ```



### Remarks

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


