---
title: FastDFS
tags: [FastDFS]
top: false
comments: false
date: 2020-06-28 15:31:40
permalink:
categories:
description:
image:
keywords: FastDFS
---

FastDFS是一款开源的高性能分布式的文件系统。非常适合[0.4M - 500M]的小附件的储存和管理

## FastDFS简介

主要包含三个组件：跟踪服务器(Tracker Server)、储存服务器(Storage Server)、客户端(Client)

1. **跟踪服务器(Tracker Server)**:主要负责调度工作；负责管理 Storage Server 和 Group (每个 Storage Server 启动是会连接 Tracker ，告知自己的 Group 信息并保持心跳)
2. **储存服务器(Storage Server)**: 储存服务器，提供容量、备份服务，以 Group 为单位， 每个 Group 内可以有多个 Starage Server，数据互相备份。
3. **Client**: 客户端，提供数据的上传和下载服务

详细的 FastDFS 设计，请参阅[官方文档](https://github.com/happyfish100/fastdfs) 

<!-- more -->

## FastDFS 安装

### 下载安装 libfastcommon

``` shell
#   github address:  https://github.com/happyfish100/libfastcommon.git
#   gitee address:   https://gitee.com/fastdfs100/libfastcommon.git

git clone https://github.com/happyfish100/libfastcommon.git
cd libfastcommon; git checkout V1.0.43
./make.sh clean && ./make.sh && ./make.sh install

```

### 下载安装 fastdfs

```
#   github address:  https://github.com/happyfish100/fastdfs.git
#   gitee address:   https://gitee.com/fastdfs100/fastdfs.git

git clone https://github.com/happyfish100/fastdfs.git
cd fastdfs; git checkout V6.06
./make.sh clean && ./make.sh && ./make.sh install
```

### 设置配置文件

开发者给我们提供了配置示例，在`*.conf.sample`中

+ Tracker 配置
编辑`tracker.conf`

```
# 配置文件是否不生效，false 为生效
disabled=false

# 提供服务的端口
port=22122

# Tracker 数据和日志目录地址(根目录必须存在,子目录会自动创建)
base_path=/home/data/tracker

# HTTP 服务端口
http.server_port=9100
```

+ Storage 配置

```
# Storage 数据和日志目录地址(根目录必须存在，子目录会自动生成)
base_path=/home/data/storage


# tracker_server 的列表 ，会主动连接 tracker_server
# 有多个 tracker server 时，每个 tracker server 写一行
tracker_server=192.168.1.111:22122

# 访问端口
http.server_port=9200

```

+ Client 配置

```
# Client 的数据和日志目录
base_path=/home/data/client

# Tracker端口
tracker_server=192.168.1.111:22122
```

### 服务的启动、重启、关闭

对应命令`start、restart、stop`

``` shell
# tracker start
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf start

#  storage start
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf start

# 查看 Tracker 和 Storage 的通信情况
/usr/bin/fdfs_monitor /etc/fdfs/client.conf 

```

### 测试 FastDFS 

上传命令上传文件
```
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf [file name]
```
上传成功将返回`group1/M00/00/00/FBUBrF74UbyAC9CVAAAAT_dlnYg009.txt`类似的文件路径

## nginx 安装配置

FastDFS 需要配合 nginx 的 fastdfs-nginx-module 模块使用

+ nginx 和 fastdfs-nginx-module 模块的安装

```
git clone https://github.com/happyfish100/fastdfs-nginx-module.git --depth 1
cp /usr/local/src/fastdfs-nginx-module/src/mod_fastdfs.conf /etc/fdfs
```

```
wget http://nginx.org/download/nginx-1.15.4.tar.gz #下载nginx压缩包
tar -xvf nginx-1.15.4.tar.gz #解压
cd nginx-1.15.4/
#添加fastdfs-nginx-module模块
./configure --add-module=/usr/local/src/fastdfs-nginx-module/src/ 
make && make install #编译安装

```

+ fastdfs-nginx-module 配置

```
vim /etc/fdfs/mod_fastdfs.conf
#需要修改的内容如下
tracker_server=192.168.52.2:22122  # 服务器1
tracker_server=192.168.52.3:22122  # 服务器2
tracker_server=192.168.52.4:22122  # 服务器3
#storage 的 server port相同
storage_server_port=23000
# Storage 配置的store_path0路径，必须和storage.conf中的一致
store_path0=/mnt/sdb/fastdfs/data/file

url_have_group_name=true
```
+ nginx 配置

```
vim /usr/local/nginx/conf/nginx.conf

#添加如下配置
server {
    listen       9200;    ## 该端口为storage.conf中的http.server_port相同
    server_name  localhost;
    location ~/group[0-9]/ {
        ngx_fastdfs_module;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
    root   html;
    }
}
```


## 使用 FastDHT 实现相同附件的过滤

主要目的是节省储存空间，通过计算文件的MD5值并储存，实现相同文件只储存一份，其他的地址都是通过软链接。


<hr />