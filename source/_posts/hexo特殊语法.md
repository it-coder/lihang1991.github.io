---
title: 一些特殊用法
tags: []
top: false
categories: hexo
keywords: hexo
description: hexo一些特定语法
abbrlink: 4e463d48
date: 2019-01-25 14:35:24
permalink:
image:
---


## 代码块带目录

``` java custom.java
public void test() {

}
```

<!-- more -->

## tag

> + danger
> + default
> + primary
> + info
> + success
> + warning
> + danger
> + [option] [no-icon]

{% note danger %}
note text
note text
note text
{% endnote %}

{% note default  %}
note text
note text
note text
{% endnote %}


## tabs

{% tabs First unique name %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

<hr />