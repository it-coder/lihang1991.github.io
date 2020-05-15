---
title: mysql gtid读写分离主从同步生产环境实战
tags:
  - mysql
  - gtid
categories: mysql
abbrlink: a95699cc
date: 2019-01-23 18:46:40
permalink:
keywords: linux, mysql, 主从同步
description: mysql gtid读写分离主从同步生产环境实战
image:
---

## 环境
- mysql-5.7.19
- Centos7
- master: 192.168.111.64
- slave: 192.168.111.66

## 主从配置

### master配置

``` text
## GTID
server_id = 100  # 服务器id，从库最好大于主库,注意要唯一
gtid_mode = on    #开启gtid模式
enforce_gtid_consistency = on    #强制gtid一致性，开启后对特定的create table不被支持
log-bin = mysql-bin    #开启二进制日志
binlog_format = row    #默认为mixed混合模式，更改成row复制，为了数据一致性
log-slave-updates = 1    #从库binlog才会记录主库同步的操作日志
skip_slave_start=1    #跳过slave复制线程
```

<!-- more -->
### slave配置

``` text
## 设置server_id，注意要唯一
server_id=101
log-bin = mysql-bin
binlog_format = row
log-slave-updates = 1
gtid_mode = on
enforce_gtid_consistency = on
skip_slave_start=1
# 从库设置为只读
read_only=on
```

## 重启数据库

``` shell
service mysqld restart
```

## 创建用户

``` sql
# 用户从库同步
GRANT SELECT,RELOAD,SHOW DATABASES,LOCK TABLES,EVENT,REPLICATION CLIENT  ON *.* TO 'repl_user'@'192.168.111.64' IDENTIFIED BY '123456';

# 创建只读用户，用户从库读取数据
GRANT Select ON *.* TO 'reader'@'192.168.111.%'  IDENTIFIED BY "123456";
```

## 数据导出

``` shell
# 在192.169.111.66上操作
nohup mysqldump --single-transaction --master-data=2 --triggers --routines --all-databases --events -h192.168.111.64 -u'root' -p'password' > /home/backup.sql &
```

> 因为数据量较大(导出大概30G)，使用<code>nohup [commend] &</code>  使用后台导出，防止导出时间过长连接断开，可以使用<code>jobs -l</code>命令查看

> 数据导出也可以直接在主库服务器上导出，然后通过<code>scp backup.sql 192.168.111.66:/home</code>命令传到从库服务器

## 从库数据导入

### 连接mysql导入

+ 登陆mysql

``` shell
# 连接到数据库
mysql -uroot -p'123456';
```
+ 执行导入sql命令

``` sql
source /home/backup.sql
```

### 命令直接导入

``` shell
nohup mysql -uroot -p’123456’ < /home/backup.sql &
```

> 推荐第二种方式，尤其是数据量较大的情况下

## 在192.168.111.66上设置主从连接

+ 连接mysql

``` shell
mysql -uroot -p'123456'
```
+ 设置

``` sql
# 设置
CHANGE MASTER TO MASTER_HOST='192.168.111.64', MASTER_PORT=3306,MASTER_USER='repl_user',MASTER_PASSWORD='123456',MASTER_AUTO_POSITION=1;

# 开启主从
start slave;

# 查看主从状态
show slave status \G;
```

> 常用命令:
>+ 停止同步:<code>stop slave</code>
>+ 重置从库状态:<code>reset slave</code>
>+ 重置主库状态:<code>reset master</code>

<hr />