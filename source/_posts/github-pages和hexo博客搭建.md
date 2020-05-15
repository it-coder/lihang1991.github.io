---
title: github pages和hexo博客搭建
categories: hexo
tags:
  - blog
  - hexo
  - github
abbrlink: 869e96f6
date: 2018-12-28 12:11:14
keywords: github, hexo, 博客
description: hexo搭建github博客
---

### 前期准备工作

+ 安装[git](https://git-scm.com/downloads)
+ 安装[node](http://nodejs.cn/)

### 本地初始化hexo

``` bash
# 全局安装hexo脚手架
npm install hexo-cli -g
# 当前目录创建myblog,并在myblog下初始化hexo
hexo init myblog
cd myblog
# 安装依赖
npm install
```
<!--more-->
### hexo常用命令

命令 | 作用
--- | ---
hexo g | 编译(hexo generate)
hexo s | 启动hexo本地服务(hexo server)
hexo d | 上传(hexo deploy),前提是配置好git弟子
hexo n 'page' | 新建页面(hexo new ''或者hexo new page '')

### github项目准备

+ 新建项目,项目名称<code>[github name].github.io</code>
> 没有github的先去注册[github](https://github.com)账号

+ 创建完成后，复制该项目的git地址
> git@github.com:it-daily/it-daily.github.io.git

+ 将git地址复制到hexo的_config.yml
``` bash
deploy:
  type: git
  repo: git@github.com:it-daily/it-daily.github.io.git
  branch: master
```

+ 生成SSH key
``` bash
# 检查当前用户.ssh下是否存在id_rsa和id_rsa.pub这两个文件,不存在生成
ssh-keygen -t rsa -C "youremail@example.com"

# 一路回车，在用户主目录里找到.ssh目录，里面有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥

```

+ 登陆 GitHub设置ssh key
> 打开“Account settings”，“SSH Keys”页面，点“Add SSH Key”，填上任意 Title，在 Key 文本框里粘贴id_rsa.pub文件的内容

+ 生成博客的静态文件
``` bash
hexo g
```
+ 安装hexo git插件
``` bash
npm install hexo-deployer-git --save
```
+ 上传静态文件到GitHub
``` bash
hexo d
```

### 博客maupassant主题

请参照[maupassant](https://www.haomwei.com/technology/maupassant-hexo.html)