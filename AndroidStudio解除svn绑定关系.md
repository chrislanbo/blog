---
title: AndroidStudio解除svn绑定关系
date: 2017-03-15 15:56:34
tags: [svn,android]
---

取消svn绑定
删除idea目录下vcs.xml文件中的svn
```xml
<mapping directory="" vcs="svn" />
```
手动删除项目目录下的隐藏文件夹./svn

ko