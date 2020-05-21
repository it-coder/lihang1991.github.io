---
title: mysql修改存储地址
tags:
  - mysql
  - linux
top: false
comments: true
description: mysql存储地址的迁移
keywords:
  - mysql
  - linux
abbrlink: a3505c8d
date: 2019-01-28 19:05:34
permalink:
categories:
image:
---

## 环境

* mysql版本:mysql-5.7.19-log
* 原存储目录: /var/lib/mysql
* 目标存储目录: /home/mysqldata

## 停止mysql服务

``` shell 
service mysqld stop
```

## 建立新的存储目录

``` shell
mkdir /home/mysqldata
```

<!-- more -->

## 复制数据库到新的存储目录

``` shell
cp -av /var/lib/mysql/* /home/mysqldata
```

## 修改目录权限

``` shell
# 更改用户组/用户为mysql:mysql
chown -vR mysql:mysql /home/mysqldata/

# 更改访问权限为700
chmod -vR 700 /home/mysqldata/
```

## 修改数据库配置

``` conf /etc/my.conf
# vim /etc/my.conf

# 修改为
datadir=/home/mysqldata
socket=/home/mysqldata/mysql.sock
```


## 备注

``` shell
# 有时会出现文件权限问题 errorcode：13
# mysql默认安装目录
$ ls -ldZ /var/lib/mysql
# 结果
$ drwxr-x---. mysql mysql system_u:object_r:mysqld_db_t:s0 /var/lib/mysql

# 查看调整后的目录权限

$ ls -ldZ /home/mysqldata/
# 结果
$ drwxr-x---. mysql mysql unconfined_u:object_r:unlabeled_t:s0 /home/mysqldata/

# 修改
chcon -Rv -u system_u -t mysqld_db_t /home/mysqldata/

```

> 如果上面修改权限后还是因为权限问题无法启动,下面二种方式(推荐第二种)执行后再

> 1.可以先关闭selinux
> <code>vim /etc/selinux/config</code>,将
> <code>SELINUX=permissive</code> -> <code>SELINUX=disabled</code>，需要重启机器
> 2.也可以使用<code>setenforce 0 </code>临时关闭后再执行修改目录权限命令

<hr />