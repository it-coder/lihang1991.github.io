---
title: linux-mint安装使用
tags: []
top: false
comments: false
categories: linux
keywords: 'linux, linux-mint, ubuntu'
abbrlink: 2afa923e
date: 2020-05-21 19:45:45
permalink:
description:
image:
---


# linuxmint-note
> 准备入linux的坑，先在虚拟机中折腾，记录下使用心得和技巧！


### 个人环境

+ linux mint 19.2
+ [资源地址](https://mirrors.tuna.tsinghua.edu.cn/linuxmint-cd/stable/19.2/)


### 常用开发命令

``` shell
apt-cache search package #搜索包
sudo apt-get -f install #修复安装"-f = --fix-missing"
sudo apt-get remove package # 删除包
sudo apt-get remove package --purge #删除包，包括删除配置文件等
sudo apt-get dist-upgrade #升级系统
sudo apt-get clean && sudo apt-get autoclean #清理无用的包

#系统apt安装的软件包通常都是下载到如下文件夹：
#/var/cache/apt
```

<!-- more -->
### 安装输入法

+ 需要安装使用fcitx框架
+ 下载安装搜狗输入法
``` shell
sudo wget http://cdn2.ime.sogou.com/dl/index/1571302197/sogoupinyin_2.3.1.0112_amd64.deb?st=FB5F9AtdR58j1bisQL2chQ&e=1575878906&fn=sogoupinyin_2.3.1.0112_amd64.deb

sudo apt-get install ~/${path}/sogoupinyin_2.3.1.0112_amd64.deb
```

+ 解决文字颜色淡的问题
```
sudo apt-get install language-selector-*
sync
reboot
```

### 微信QQ安装


+ 最新的安装方法(update:2019-12-16)(谢谢[凌风](https://forum.ubuntu.org.cn/viewtopic.php?f=73&p=3217021&sid=6194a64cefc1f4c5ac43dcd8729ca3c8))
```shell
#!/bin/bash
echo "deb [trusted=yes] https://mirrors.aliyun.com/deepin stable main contrib non-free" | sudo tee /etc/apt/sources.list.d/deepin.list
sudo apt update
sudo apt install -t bionic deepin.com.wechat deepin.com.qq.im -fy
sudo rm -f /etc/apt/sources.list.d/deepin.list
sudo apt update
```

+ 卸载

``` shell
# 请注意 apt install -t bionic
# 添加-t 参数，会优先填补ubuntu 系统源，自动安装最新的deepin-wine，如果老旧了卸载方法
sudo apt purge deepin.com.* -fy
sudo apt autoremove -fy
```

+ [deepin软件资源地址](http://mirrors.aliyun.com/deepin/pool/non-free/d/)
+ [deepin框架资源下载地址](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin-wine/)

> 原github的deepin-wine-ubututu项目因有一年未维护所以无法安装成功了.
> 根据教程下载安装最新版本的deepin-wine框架 然后再安装微信等聊天工具

### jdk开发环境

+ 下载jdk1.8
+ 配置
> 用户级别：`./.bashrc`，系统级别： `/etc/profile`
``` shell
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_131    # 你的 JDK 解压路径
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

# 刷新
source ~/.bashrc
# 或 source /etc/profile
```

### window字体

window10字体路径`C:\WINDOWS\Fonts`
ubuntu字体路径`/usr/local/share/fonts/`
更新命令`sudo fc-cache -fv`

### sublinetext

``` shell
#下载Sublime Text 3存储库的安全密钥
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
# Sublime Text稳定库添加
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

sudo apt update && sudo apt install sublime-text

```


<hr />