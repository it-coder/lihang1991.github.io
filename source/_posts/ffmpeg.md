---
title: ffmpeg
tags:
  - ffmpeg
  - video
  - audio
  - mp3
top: false
comments: false
categories: ffmpeg
keywords: 'ffmpeg, mp3'
abbrlink: 2ffce39e
date: 2020-05-21 19:29:51
permalink:
description:
image:
---


### 资源

+ [ffmpeg](https://ffmpeg.org/releases/ffmpeg-4.2.2.tar.gz)

+ [lame](https://sourceforge.net/projects/lame/files/lame/3.100/lame-3.100.tar.gz/download)

+ [yasm](http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz)

ffmpeg默认不支持MP3编码,lame是一个MP3编码库

<!-- more -->

### 安装yasm

``` shell
tar -xvf yasm-1.3.0.tar.gz

cd yasm-1.3.0

./configure
make && make install
```

### 安装lame库

``` shell
tar -xvf lame-3.100.tar.gz

cd lame-3.100

./configure
make && make install
```

### 安装ffmpeg

启用lame编码库支持
``` shell
tar -xvf ffmpeg-4.2.2.tar.gz

cd ffmpeg-4.2.2

./configure --enable-libmp3lame
make && make install
```

### 简单使用记录

#### 合成音频

```
ffmpeg -i 1.mp3 -i 2.mp3 -i 3.mp3 -i 4.mp3 -i 5.mp3 -filter_complex amix=inputs=5:duration=first:dropout_transition=5 -f mp3 out.mp3

```
> 问题：音频合成后整理音量变低

#### ffprobe获取音频的信息json返回

```
ffprobe -v quiet -print_format json -show_format -show_streams 3.mp3
```

#### 使用`ffmpeg --version`报错

错误如下：
```
ffmpeg: error while loading shared libraries: libmp3lame.so.0: cannot open shared object file: No such file or directory

```

编缉`/etc/ld.so.conf`，新建一行追加：`/usr/local/lib`；保存后执行命令重加载配置文件：`ldconfig`

<hr />