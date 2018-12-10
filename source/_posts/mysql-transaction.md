---
title: mysql transaction
categories: program
tags: mysql
comments: true
date: 2018-12-09
---

数据库事务（Database Transaction）,是指作为单个逻辑工作单元执行的一系列操作，要么完全执行，要么完全地不执行

### 案例

- A 转账给B 6元,  需要保证，A减少 6 元，B增加 6 元，缺一不可

### 相关概念

- ACID特性

  **Atomicity** ： **原子性**

  一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位

  **Consistency** ： **一致性**

  事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到

  **Isolation** ： **隔离性**

  数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）

  同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账

  **Durability** ： **持久性**

  事务完成后，事务对数据库的所有更新将被保存到数据库，不可回滚

- 事务隔离级别

  **read-uncommitted**  ： **读未提交**

  **read-committed**  ： **读已提**

  **repeatable-read**  ：  **可重复读**

  **serializable**  ：  **串行化**



### ACID



### 隔离级别

建立测试数据

```bash
create database transaction default character set utf8mb4 collate utf8mb4_unicode_ci;

CREATE TABLE `transaction`.`account`  (
  `id` bigint(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(55) NULL COMMENT '姓名',
  `balance` decimal(10, 2) NULL COMMENT '余额',
  PRIMARY KEY (`id`)
) COMMENT = '账号表';

INSERT INTO `transaction`.`account`(`name`, `balance`) VALUES ('joe', 100)
INSERT INTO `transaction`.`account`(`name`, `balance`) VALUES ('she', 200)
```



1. read-uncommitted

   - 打开客户端 A ,并设置当前事务模式为read uncommitted（未提交读），查询表account的初始值：

     ```bash
     # 进入 mysql 命令行
     set session transaction isolation level read uncommitted;
     # Query OK, 0 rows affected (0.00 sec)
     
     # 开启事务
     start transaction;
     # Query OK, 0 rows affected (0.00 sec)
     
     
     ```






### 查看隔离级别

mysql默认的事务处理级别是'REPEATABLE-READ',也就是可重复读

1. 查看当前会话隔离级别

select @@tx_isolation;

2. 查看系统当前隔离级别

select @@global.tx_isolation;

3. 设置当前会话隔离级别

set session transaction isolatin level repeatable read;

4. 设置系统当前隔离级别

set global transaction isolation level repeatable read;