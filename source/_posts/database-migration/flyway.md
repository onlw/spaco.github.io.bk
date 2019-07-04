---
title: flyway
permalink: flyway
categories:
- program
tags: 
- database migration
comments: true
date: 2018-12-14
updated: 2018-12-14
---

## Flyway

###  Introduction

Version control for your database.Robust schema evolution across all your environments.With ease, pleasure and plain SQL.

数据库的版本控制。跨所有环境的强大架构演变。轻松，愉快和简单的SQL

[官网](https://flywaydb.org/)

[How Flyway works](https://flywaydb.org/getstarted/how)

#### Development environment

- flyway 

  ```bash
  version : 5.2.4
  ```

- mysql

  ```bash
  version : 5.7
  ```

### Install

[docker-boxfuse-flyway](https://hub.docker.com/r/boxfuse/flyway/)

` 关注 Supported Volumes`

`见底部 docker-compose.yml `

### Configuration

` 见底部  Configuration ` 

### Test

#### Creating the first migration

Now create a first migration in the `/sql` directory called `V1__Create_person_table.sql`

```bash
vim V1__Create_person_table.sql
```

```bash
create table PERSON (
    ID int not null,
    NAME varchar(100) not null
);
```

#### Connect docker

```bash
docker-compose up -d flyway
docker-compose exec flyway bash
```

#### Migrating the database

[flyway migrate](https://flywaydb.org/documentation/commandline/migrate)

```bash
flyway migrate
```

If all went well, you should see the following output:

```bash
Creating Schema History table: `default`.`flyway_schema_history`
Current version of schema `default`: << Empty Schema >>
Migrating schema `default` to version 1 - Create person table
```

`version记录，可以在 db 的 flyway_schema_history 数据表中看到`

![](https://raw.githubusercontent.com/spaco/personal-graph-bed-lucky/master/blog/WX20181216-104924%402x.png)

#### Adding a second migration

If you now add a second migration to the `/sql` directory called `V2__Add_people.sql`

```bash
vim ./flyway/sql/V2__Add_people.sql
```

```bash
insert into PERSON (ID, NAME) values (1, 'Axel');
insert into PERSON (ID, NAME) values (2, 'Mr. Foo');
insert into PERSON (ID, NAME) values (3, 'Ms. Bar');
```

and execute it by issuing:

```bash
flyway migrate
```

you now get:

```bash
Database: jdbc:mysql://106.14.6.94:3306/default (MySQL 5.7)
Successfully validated 2 migrations (execution time 00:00.137s)
Current version of schema `default`: 1
Migrating schema `default` to version 2 - Add people
Successfully applied 1 migration to schema `default` (execution time 00:00.523s)
```

`migrate version 同样可以在 db 的 flyway_schema_history 数据表看到`

### Other command

#### flyway clean  ：慎用

[flyway clean](https://flywaydb.org/documentation/commandline/clean)

`Drops all objects (tables, views, procedures, triggers, …) in the configured schemas.
The schemas are cleaned in the order specified by the `schemas` property.`

`删除所有的 表 视图 过程 触发器`     所有～～

```bash
flyway clean
```

```bash
Database: jdbc:mysql://106.14.6.94:3306/default (MySQL 5.7)
Successfully cleaned schema `default` (execution time 00:00.224s)
# 此时所有的表都没了
```

#### flyway info

[flyway clean](https://flywaydb.org/documentation/commandline/info)

`Prints the details and status information about all the migrations.`

`打印有关所有迁移的详细信息和状态信息。`

#### flyway validate : 可作为 migration 前置命令

[flyway validate](https://flywaydb.org/documentation/commandline/validate)

假设 开发A 创建了 `V2__Add_user.sql` , 开发 B 创建了 `V2__Add_user2.sql',造成版本差错

此时：`ERROR: Found more than one migration with version 2`

```bash
flyway validate
```

```bash
Database: jdbc:mysql://106.14.6.94:3306/default (MySQL 5.7)
ERROR: Found more than one migration with version 2   # 版本2 migration more than one
Offenders:
-> /flyway/sql/V2__Add_people.sql (SQL)
-> /flyway/sql/V2__Add_people2.sql (SQL)
```

#### undo ： not supported by Flyway Community Edition

`undo the most recently applied versioned migration.`

[undo](https://flywaydb.org/documentation/commandline/undo)

[undo Important notes](https://flywaydb.org/documentation/commandline/undo)  ！！！

- This should be complemented with a **proper, well tested, backup and restore strategy** ： 

- undo fail : you end up creating home-made alternatives for restoring backups, which need to be properly tested as well.
- **maintain backwards compatibility between the DB and all versions of the code currently deployed in production** :This way a failed migration is not a disaster. The old version of the application is still compatible with the DB, so you can simply roll back the application code （即 迁移失败不可怕，回滚code保证老代码适应DB）

`撤消最近应用的版本化迁移 -target=** 确定版本名`

```bash
flyway undo
```

`undo is not supported by Flyway Community Edition`

```bash
ERROR: Flyway Pro Edition or Flyway Enterprise Edition upgrade required: undo is not supported by Flyway Community Edition.
```

#### baseline

[baseline](https://flywaydb.org/documentation/commandline/baseline)

`Baselines an existing database, excluding all migrations up to and including baselineVersion`

`生成 版本历史记录库，按照默认流程，版本记录库是已经生成的,默认表名：flyway_schema_history，此步骤可以跳过`

```bash
flyway baseline
# error
Database: jdbc:mysql://106.14.6.94:3306/default (MySQL 5.7)
ERROR: Unable to baseline schema history table `default`.`flyway_schema_history` as it already contains migrations
```

#### repair

[repair](https://flywaydb.org/documentation/commandline/repair)

`Repairs the Flyway schema history table. This will perform the following actions:`

`即 清理 failed migrations，保证版本 log 的完整性`

```bash
flyway repair

Successfully repaired schema history table `default`.`flyway_schema_history` (execution time 00:00.224s).
```

### Deploy

```bash
flyway validate
flyway migrate

#test last
```

### References

- [flyway.org](https://flywaydb.org/)

### QAQ

- docker-compose.yml 中，关于command 的设置无效，无法映射到 conf 下，导致 flyway.url 为空

  A :

  使用 volumes ：

  volumes:

  ​        \- "./flyway/sql/:/flyway/sql"

  ​        \- "./flyway/conf/:/flyway/conf"

### Extra

#### Configuration

- docker-compose.yml

  ```dockerfile
  ## 事实上，关于 url user 的配置放于 ./flyway/conf/flyway.conf 中，command 中的参数不重要，但是删除 也不行，会无法启动 flyway
  ### Flyway ################################################
      flyway:
        image: boxfuse/flyway:5.2.4  #也可以去掉版本号，使用最新的
        command: -url=jdbc:mysql://db -schemas=myschema -user=root -password=root -connectRetries=60 migrate
        volumes:
          - "./flyway/sql/:/flyway/sql"
          - "./flyway/conf/:/flyway/conf"
  ```

- /flyway/conf/flyway.conf

  `本次是 copy 一份 conf模版，然后配置相关信息，做 docker 映射 `

  [flyway.conf](https://flywaydb.org/documentation/configfiles)

  [flyway.conf模版](https://flywaydb.org/documentation/configfiles#structure)

  ```bash
  # 本次单纯设置以下三项基础配置， 其余的请自行配置
  flyway.url=jdbc:mysql://106.14.6.94:3306/db-name
  flyway.user=db-user-name
  flyway.password=db-password
  ```

#### docker compose 设置flyway environment 为pro

不行的～～

```bash
docker-compose logs flyway

ERROR: Missing license key. Ensure flyway.licenseKey is set to a valid Flyway license key ("FL01" followed by 512 hex chars)
```











