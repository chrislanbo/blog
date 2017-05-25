---
title: 'Vue出坑手册-控件android对齐，但ios不对齐 '
date: 2017-05-25 09:16:40
tags: [vue]
categories: [vue]
---

>状态： 设置的padding 和 margin 明明一致，在android上是对齐的
但是在ios上就是不对齐的

解决： px 和 rem 单位要一致使用，并且布局元素会由外到内依次为margin、border、padding、content
如果你的布局有border，使用margin和padding即使用相同的距离，在android也许会对齐，但是在ios上就不是对齐的了。

建议：
统一使用padding或者margin来对齐单独控件
统一使用px或者rem单位


