---
title: react-native运行缺少sdk错误
date: 2017-04-13 11:08:46
tags: [react-native]
categories: [react-native]
---
出现这个错误，如果不是真的没有下载sdk
或者文件路径不对
那么就是缺少local.properties文件了

手动创建一个（在android项目下创建）
内容
```
sdk.dir=/Users/apple/Library/Android/sdk
##我这个路径仅供参考，换成你自己的
```


