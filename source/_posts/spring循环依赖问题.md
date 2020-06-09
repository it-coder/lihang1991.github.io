---
title: springboot版本升级
tags: []
top: false
comments: false
categories: spring
keywords: 'spring, springboot, 循环依赖'
abbrlink: b684cb3f
date: 2020-06-09 17:20:38
permalink:
description:
image:
---

因项目需要，需要将springboot版本从1.5.×升级到2.×，记录升级过程中遇到的问题以及解决方案

### bean已被注册问题

> bean could not be registered. A bean with that name has already been defined

您需要做的是启用Bean Overriding。从Spring2.1开始，此功能默认为禁用。
您将得到一个异常，因为已经定义了一个具有相同名称的bean
新增配置
``` yaml
spring:
   main:
     allow-bean-definition-overriding: true
```

<!-- more -->

### tomcat版本升级

不支持8.5.×,最好使用springboot默认内置的版本

### 循环依赖问题

在bean A中注入了B，但又在B中注入了A，项目启动时spring不知道先加载哪个bean导致的问题.
开启全局懒加载
``` yml
spring:
  main:
    lazy-initialization: true
```

### yml文件格式规范

在1.5.x的版本中，配置项中驼峰命名、小写加“-”隔开都是可以的，但2.1.x中明确要求只能用小写加“-”。

### servlet监听类变换
```
import org.springframework.boot.web.support.SpringBootServletInitializer;
包变成下面的
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

```

<hr />