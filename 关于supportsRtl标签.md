---
title: 关于 android:supportsRtl 标签
date: 2014-09-28 15:32:49
tags: [supportsRtl,布局]
---

#关于 android:supportsRtl 标签

	
	android4.2的一个新特性 layoutRtl，主要是方便开发者去支持阿拉伯语/波斯语等阅读习惯是从右往左的情况。
	
	而使用 android:supportsRtl="true"要求最低SDK版本为17，注意最低SDK配置
	
	可以在manifest的application标签添加：android:supportsRtl 取值：true/false
	
	为了国际化适配，所以在写xml布局的时候，不推荐layout_marginRight这类标签，推荐使用layout_marginEnd这类。