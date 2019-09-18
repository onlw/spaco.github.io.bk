---
title: Spring Cloud Stream
permalink: spring-cloud-stream
categories:
- program
tags: 
- spring-cloud
comments: true
date: 2019-09-17
updated: 2019-09-17
---


Spring Cloud Stream

## Introduction

`Spring Cloud Stream`是一种用于构建具有共享消息系统连接高度可扩展的事件驱动微服务的框架。 该框架提供了一个灵活的编程模型建立在已建立的和熟悉的Spring最佳实践，包括持续的pub / sub （消息的发布（Publish）和订阅（Subscribe）是事件驱动的经典模式），消费群体和状态分区的支持。

## Advantage

- 解耦
- 扩展性
- 可恢复性
- 异步通信
- 无缝切换消息队列选型

## Development Environment

- Spring Boot : 2.1.7
- gradle : 5.6
- Java : jdk1.8
- Kafka : kafka_2.12
- intelliji

## Key Words

- @Input

  类似于`Consumer`，消息消费者

- @Output

  类似于 `Producer`，消息生产者

- Binder

  Binder 是 Spring Cloud Stream 的一个抽象概念，是应用与消息中间件之间的粘合剂

- Destination

  通过 binder ，可以很方便的连接中间件，可以动态的改变消息的destinations（对应于 Kafka 的topic，Rabbit MQ 的 exchanges），这些都可以通过外部配置项来做到。

- Consumer Group

  `Group`，如果使用过 `Kafka` 的并不会陌生。Spring Cloud Stream 的这个分组概念的意思基本和 Kafka 一致。微服务中动态的缩放同一个应用的数量以此来达到更高的处理能力是非常必须的。对于这种情况，同一个事件防止被重复消费，只要把这些应用放置于同一个 “group” 中，就能够保证消息只会被其中一个应用消费一次。

- [Partition](https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/2.2.1.RELEASE/spring-cloud-stream.html#partitioning)

## Build Producer Client

### 配置Gradle

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.apache.kafka:kafka-streams'
    implementation 'org.springframework.cloud:spring-cloud-function-web'
    implementation 'org.springframework.cloud:spring-cloud-starter'
    implementation 'org.springframework.cloud:spring-cloud-starter-task'
    implementation 'org.springframework.cloud:spring-cloud-stream'
    implementation 'org.springframework.cloud:spring-cloud-stream-binder-kafka-streams'
    implementation 'org.springframework.cloud:spring-cloud-stream-binder-kafka'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework.cloud:spring-cloud-stream-test-support'
}
```

### 配置 application.properties

```properties
server.port=8085
spring.application.name=stream-output
spring.cloud.stream.kafka.binder.brokers=localhost:9092
spring.cloud.stream.kafka.binder.auto-create-topics=true
spring.cloud.stream.bindings.refuel.destination=destination-demo
spring.cloud.stream.bindings.refuel.content-type=application/json
```

### Code : Producer

`控制器`提供外部访问接口，调取`Producer`,`Producer` 通过 `消息通道` 生产消息，

### 自定义消息通道 Channel

```java
import org.springframework.cloud.stream.annotation.Output;
import org.springframework.messaging.MessageChannel;

public interface RefuelChannel {
    String OUTPUT = "refuel";

    @Output(OUTPUT)
    MessageChannel output();
}
```

### 自定义 Producer

```java
import com.spaco.streamoutput.channel.RefuelChannel;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.messaging.support.MessageBuilder;

@EnableBinding(RefuelChannel.class)
public class RefuelProducer {
    private RefuelChannel refuelChannel;
  
  	@Autowired
    public RefuelProducer(RefuelChannel refuelChannel)
    {
        this.refuelChannel = refuelChannel;
    }

    public void send(String message)
    {
        refuelChannel.output().send(MessageBuilder.withPayload(message).build());
    }
}
```

#### Controller

```java
import com.spaco.streamoutput.producer.RefuelProducer;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class RefuelController {

    private RefuelProducer refuelProducer;

    public RefuelController(RefuelProducer refuelProducer){
        this.refuelProducer = refuelProducer;
    }

    @GetMapping("/refuel")
    public String send(@RequestBody Object message)
    {
        log.error("sent message: {}",message);
        this.refuelProducer.send(message);

        return message.toString();
    }
}
```

### 启动服务

### Test Cases

命令行执行

```bash
curl -X  GET http://127.0.0.1:8085/refuel -H 'Content-Type: application/json' -d '{"message":"hello"}'
```

可以看到接口返回值，且消息已经发送成功

```bash
{message=hello}
```

测试通过

## Build Consumer Client

### 配置Gradle

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-mongodb'
    implementation 'org.apache.kafka:kafka-streams'
    implementation 'org.springframework.cloud:spring-cloud-function-web'
    implementation 'org.springframework.cloud:spring-cloud-starter'
    implementation 'org.springframework.cloud:spring-cloud-starter-task'
    implementation 'org.springframework.cloud:spring-cloud-stream'
    implementation 'org.springframework.cloud:spring-cloud-stream-binder-kafka-streams'
    implementation 'org.springframework.cloud:spring-cloud-stream-binder-kafka'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework.cloud:spring-cloud-stream-test-support'
}
```



### 配置 application.properties

```properties
server.port=8086
spring.application.name=stream-input
spring.cloud.stream.kafka.binder.brokers=localhost:9092
spring.cloud.stream.kafka.binder.auto-create-topics=true
#spring.cloud.stream.bindings.message-channel.destination=destination-demo
spring.cloud.stream.bindings.refuel.destination=destination-demo
spring.cloud.stream.bindings.refuel.content-type=application/json
spring.cloud.stream.bindings.refuel.group=group-name
```

### Code : Consumer

#### 自定义消息通道 Channel

```java
import org.springframework.cloud.stream.annotation.Input;
import org.springframework.messaging.MessageChannel;

public interface RefuelChannel {
    String INPUT = "refuel";

    @Input(INPUT)
    MessageChannel input();
}
```

#### 自定义Consumer

```java
package com.spaco.streaminput.consumer;

import com.spaco.streaminput.channel.RefuelChannel;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;

@EnableBinding(RefuelChannel.class)
public class RefuelConsumer{
    @StreamListener(RefuelChannel.INPUT)
    public void receive(Object payload) {
        System.out.println("received message: "+ payload);
    }
}
```

### 启动服务

#### Test Cases

producer 信息发送成功后，可以在 consumer 的Console 打印台看到如下信息

```bash
received message: {"message":"hello"}
```

测试通过，效果与我们希望的结果一致。producer 发送消息成功后，consumer 消费消息。

## Questions

- 重复执行

- 执行日志，记录每个消费操作执行的结果，失败可以通过定时任务重试

- 保证消息不丢失

- 幂等性设计

## References

- [项目Demo地址](https://github.com/spaco/spring-cloud-tutorial)
- [官方文档](https://spring.io/projects/spring-cloud-stream#overview)
- [SpringCloud实战9-Stream消息驱动](https://www.cnblogs.com/huangjuncong/p/9102843.html)
- [微服务中消息总线架构设计应用](https://segmentfault.com/a/1190000017212260)
- [构建消息驱动微服务的框架 Spring Cloud Stream](https://segmentfault.com/a/1190000018910808)
- [Spring Cloud Stream 体系及原理介绍](https://fangjian0423.github.io/2019/04/03/spring-cloud-stream-intro/)
