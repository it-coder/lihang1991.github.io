---
title: mysql三种安装方式之-rpm方式安装
toc: true
tags:
  - linux
  - centos
  - mysql
  - rpm
abbrlink: dceaf414
date: 2018-12-28 16:47:00
keywords: linux, mysql
description: mysql rpm方式安装
---

### 查看安装以及卸载
``` shell
# 查看
rpm -qa | grep mysql
# 卸载
yum -y remove mysql-libs-5.1.66-2.el6_3.x86_64
```

### 安装包资源地址

[<i class="fa fa-download"></i>mysql下载地址](https://dev.mysql.com/downloads/mysql/)

``` bash
# 下载资源名称
mysql-5.7.22-1.el7.x86_64.rpm-bundle.tar
```
<!--more-->


### 解压资源

```
tar -xvf mysql-5.7.22-1.el7.x86_64.rpm-bundle.tar 
```

### 获取rpm包

```
mysql-community-common-5.7.19-1.el6.x86_64.rpm
mysql-community-client-5.7.19-1.el6.x86_64.rpm
mysql-community-server-5.7.19-1.el6.x86_64.rpm
mysql-community-libs-5.7.19-1.el6.x86_64.rpm
```

### 安装

```
# 安装顺序
# common --> libs --> clients --> server
# 安装命令
rpm -ivh mysql-community-common-5.7.19-1.el6.x86_64.rpm mysql-community-libs-5.7.19-1.el6.x86_64.rpm mysql-community-client-5.7.19-1.el6.x86_64.rpm mysql-community-server-5.7.19-1.el6.x86_64.rpm
```
> 1.centos7需要先卸载mariadb，卸载命令<code>rpm -qa | grep mariadb</code>

> 可能需要依赖包<code>libaio</code>
> 安装命令<code>yum -y install libaio</code>

### 修改数据库初始化路径

先检查linux配置：<code>vim /etc/selinux/config</code>,将
<code>SELINUX=permissive</code>改成<code>SELINUX=disabled</code>

+ 配置`[mysqld]`
```  shell
# 创建目录
mkdir -p /home/data/

# 修改数据库配置
# vim /etc/my.conf

# 修改为
datadir=/home/data
socket=/home/data/mysql.sock
````

+ 配置`[client]`

```
[client]
socket=/home/data/mysql.sock

```
> `[mysqld]`是数据库服务的配置项，`[client]`是数据库客户端链接的配置项，初始化目录变动后，不配置该项本地命令连接时会出现sock文件找不到的错误


### 初始化

``` shell
mysqld --initialize --user=mysql
```
### 启动
```
service mysqld start
```

### 登陆
```
# 查看初始密码
cat /var/logs/mysql.log | grep 'temporary password'
# 登陆
mysql -uroot -p'123456'
# 修改初始密码
set PASSWORD=PASSWORD('密码');
```

### 授权远程链接

``` sql
### 授权192.168.0.1可以远程链接*[所有数据库].*[所有表] 账号：root 密码123456
grant all privileges on *.* to 'root'@'192.168.0.1' identified by '123456';

### 授权所有ip
grant all privileges on *.* to 'root'@'%' identified by '123456';

### 以上*.*表示[数据库].[表]


### 使授权立刻生效
flush privileges;
```
