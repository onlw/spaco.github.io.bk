---
title: difference between mongo mongodb mongod
permalink: difference-between-mongo-mongodb-mongod
categories:
- program
tags: 
- mongo
comments: true
date: 2019-07-29
updated: 2019-07-29
---

Difference between mongo mongodb mongod

### Mongod 

Mongo Daemon 即守护进程

```bash
# ps -A
root@2524adecfdfb:/# ps -A
PID TTY          TIME CMD
  1 ?        00:03:17 mongod
 57 pts/0    00:00:00 bash
 83 pts/0    00:00:00 ps
```



### Mongo

command-line shell

```bash
# mongo localhost:27017
root@2524adecfdfb:/# mongo localhost:27017
MongoDB shell version v4.0.10
connecting to: mongodb://localhost:27017/test?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("d1bfacc3-15b2-41a6-b6fa-cf422e0506fc") }
MongoDB server version: 4.0.10
Server has startup warnings:
2019-07-25T07:14:11.075+0000 I CONTROL  [initandlisten]
2019-07-25T07:14:11.075+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2019-07-25T07:14:11.075+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2019-07-25T07:14:11.075+0000 I CONTROL  [initandlisten]
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
>
```

 连接之后，可以进行下一步操作，例如：查询数据库列表

```bash
# show dbs
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
report  0.021GB
```



### Mongodb

MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案

