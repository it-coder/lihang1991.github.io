---
title: reids6伪集群搭建
tags:
  - redis6
  - linux
top: false
comments: false
categories: redis
keywords: redis6
abbrlink: 2f3e15ea
date: 2020-05-21 17:32:53
permalink:
description:
image:
---


资源有限，在一台机器上搭建了6个节点的redis集群，记录搭建过程已经遇到的问题。
在搭建过程中发现搭建方式和之前的版本差别挺大的

## 环境

+ Centos7
+ redids6

## redis命令

### 启动、停止、重启
```
./redis-server [conf]
./redis-server stop
./redis-server restart
```

### 连接命令

```
./redis-cli [-h] [-p] [-a]
```
参数说明：`-h`连接ip(和配置文件中的`bind`属性有关)，
`-p`连接端口，
`-a`密码(配置文件`requirpass`属性)

### 操作命令

| 命令 | 举例 | 说明 |
| --- | --- | --- |
| `auth`| auth 123456 | 连接后认证|
| `select`| select 15 | 选择数据库(redis单机版默认16个)|
| `keys` | keys * | 查询所有key |
| `set` | set name lisi | 保存key-value |
| `get` | get name | key获取value |
| `flushall` | - | 清除所有数据　|
| `cluster nodes` | - | 查看节点 |
| `info` | - | 内存使用情况|
| `slowlog get` | slowlog get 2| 获取２条慢记录| 

## 集群搭建

<!-- more -->

环境准备阶段redis6和之前的版本没差别

### 配置文件准备

先修改redis基础配置文件redis.conf.default

```
port 6379
requirpass 123456 # 密码
bind 192.168.1.17  # 绑定当前机器 IP
cluster-enabled yes # 取消注释，启动集群模式
cluster-config-file nodes-6379.conf # 取消注释
cluster-node-timeout 15000 # 取消注释
appendonly yes # 将 no 修改为 yes
```

### 集群配置文件

```
cd /usr/local/redis/conf
echo 9001.conf 9002.conf 9003.conf 9004.conf 9005.conf 9006.conf | xargs -n 1 cp -v redis.conf.default
sed -i 's/6379/9001/g'  9001.conf 
sed -i 's/6379/9002/g'  9002.conf 
sed -i 's/6379/9003/g'  9003.conf 
sed -i 's/6379/9004/g'  9004.conf 
sed -i 's/6379/9005/g'  9005.conf 
sed -i 's/6379/9006/g'  9006.conf 
```

配置完成后，各自启动`./redis-server ../conf/9001.conf`

### 集群配置

```
/usr/local/redis/src/redis-cli -p 9001 -a 123456 --cluster create --cluster-replicas 192.168.1.17:9001 192.168.1.17:9002 192.168.1.17:9003 192.168.1.17:9004 192.168.1.17:9005 192.168.1.17:9006

```
redis之前的版本启动命令是`/usr/local/redis/src/redis-trib.rb create --replicas 1 192.168.2.123:9001 192.168.2.123:9002 192.168.2.123:9003 192.168.2.123:9004 192.168.2.123:9005 192.168.2.123:9006`,命令类似，就是将redis-trib.rb的功能集成到了redis-cli中。
使用`./redis-cli --cluster help`获取cluster命令说明

配置完成后登录节点使用`cluster nodes`查看主从节点配置情况

## 配置过程中遇到的问题

### slot已经占用

```
[ERR] Node 20.21.1.172:9001 is not empty. Either the node already knows other nodes (check with CLUSTER NODES) or contains some key in database 0.
```
说明redis节点存在已经被使用的情况，建立集群前要先保证各节点为空。

登录各节点，使用`cluster nodes`查看插槽slot是否为空，不为空使用`flushall`和`cluster reset`重置，
重置后再进行集群创建








<hr />