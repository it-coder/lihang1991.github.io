---
title: linux-nginx安装
tags:
  - linux
  - nginx
top: false
comments: false
categories: nginx
keywords: nginx
abbrlink: 11b94a1d
date: 2020-05-21 19:41:53
permalink:
description:
image:
---



### 安装rpm

```
rpm -ivh nginx-release-centos-6-0.el6.ngx.noarch.rpm

yum info nginx

yum install nginx
```

目录
1) 配置所在目录：/etc/nginx/

2) PID目录：/var/run/nginx.pid

3) 错误日志：/var/log/nginx/error.log

4) 访问日志：/var/log/nginx/access.log

5) 默认站点目录：/usr/share/nginx/html

5 常用命令

1) 启动nginx：nginx

2) 重启nginx：nginx -s reload

3) 测试nginx配置：nginx -t

<hr />