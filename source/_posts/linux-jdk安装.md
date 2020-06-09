---
title: linux-jdk安装
tags:
  - jdk
  - java
  - linux
top: false
comments: false
categories: java
keywords: 'jdk, java'
abbrlink: f6b672db
date: 2020-05-21 19:36:09
permalink:
description:
image:
---


### 查看jdk是否已经存在

``` 
ps -ef |grep jdk
rpm -qa | grep jdk
```

> 卸载命令`yum -y remove [name]`

### 安装

```
rpm -ivh jdk-8u144-linux-x64.rpm
```

### 环境配置

`vim /etc/profile`
```
export JAVA_HOME=/usr/java/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export CLASSPATH=$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```

刷新配置`source  /etc/profile`

<hr />