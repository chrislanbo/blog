---
title: 基于Retrofit+Okio+RxBus实现文件下载（带进度）
date: 2016-07-14 11:25:10
tags: [linux,blog,hexo] 
---
#基于Retrofit+Okio+RxBus实现文件下载（带进度）
retrofit处理文件下载，尤其是下载进度回调。
Retrofit是一个简化的HTTP请求库
由于没有提供显示文件下载进度的回调，下载文件的时候用户体验不佳