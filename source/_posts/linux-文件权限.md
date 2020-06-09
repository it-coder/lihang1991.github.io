---
title: linux-文件权限
tags:
  - linux
  - permission
top: false
comments: false
categories: linux
keywords: 'linux,　文件权限'
abbrlink: 52ee900f
date: 2020-05-21 19:39:46
permalink:
description:
image:
---


### 文件权限详解以及常用操作

#### 文件所有者

| 字符 | 说明 |
| --- | --- |
| `u` | 文件所有者 |
| `g` | 所有者所在群组 |
| `o` | 其他所有的人 不包含`u`和`g` |
| `a` | 全部的人 |

#### 文件权限

| 字符 | 说明 |
| --- | --- |
| `r` | 读权限 |
| `w` | 写权限 |
| `x` | 文件可执行权限 |
| `-` | 无权限 |

#### 文件类型

| 字符 | 说明 |
| --- | --- |
| `p` | 管道文件 |
| `d` | 文件夹 |
| `l` | 链接文件 |
| `-` | 普通文件 |
| `s` | socket文件 |
| `c` | 字符设备文件 |
| `b` | 块设备文件 |

#### 查看文件权限

命令：`ls -l`,返回结果`drwxrwxrwx 7 lih root`
`drwxrwxrwx`这十位下表计数为：9 8 7 6 5 4 3 2 1 0

0-2：其他用户权限
3-5：root用户组权限
6-8：lih用户权限
9：文件或文件夹类型

后面`lih root`
其中`lih`代表文件所有者，`root`所有者所在的群组


#### 修改权限命令`chmod`

| 命令 | 说明 |
| --- | --- |
| `sudo chmod a+wrx test.sh` | 给全部的人添加test.sh文件的读写以及可执行权限 |
| `sudo chmod a-wrx test.sh` | 给全部的人删除test.sh文件的读写以及可执行权限 |

> 动作命令： `-`删除权限 `+`添加权限，该动作将文件所有者和文件权限关联

#### 改变文件夹或文件所有者命令`chown`

| 命令 | 说明 |
| --- | --- |
| `sudo chown lih /home/test.sh` | 将test.sh文件的所有者改为lih |
| `sudo chown :lih /home/test.sh` | 将test.sh的所在组改为lih |
| `sudo chown lih:lih /home/test.sh` | 合并上面二项 |


#### 命令补充

在上述所有命令基础上新增`-R`代表对下级子级目录生效
举个栗子:
`sudo chmod -R a+w /home`,给home以及下级所有人可写权限





<hr />