---
title: mysql-transaction
permalink: mysql-transaction
categories:
- program
tags: 
- mysql
comments: true
date: 2018-12-09
updated: 2018-12-09
---


数据库事务（Database Transaction）,是指作为单个逻辑工作单元执行的一系列操作，要么完全执行，要么完全地不执行

### 案例

- A 转账给B 6元,  需要保证，A减少 6 元，B增加 6 元，缺一不可

### Development Environment

- mysql 

  ```bash
  version: 5.7
  ```

### 相关概念

- ACID特性

  **Atomicity** ： **原子性**

  一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位

  **Consistency** ： **一致性**

  事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到

  **Isolation** ： **隔离性**

  数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）

  **Durability** ： **持久性**

  事务完成后，事务对数据库的所有更新将被保存到数据库，不可回滚

- 事务隔离级别

  **read-uncommitted**  ： **读未提交**

  **read-committed**  ： **读已提交** 

  **repeatable-read**  ：  **可重复读**   (mysql5.7默认级别)

  **serializable**  ：  **串行化**

### ACID

### Create test data

```bash
# 创建数据库
create database transaction default character set utf8mb4 collate utf8mb4_unicode_ci;
# 创建数据表
CREATE TABLE `transaction`.`account`  (
  `id` bigint(11) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(55) NULL COMMENT '姓名',
  `balance` decimal(10, 2) NULL COMMENT '余额',
  PRIMARY KEY (`id`)
) COMMENT = '账号表';
# 插入测试数据
INSERT INTO `transaction`.`account`(`name`, `balance`) VALUES ('joe', 450);
INSERT INTO `transaction`.`account`(`name`, `balance`) VALUES ('she', 600);
```

### 隔离级别 举例说明

- read-uncommitted

  1. 打开客户端 A ,并设置当前事务级别为read-uncommitted（未提交读），并查询表account的初始值

     ```bash
     # 进入 mysql 命令行
     mysql -uroot -proot
     
     # 设置当前窗口事务级别为 read uncommitted
     set session transaction isolation level read uncommitted;
     # Query OK, 0 rows affected (0.00 sec)
     use transaction;
     start transaction;
     select * from account;
     
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  450.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

  2. 打开客户端 B ,同样设置当前事务级别为 read uncommitted ，并更新数据

     ```bash
     mysql -uroot -proot
     set session transaction isolation level read uncommitted;
     use transaction;
     start transaction;
     update account set balance = balance-50 where id = 1;
     ```

  3. 返回客户端 A，B 的事务未提交, 窗口 A 仍然可以查看更新后的数据

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

  4. 返回客户端 B,假设因为异常数据回滚，那么此时客户端 A 返回给用户的数据就是 `脏数据`

     ```bash
     # 客户端 B
     select * from account;
     
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     
     # Q1:回滚之前，id=1 id=2两条数据是否可写？
     
     # 回滚
     rollback;
     
     select * from account;
     
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  450.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

  5. 打开客户端 A ,执行减少 50 的更新操作，结果并不是400-50=350, 而是 400，`用户把钱全转出去，操作结束发现还剩50`

     在应用程序中，我们会用400-50=350，并不知道其他会话回滚了，要想解决这个问题可以采用 `read-committed` 的隔离级别

     ```bash
     # 客户端 A
     mysql> select * from account;  # 此步骤是在客户端 B 未rollback 时执行
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     
     # 以下命令是在rollback 已执行的情况下运行
     mysql> update account set balance = balance-50 where id = 1;
     Query OK, 1 row affected (0.00 sec)
     Rows matched: 1  Changed: 1  Warnings: 0
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

- read committed

  1. 打开一个客户端 A，并设置当前事务模式为read committed，查询表account的所有记录：

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  450.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.01 sec)
     ```

  2. 打开客户端 B, 开启一个事务，并更新数据

     ```bash
     # 客户端 B
     mysql> start transaction;
     Query OK, 0 rows affected (0.00 sec)
     
     mysql> update account set balance = balance-50 where id = 1;
     Query OK, 1 row affected (0.00 sec)
     Rows matched: 1  Changed: 1  Warnings: 0
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

  3. 打开客户端 A, 查看所有记录，未读取到 B 已经更新的数据 ,  `解决了脏读`

     ```bash
     mysql> select * from account;
     
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  450.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     ```

  4. 打开客户端 B, 提交事务

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     
     commit;
     ```

  5. 打开客户端 A, 执行查看所有的查询，发现与上一步结果不一致,产生 `不可重复读` 问题

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  450.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     ```

- repeatable read

  1. 打开客户端 A, 查询表account的所有记录

     ```bash
     set session transaction isolation level repeatable read;
     start transaction;
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     ```

  2. 在客户端A的事务提交之前，打开另一个客户端B，更新表account

     ```bash
     mysql> update account set balance = balance-50 where id = 1;
     Query OK, 1 row affected (0.01 sec)
     Rows matched: 1  Changed: 1  Warnings: 0
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  350.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     ```

  3. 在客户端A查询表account的所有记录，与上一步查询结果一致，没有出现不可重复读的问题

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     ```

  4. 在客户端A，接着执行balance = balance - 50 where id = 1，balance没有变成400-50=350，balance值用的是步骤（2）中的350来算的，所以是300，数据的一致性倒是没有被破坏。可重复读的隔离级别下使用了MVCC机制，select操作不会更新版本号，是快照读（历史版本）；insert、update和delete会更新版本号，是当前读（当前版本）

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     2 rows in set (0.00 sec)
     
     mysql> update balance = balance - 50 where id = 1;
     mysql> update account set balance = balance-50 where id = 1;
     Query OK, 1 row affected (0.00 sec)
     Rows matched: 1  Changed: 1  Warnings: 0
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  300.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     
     commit;
     ```

  5. 重新打开客户端B，插入一条新数据

     ```bash
     mysql> insert into account values(4,'kid',700);
     Query OK, 1 row affected (0.01 sec)
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  350.00 |
     |  2 | she  |  600.00 |
     |  4 | kid  |  700.00 |
     +----+------+---------+
     ```

  6. 在客户端A查询表account的所有记录，没有 查出 新增数据，所以没有出现幻读

     ```bash
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  350.00 |
     |  2 | she  |  600.00 |
     +----+------+---------+
     ```

- serializable

  1. 打开一个客户端A，并设置当前事务模式为serializable，查询表account的初始值：

     ```bash
     mysql> set session transaction isolation level serializable;
     Query OK, 0 rows affected (0.00 sec)
     
     mysql> start transaction;
     Query OK, 0 rows affected (0.00 sec)
     
     mysql> select * from account;
     +----+------+---------+
     | id | name | balance |
     +----+------+---------+
     |  1 | joe  |  400.00 |
     |  2 | she  |  600.00 |
     |  4 | kid  |  700.00 |
     +----+------+---------+
     ```

  2. 打开一个客户端 B，并设置当前事务模式为serializable，插入一条记录报错，表被锁了插入失败，mysql中事务隔离级别为serializable时会锁表，因此不会出现幻读的情况，这种隔离级别并发性极低，开发中很少会用到

     ```bash
     set session transaction isolation level serializable;
     
     mysql> insert into account values(5,'tom',0);
     ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
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