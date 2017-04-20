---
title: adjustViewBounds属性
date: 2017-03-06 11:18:07
tags: [android,布局]
---

android的xml布局中
android:adjustViewBounds（调整视图边界）
该属性用于保持宽高比，必须要和maxWidth、MaxHeight配合使用

java代码中
用于固定图片大小，且保持图片宽高比：
1） 设置setAdjustViewBounds为true；
2） 设置maxWidth或者MaxHeight；
3） 设置layout_width和layout_height为wrap_content。
