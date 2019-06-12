---
title: kafka-case-with-python
permalink: kafka-case-with-python
categories:
- program
tags: 
- MQ
- kafka
comments: true
date: 2018-10-05
updated: 2018-10-05
---

## Kafka in python

## Build kafka

## environment

- kafka 

  ```bash
  find / -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*'
  kafka_2.11-2.0.1
  ```

- python

  ```bash
  python -V    #use pyenv
  3.6.6
  ```

- zookeeper

  ```bash
  version:
  echo stat|nc 127.0.0.1 2181
  
  Zookeeper version: 3.4.9-1757313, built on 08/23/2016 06:50 GMT
  Clients:
   /172.23.0.1:41242[0](queued=0,recved=1,sent=0)
   /172.23.0.2:49830[1](queued=0,recved=1152,sent=1153)
  
  Latency min/avg/max: -33/0/91
  Received: 5129
  Sent: 5139
  Connections: 2
  Outstanding: 0
  Zxid: 0xa3
  Mode: standalone
  Node count: 133
  ```

### Test

1. Install Python Client

   [kafka-python 1.4.4](https://pypi.org/project/kafka-python/)

   ```bash
   pip install kafka-python
   ```

2. Producer

   ```bash
   # coding=utf-8
   from kafka import KafkaProducer
   import time
   producer = KafkaProducer(bootstrap_servers=['127.0.0.1:9092'])  #此处ip可以是多个['0.0.0.1:9092','0.0.0.2:9092','0.0.0.3:9092' ]
   topic_name = 'test'
   
   i=0
   while True:
       ts = int(time.time() * 1000)
   
       # msg = '{"phone": 17626041117, "extra-key": "extra-value"}'
       msg = str(i)
       print(msg)
       producer.send(topic_name, msg.encode('utf-8'))  # 参数为主题和bytes数据
       producer.flush()
       i+=1              
       time.sleep(2)
   ```

   运行：

   ```bash
   python producer.py
   
   0
   1
   2
   3
   ```

3. consumer

   - basic

     ```bash
     # coding=utf-8
     from kafka import KafkaConsumer, TopicPartition
     
     topic_name = 'test'
     groupid = 'group1'
     server_list = '127.0.0.1:9092'
     
     consumer = KafkaConsumer(topic_name,bootstrap_servers=server_list)
     for message in consumer:
       print(message)
     ```

     运行：

     ```bash
     python consumer.py
     
     ConsumerRecord(topic=u'test', partition=0, offset=69, timestamp=1544090653375, timestamp_type=0, key=None, value='0', headers=[], checksum=None, serialized_key_size=-1, serialized_value_size=1, serialized_header_size=-1)
     
     ConsumerRecord(topic=u'test', partition=0, offset=70, timestamp=1544090655386, timestamp_type=0, key=None, value='1', headers=[], checksum=None, serialized_key_size=-1, serialized_value_size=1, serialized_header_size=-1)
     ```

   - 拉取未消费的信息：断开连接，再次bash进入消费的时候，会拉取所有的消息，而我们需要获取从断开点之后创建的信息

     ```bash
     # coding=utf-8
     from kafka import KafkaConsumer, TopicPartition
     
     topic_name = 'test'
     groupid = 'group1'
     server_list = '127.0.0.1:9092'
     # offset                        
     consumer = KafkaConsumer(group_id=groupid,
                              bootstrap_servers=server_list)
     partition = TopicPartition(topic_name, 0)
     consumer.assign([partition])
     print("start offset is ", consumer.position(partition))
     
     for message in consumer:
       print(message)
       file = open('consumer.log','a')
       file.write(message.value + "\n")
     ```

     运行：

     ```bash
     python consumer.py
     ```

### reference

- [kafka-python重复消费的问题](http://qimingyu.com/2018/06/30/Kafka%E9%87%8D%E5%A4%8D%E6%B6%88%E8%B4%B9%E7%9A%84%E9%97%AE%E9%A2%98/)

- [Python脚本消费kafka数据](https://blog.csdn.net/beyond_f4/article/details/80310340)

- [Python操作分布式流处理系统Kafka](http://www.uml.org.cn/python/201801092.asp)


### QAQ

- vscode pip安装kafka 后，通过 iterm2 操作出现 ImportError: No module named kafka

  重装。。。

- kafka.errors.NoBrokersAvailable: NoBrokersAvailable

  指定的kafka host无效

### Remarks

- group_id 不需要配置，用户确定名称即可



