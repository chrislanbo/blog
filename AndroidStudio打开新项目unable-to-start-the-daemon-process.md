---
title: AndroidStudio打开新项目unable to start the daemon process
date: 2017-03-09 10:16:17
tags: [as,android,项目报错]
---

很多同学新建或者打开别的项目的时候，会报错
`unable to start the daemon process`
...
`Could not reserve enough space for <1572864KB> object heap` 
><>中的数字因人而异

其实是你项目文件夹下的gradle.properties文件中的一个配置导致电脑分配给as的内存不够用

我的项目显示的是
`org.gradle.jvmargs=-Xmx1536m`

换算一下单位正好匹配
`1536 * 1024 = 1572864`

#【解决方法】
直接修改
`org.gradle.jvmargs=-Xmx1536m` --> `org.gradle.jvmargs=-Xmx512m`

