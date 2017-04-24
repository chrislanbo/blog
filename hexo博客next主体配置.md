---
title: hexo博客next主体配置
date: 2017-04-21 09:40:13
tags: [hexo,博客主题]
categories:
---

# 配置文件位置

站点根目录下 _config.yml 称为 站点配置文件(Hexo 本身的配置) ，
主题目录下 _config.yml 称为 主题配置文件(配置主题)。

## 站点配置

title 标题名
author 作者名
language 语言  需要设置成 zh-Hans 否则有乱码
theme 主题 设置为theme目录下的主题名

```xml
deploy: 
  type: git
  repo: https://github.com/chrislanbo/chrislanbo.github.io.git
  branch: master
```

## 主题配置

将ico图标放到source目录下
Schemes Muse|Mist|Pisces（当前）
menu 设置菜单页面位置
avatar 头像链接


