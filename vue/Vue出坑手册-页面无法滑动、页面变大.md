---
title: Vue出坑手册-页面无法滑动、页面变大
date: 2017-05-25 16:09:38
tags: [vue]
categories: [vue]
---


app中使用webview加载页面无问题，但是在手机浏览器中会偶现页面变大、和必现页面无法滑动的问题
看看你的css样式是否使用
`overflow auto `属性
overflow 属性规定当内容溢出元素框时调用的属性
设置为auto，如果内容显示不全，则浏览器会显示滚动条以便查看其余的内容
但是有些手机浏览器会不识别，需要去掉该属性
