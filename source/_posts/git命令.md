---
title: git命令备忘录
tags: []
top: false
comments: false
abbrlink: 341ecd43
date: 2020-05-16 10:07:24
permalink:
categories: git
description:
image:
keywords: git
---


## 基础命令

| 命令 | 说明 |
| --- | --- |
| `git clone [url]` | 克隆 |
| `git branch -a` | 所有的分支 |
| `git checkout [branch]` | 切换分支 |
| `git checkout -b [branch]` | 创建并切换分支 |
| `git fetch [remote]` | 拉取[远程仓库] |
| `git push origin --delete <branchName>`| 删除远程分支
| `git remote -v` | 查看远程仓库 |
| `git remote add [name] [url]` | 添加远程仓库 |
| `git remote rm [name]` | 删除远程仓库 |
| `git diff [file name]` | 和远程仓库内容对比 |


## 使用问题

### 第一次提交github远程仓库

```
git init

git add remote origin [url]

git pull origin --allow-unrelated-histories


git push origin master
```


## github使用技巧

### github更新fork的仓库

#### 添加远程仓库(fork的源仓库)
```
git remote add upstream [url]
```

#### 拉取远程仓库更新
```
git fetch upstream

# 合并仓库
git pull upstream master
```
> 拉取更新后会产生一个新的upstream/master分支，可以和主分支合并

> 合并时，会弹出添加合并信息编辑器，git默认使用的是`nano`
> 填写内容后使用`Ctrl + x`，输入Y保存
>  可以使用`git config --global core.editor vim`修改成vim

#### 合并分支

先确保自己在master分支
```
git merge upstream/master
```


##

##

<hr />