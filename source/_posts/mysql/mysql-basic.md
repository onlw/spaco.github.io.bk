---
title: mysql-basic
permalink: mysql-basic
categories:
- program
tags: 
- mysql
comments: true
date: 2018-12-14
updated: 2018-12-14
---

### Backend Service

LNMP

- 操作系统 ：windows / linux（centos / ubuntu）

- HTTP 服务器 ： Apache / Nginx

  - 作用目的是一致的, 简单说就是接收用户请求, 然后处理请求, 最后将处理结果返回给用户

- 数据库

  - 关系型数据库 ：, 是指采用了关系模型来组织数据的数据库.  [几种常见关系型数据库](https://zhuanlan.zhihu.com/p/35003675)

  - - mysql ：
    - Oracle :
    - SQLite :
    - PostgreSQL

  - extra ： 非关系型数据库Nosql (not only sql) :数据也是在逐渐增长和变得复杂、非固定结构化的. 这些所有的变化都是很难在原有的关系型数据库中管理的;

  - - key-value（Redis）:
    - 文档型 （MongoDB）：
    - [常见几种nosql特点、区别以及应用场景](https://cloud.tencent.com/developer/article/1082582)
    - [关系型数据库和非关系型数据库的特性以及各自的优缺点](http://blog.51cto.com/2406236/2108520)

- Language

  php

### Mysql Basic

[官网](https://www.mysql.com/)

当前最新版本：V8.0

https://www.mysql.com/products/enterprise/database/

#### Develop Environment

```bash
mysql -V
version 5.7.25
```



#### 什么是数据库

数据库（Database）是按照数据结构来组织、存储和管理数据的仓库. 

每个数据库都有一个或多个不同的 API 用于创建, 访问, 管理, 搜索和复制所保存的数据. 

我们也可以将数据存储在文件中, 但是在文件中读写数据速度相对较慢. 

所以, 现在我们使用关系型数据库管理系统（RDBMS）来存储和管理的大数据量. 所谓的关系型数据库, 是建立在关系模型基础上的数据库, 借助于集合代数等数学概念和方法来处理数据库中的数据. 

RDBMS 即关系数据库管理系统(Relational Database Management System)的特点：

- 1.数据以表格的形式出现
- 2.每行为各种记录名称
- 3.每列为记录名称所对应的数据域
- 4.许多的行和列组成一张表单
- 5.若干的表单组成database

MySQL 是最流行的关系型数据库管理系统, 在 WEB 应用方面 MySQL 是较好的 RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一

#### 为什么选用Mysql

```bash

MySQL 是开源的, 所以你不需要支付额外的费用. 
MySQL 支持大型的数据库. 可以处理拥有上千万条记录的大型数据库. 
MySQL 使用标准的 SQL 数据语言形式. 
MySQL 可以运行于多个系统上, 并且支持多种语言. 这些编程语言包括 C、C++、Python、Java、Perl、PHP、Eiffel、Ruby 和 Tcl 等. 
MySQL 对PHP有很好的支持, PHP 是目前最流行的 Web 开发语言. 
MySQL 支持大型数据库, 支持 5000 万条记录的数据仓库, 32 位系统表文件最大可支持 4GB, 64 位系统支持最大的表文件为8TB. 
MySQL 是可以定制的, 采用了 GPL 协议, 你可以修改源码来开发自己的 MySQL 系统. 
```

#### 相关概念

- 数据库(database) : 数据库是一些关联表的集合
- 数据表
- 主键(PRIMARY KEY) : 主键是唯一的,一个数据表中只能包含一个主键. 你可以使用主键来查询数据. 
- 索引(index) :使用索引可快速访问数据库表中的特定信息. 索引是对数据库表中一列或多列的值进行排序的一种结构. 类似于书籍的目录
- 表头(header): 每一列的名称;
- 列(col): 具有相同数据类型的数据的集合;
- 行(row): 每一行用来描述某条记录的具体信息;
- 值(value): 行的具体信息, 每个值必须与该列的数据类型相同;

#### 安装

- https://www.mysql.com/downloads/

#### Console

```bash
## 进入mysql容器
docker-compose exec mysql bash
```

- connect

- ```bash
  mysql -u root -p
  ```

- create database

  ```bash
  create database zeaho default character set utf8mb4 collate utf8mb4_unicode_ci;
  
  ## check
  show databases;
  ```

- drop database

  ```bash
  drop database zeaho;
  
  ## check
  show databases;
  ```

- select database

  在你连接到 MySQL 数据库后, 可能有多个可以操作的数据库, 所以你需要选择你要操作的数据库. 

  成功选择了某个数据库后, 在后续的操作中都会在选择的数据库中执行

  ```bash
  use zeaho;
  
  Database changed
  ```

- create datatable

  ```bash
  use zeaho;
  
  CREATE TABLE `admin` (
    `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
    `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
    `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `nick_name` char(50) DEFAULT NULL,
    `unique_name` char(16) NOT NULL,
    `password` varchar(128) NOT NULL,
    `is_active` tinyint(3) unsigned NOT NULL DEFAULT '1',
    `remember_token` varchar(255) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `unique_name` (`unique_name`)
  ) ENGINE=InnoDB AUTO_INCREMENT=43 DEFAULT CHARSET=utf8mb4;
  
  ## check
  show tables;
  ```

  数据库字段具体如何选择见底部 Expansion

- drop datatable 删除数据表

  ```bash
  drop table admin;
  ```

- 插入数据

  ```bash
  INSERT INTO table_name ( field1, field2,...fieldN )
                         VALUES
                         ( value1, value2,...valueN );
                         
  ## 插入数据
  INSERT INTO `zeaho`.`admin`(`nick_name`, `unique_name`, `password`, `remember_token`) VALUES ('name', '176', 'secrey', '1dsddas');
  
  INSERT INTO `zeaho`.`admin`(`nick_name`, `unique_name`, `password`, `remember_token`) VALUES ('name1', '1761', 'secrey1', '1dsddas1');
  
  INSERT INTO `zeaho`.`admin`(`nick_name`, `unique_name`, `password`, `remember_token`) VALUES ('name1', '17612', 'secrey1', '1dsddas1');
  ```

  如果数据是字符型, 必须使用单引号或者双引号, 如："value". 

- 查询数据

  - 所有列

    ```
    select * from admin;
    ```

  - 指定列

    ```
    select id,nick_name from admin;
    ```

  - 去重列

    ```
    select distinct（password）from admin;
    ```

  - 最大列, 最小列

    ```
    select max(id) from admin;
    select min(id) from admin;
    ```

  - 计数

    ```
    select count(id) from admin;
    ```

  - 指定行

    ```
    select * from admin where id = 1 and (id is null or id is not null);
    select * from admin where nick_name like '%name%';
    ```

  - 多表查询

    ```bash
    ## 插入测试数据
    DROP TABLE IF EXISTS `user`;
    CREATE TABLE `user` (
      `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
      `creator_id` bigint(20) DEFAULT NULL,
      `name` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
      `score` int(10) unsigned DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
    
    -- ----------------------------
    -- Records of user
    -- ----------------------------
    INSERT INTO `user` VALUES (1, 43, 'a', 1);
    INSERT INTO `user` VALUES (3, NULL, 'b', 2);
    INSERT INTO `user` VALUES (4, NULL, 'c', 4);
    INSERT INTO `user` VALUES (5, NULL, 'd', 3);
    INSERT INTO `user` VALUES (6, NULL, 'e', 2);
    ```



    ```bash
    select u.*, a.id as admin_id from user u, admin a where a.id = u.creator_id;
    ```

- 更新数据

  ```bash
  ## 语法
  UPDATE table_name SET field1=new-value1, field2=new-value2
  [WHERE Clause]
  ```

  - 你可以同时更新一个或多个字段. 
  - 你可以在 WHERE 子句中指定任何条件. 
  - 你可以在一个单独表中同时更新数据. 

  ```bash
  ##单表更新
  UPDATE `zeaho`.`admin` SET `unique_name` = '17626' WHERE `id` = 43;
  
  ##多表联合修改
  update user u,admin a set u.name = 'new name' where a.id = u.creator_id;
  ```

- 删除数据

  ```bash
  ## 语法
  DELETE FROM table_name [WHERE Clause]
  ```

  ```bash
  如果没有指定 WHERE 子句, MySQL 表中的所有记录将被删除. 
  你可以在 WHERE 子句中指定任何条件
  您可以在单个表中一次性删除记录. 
  ```

  ```bash
  ##单表删除
  delete from  `zeaho`.`admin` WHERE `id` = 44;
  
  ##多表联合删除
  delete u from user u, admin a where a.id = u.creator_id and u.name = 'real_name';
  ```

- 排序

  ```bash
  SELECT field1, field2 FROM table_name ORDER BY field1, [field2...] [ASC [DESC]]
  ```

  ```bash
  SELECT * from user where id > 1 ORDER BY score DESC,id ASC;
  ```

- 分组

  ```bash
  SELECT column_name, function(column_name)
  FROM table_name
  WHERE column_name operator value
  GROUP BY column_name;
  
  GROUP BY 语句根据一个或多个列对结果集进行分组. 
  在分组的列上我们可以使用 COUNT(总数), SUM(求和), AVG(平均值),等函数. 
  ```

  测试数据表

  ```bash
  CREATE TABLE `sign_in` (
    `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键',
    `user_id` bigint(20) unsigned NOT NULL COMMENT '用户id',
    `date` date NOT NULL COMMENT '签到时间',
    `score` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '上课评分',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
  
  ## 查看测试数据
  INSERT INTO `sign_in` VALUES (1, 1, '2019-01-31', 1);
  INSERT INTO `sign_in` VALUES (2, 2, '2019-01-23', 2);
  INSERT INTO `sign_in` VALUES (3, 2, '2019-01-19', 5);
  INSERT INTO `sign_in` VALUES (4, 1, '2019-01-19', 7);
  ```



  演示：

  ```bash
  ## COUNT
  ## 查询签到次数
  SELECT `user_id`, COUNT(*) FROM  sign_in GROUP BY user_id;
  
  ## SUM
  ## 查询评分总数
  SELECT `user_id`, SUM(score) FROM  sign_in GROUP BY user_id;
  
  ## AVG
  ## 查询平均得分
  SELECT `user_id`, AVG(score) FROM  sign_in GROUP BY user_id;
  ```



### Expansion

- 数据库字段的选择
