---
title: nginx添加新模块以及自定义http响应头
tags:
  - nginx
  - linux
categories: nginx
abbrlink: 15b18a2
date: 2019-01-23 18:24:38
permalink:
keywords: linux, nginx, headers-more-nginx-module
description: mysql添加新模块方案
image:
---


## 需求场景
编译好的nginx需要添加headers-more-nginx-module模块来自定义响应头

> nginx其它模块同样适用

## 环境 
- nginx包目录:/app/nginx-1.12.2
- nginx安装目录:/app/nginx112
- 插件目录:/app/tools/headers-more-nginx-module-0.33
- 系统环境centOs7

<!-- more -->

## 资源地址

- [<i class="fa fa-link"></i>headers-more-nginx-module-0.33github地址](https://github.com/openresty/headers-more-nginx-module)
- [<i class="fa fa-link"></i>查看插件支持的nginx版本github地址](https://github.com/openresty/headers-more-nginx-module#compatibility)
- [<i class="fa fa-link"></i>headers-more-nginx-module下载地址](https://github.com/openresty/headers-more-nginx-module/tags)
- [<i class="fa fa-link"></i>nginx下载地址](http://nginx.org/)

## 下载

``` shell
# 举例目录/app/tools
cd /app/tools/
#下载插件
wget https://github.com/openresty/headers-more-nginx-module/archive/v0.33.tar.gz
#解压
tar -zxvf v0.33.tar.gz
```

## 加载模块

``` shell
# 查看安装参数命令(取出：configure arguments:)
/app/nginx/sbin/nginx -V
# 在nginx资源目录编译
cd /app/nginx-1.12.2/
# 将上面取出的configure arguments后面追加 --add-module=/app/tools/headers-more-nginx-module-0.33
./configure --prefix=/app/nginx112 --add-module=/app/tools/headers-more-nginx-module-0.33
# 编辑，切记没有make install
make
# 备份
cp /app/nginx112/sbin/nginx /app/nginx112/sbin/nginx.bak 
# 覆盖(覆盖提示输入y)
cp -f /app/nginx-1.12.2/objs/nginx /app/nginx112/sbin/nginx
```

## 修改配置

``` shell
vim /app/nginx112/conf/nginx.conf
# 添加配置(在http模块)
more_clear_headers 'Server';
```
> 上面配置只是将http响应头中的Server:nginx/1.12.2清楚，详细使用方案可阅读[参考文档](https://github.com/openresty/headers-more-nginx-module),
支持添加·修改·清除响应头的操作，

## 重启nginx

``` shell
/app/nginx112/sbin/nginx -s stop
/app/nginx112/sbin/nginx
```
> 直接使用reload可能会无效

<hr />