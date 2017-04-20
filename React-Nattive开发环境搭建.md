---
title: React Native开发--1.环境搭建
date: 2016-09-25 15:06:34
tags: [js,React Native]
---

平台: Windows
项目: Android

Android SDK下载

	Tools/Android SDK Tools (24.3.3
	Tools/Android SDK Platform-tools (22)
	Tools/Android SDK Build-tools (23.0.1)（这个必须版本严格匹配23.0.1）
	Android 6.0 (API 23)/SDK Platform (1)
	Extras/Android Support Library(23.0.1)
	Extras/Android Support Repository

设置国内的镜像下载更快 : `http://android-mirror.bugly.qq.com:8080/include/usage.html`

配置SDK以及platform-tools目录到环境变量

1.  安装c++环境
（如果是v2015版本需要配置：）`npm config set msvs_version 2015 --global`
* 安装git环境
安装过程中注意选择`Run Git from Windows Command Prompt`。
* 安装python环境(暂时只支持2.7.x版本)
* 安装node.js（4.1或者更高版本）
```
（设置国内npm镜像）
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```
* 安装react-native命令行工具

	npm install -g react-native-cli

或者使用
管理员模式打开`cmd`
键入命令安装Chocolatey，这是一个Windows上的包管理器，方便下载软件

```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH="%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

键入`choco install python2`安装python2


安装好安装python2后，继续键入`npm install -g react-native-cli`(用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。)

	react-native init HelloWorld(不要在系统目录创建)
	react-native start
可以用浏览器访问http://localhost:8081/index.android.bundle?platform=android看看是否可以看到打包后的脚本
保持packager开启，另外打开一个命令行窗口，然后在工程目录下运行`react-native run-android`