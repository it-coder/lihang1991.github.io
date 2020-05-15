---
title: mysql备份策略(全量备份和增量备份)
tags: []
top: false
comments: true
abbrlink: 5a46718a
date: 2019-01-29 18:24:52
permalink:
categories: mysql
description: mysql备份策略-全量备份和增量备份
image:
keywords:
    - mysql
    - crontab
    - linux
---

## 技术点
+ shell
+ mysql
+ crontab

## 环境
+ 服务器:centos7
+ 数据库:mysql-5.7.19

## 准备工作
+ 开启bin-log
```conf /etc/my.cnf
log-bin=master-bin
```
+ 重启mysql
```bash 
service mysqld restart
```

<!-- more -->

## 全量备份脚本

```shell /home/shell/mysql-full-bak.sh
#!/bin/bash
source /etc/profile

# 常量
Bakdir=/home/backup
Logfile=/home/backup/bak.log
Date=`date +%Y%m%d`
Begin=`date +"%Y年%m月%d日 %H:%M:%S"`

cd $Bakdir
# 全量导出文件
Dumpfile=$Date.sql
# 压缩后文件
GzDumpfile=$Date.sql.tar.gz

# 全量导出jczldb_dev_dy数据库
mysqldump --single-transaction --master-data=2 --triggers --routines --databases jczldb_dev_dy --flush-logs --delete-master-logs --events  -uroot -p
'123456' > $Dumpfile

tar -zvcf $GzDumpfile $Dumpfile

rm -rf $Dumpfile

OldDate=`date -d '7 days ago' +%Y%m%d`

OldBakFile=${OldDate}".tar.gz"
# 删除七天前文件
rm -rf $OldBakFile

Last=`date +"%Y年%m月%d日 %H:%M:%S"`

# 写入备份日志
echo 开始:$Begin 结束:$Last $GzDumpfile >> $Logfile

cd $Bakdir/daily
# 全量备份后删除之前的增量备份文件
rm -f *
```

## 增量备份脚本

```shell /home/shell/mysql-daily-bak.sh
#!/bin/bash
source /etc/profile
BakDir=/home/backup/daily
BinDir=/home/mysqldata
LogFile=/home/backup/bak.log
BinFile=/home/mysqldata/master-bin.index

# 创建新的bin
mysqldump -uroot -p'123456' flush-logs
# bin文件数量
Counter=`wc -l $BinFile | awk '{print $1}'`

NextNum=0
# 循环所有的bin文件
for file in `cat $BinFile`
do
    base=`basename $file` # 取得bin文件名 如：master-bin.00001
    # 留取新生成的bin不处理
    NextNum=`expr $NextNum + 1`
    if [ $NextNum -eq $Counter ]
    then
       # 最新的bin停止处理
       echo $base 新生产bin文件不处理! >> $LogFile
    else
        # 判断已存在的bin不处理
        dest=$BakDir/$base
        if( test -e $dest )
        then
            echo $base 已存在! >> $LogFile
        else
            cp $BinDir/$base $BakDir
            echo $base 拷贝中 >> $LogFile
          fi
     fi

done
```

## 创建定时任务

+ 编辑命令
```bash
crontab -e
```

+ 配置
```conf
#每个星期日凌晨3:00执行完全备份脚本
0 3 * * 0 /bin/bash -x /home/shell/mysql-full-bak.sh 2>&1
#周一到周六凌晨3:00做增量备
0 3 * * 1-6 /bin/bash -x /home/home/mysql-daily-bak.sh 2>&1
```
+ 赋予可执行权限

```bash /home/shell/
chmod a+x mysql-full-bak.sh 
chmod a+x mysql-daily-bak.sh 
```

## 将二进制bin转为sql

```bash /home/mysqldata/
mysqlbinlog --no-defaults master-bin.0000003 > ../dump.sql
```

## 还原备份(待完善)

<hr />