---
title: Eclipse开启注解功能
date: 2017-03-06 15:37:59
tags:
---

在使用ButterKnife需要先配置一下Eclipse。
项目右键-Properties->Java Complier->Annotation Processing 确保设置全部勾选
若没有Annotation Processing条目，说明你的eclipse版本比较新，被谷歌默认删了（好推他们自己的注解）。
>解决方法:![](http://www.jcodecraeer.com/uploads/20150515/1431677856258018.png)

接着展开Annotation Processing选择Factory Path,选中Enable project specific settings。然后点击 Add JARs…,选中ButterKnife的jar包


确保你项目的根目录里有一个.apt_generated的文件夹，文件夹中包含YOURACTIVITY$$ViewInjector.java这样的文件，那样你就可以使用注解了。