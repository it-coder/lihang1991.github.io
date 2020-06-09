---
title: linux-软件安装卸载
tags:
  - linux
top: false
comments: false
categories: linux
keywords: linux
abbrlink: 5df5a9e8
date: 2020-05-21 19:33:51
permalink:
description:
image:
---


## ubuntu软件安装方式


### 1. apt方式安装

+ 普通安装`apt-get intsall name`
+ 修复安装`apt-get -f install name`
+ 重新安装`apt-get -reinstall install name`

+ 移除式卸载`apt-get remove name`
+ 清除式卸载`apt-get -purge remove name`
+ 清楚式卸载`apt-get purge name`

### 2. dpkg方式安装

+ `dpkg -i name.deb`

+ 移除式卸载`dpkg -r name`
+ 清除式卸载`dpkg -P name`

+ 查询软件`dpkg l name`

### 3. 源码方式安装

``` shell

./configure
make
sudo make install
```

### 软件的卸载

<hr />