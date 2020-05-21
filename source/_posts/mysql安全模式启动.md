---
title: mysql安全模式启动
toc: true
tags:
  - linux
  - centos
  - mysql
top: false
comments: true
description: mysql忘记密码或者无法正常登录时使用安全模式修改密码
keywords: linux, mysql, skip-grant-tables
abbrlink: 40ace4df
date: 2020-05-15 12:59:37
permalink:
categories:
image:
---


### 修改配置文件

使用`vim /etc/my.cnf`,并在`[mysqld]`下新增配置：`skip-grant-tables`,
并`service mysqld resart`重启服务器

### 登录mysql

`mysql -uroot`

### 修改密码

```
# 切换数据库
use mysql 

#修改密码
update user set authentication_string = password('新密码') where user=”root”; 

#使修改立即生效
flush privileges;
```

<hr />