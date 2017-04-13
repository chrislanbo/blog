---
title: Vue开发环境搭建双平台版
date: 2017-04-11 15:56:08
tags: [Vue,环境搭建]
---

### Homebrew, Mac系统的包管理器
在命令行输入
`cd ~ & /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"    `

### 如果你看到EACCES: permission denied这样的权限报错，可以使用下面的命令修复：
```
sudo chown -R `whoami` /usr/local
```
### 可能发生的错误
You have not agreed to the Xcode license.
Before running the installer again please agree to the license by opening
Xcode.app or running:
    sudo xcodebuild -license
查看协议：sudo xcodebuild -license
允许协议：在命令行输入agree 

### 安装node
`brew install node`

### 使用淘宝镜像
```
npm config set registry https://registry.npm.taobao.org --global npm config set disturl https://npm.taobao.org/dist --global
```

### 安装vue 
`npm install -g vue-cli`

