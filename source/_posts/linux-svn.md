---
title: linux-svn
tags:
  - svn
  - linux
top: false
comments: false
categories: svn
keywords: 'svn, linux'
abbrlink: dbeee8f
date: 2020-05-21 19:38:16
permalink:
description:
image:
---


## linux下svn常用命令

### 查提交记录

| 命令 | 说明　|
| --- | --- |
| `svn log` | 查最近提交记录　|
| `svn log -r {2020-02-02}:{2020-02-10}` | 查看时间段内的提交记录 |
| `svn log [remote]` | 查看远程地址的最近提交记录 |
| `svn log | grep -A 2 'name'` | 查看关键字name的提交记录 |
| `svn log -v` | 查看提交记录并显示提交明细 |
| `svn log -r {2020-03-03}:{2020-03-10} -v https://test/web/ | grep -A 2 'LiHang'` | 组合查询举例 |
| `svn log -r r84349 -v` | 查看r84349版本提交明细 |

<hr />