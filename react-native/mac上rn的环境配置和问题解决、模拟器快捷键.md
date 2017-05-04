---
title: mac上rn的环境配置和问题解决、模拟器快捷键
date: 2017-04-13 10:06:17
tags: [环境配置]
---

export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

~表示用户目录，即/Users/你的用户名/
source ~/.bash_profile
可以使用echo $ANDROID_HOME检查此变量是否已正确设置。
本机jdk：  /Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home

/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home/bin/java

/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin/java

➜  ~ touch ~/.bash_profile 
➜  ~ vim ~/.bash_profile 
➜  ~ vim ~/.bash_profile
➜  ~ cd ~/Library/





export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_40.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH:
export PATH=/Library/Frameworks/Python.framework/Versions/2.7/bin:$PATH
export NODE_PATH="/usr/local/lib/node_modules"
export NVM_DIR="/Users/apple/.nvm"

export ANDROID_HOME=~/Library/Android/sdk

export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist

npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated lodash.assign@4.2.0: This package is deprecated. Use Object.assign.
npm WARN prefer global marked@0.3.6 should be installed with -g

<!-- more -->
检查：
npm -v minimatch
npm  install -g marked    
npm  install -g marked    
npm  uninstall lodash.assign
npm install Object.assign

FAILURE: Build failed with an exception.

* Where:
Build file '/Users/apple/Desktop/TE/android/app/build.gradle' line: 110

* What went wrong:
A problem occurred evaluating project ':app'.
> SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:installDebug'.
> com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: Failed to establish session

solution:
试着将项目目录/android/build.gradle中的Gradle版本改为1.2.3。

touch ~/.gradle/gradle.properties && echo "org.gradle.daemon=true" >> ~/.gradle/gradle.properties

so we can create a file named local.properties , and put some words in this file

进入项目文件夹。
运行程序 react-native run-android 
初始运行的时候会下载 gradle 2.4  有科学上网工具
或者自己找资源下载到本地 


css flex-box
flexdirection
row 水平排列
row-reverse 逆向水平排列
column 垂直排列 （默认）
column-reverse 逆向垂直排列
flexWrap
row
no wrap
wrap
wrap-reverse
justify -content

sudo chmod 755 *.sh
sudo sh startup.sh 


Found Xcode project AwesomeProject.xcodeproj
xcrun: error: active developer path ("/Applications/Xcode 2.app/Contents/Developer") does not exist, use `sudo xcode-select --switch path/to/Xcode.app` to specify the Xcode that you wish to use for command line developer tools (or see `man xcode-select`)

Command failed: xcrun instruments -s
xcrun: error: active developer path ("/Applications/Xcode 2.app/Contents/Developer") does not exist, use `sudo xcode-select --switch path/to/Xcode.app` to specify the Xcode that you wish to use for command line developer tools (or see `man xcode-select`)

mac 命令行里 编译 链接 出现xcrun: error: active developer path


mac cc 编译出现
xcrun: error: active developer path ("/Volumes/Xcode/Xcode.app/Contents/Developer") does not exist, use xcode-select to change

在命令行里输入
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer


mac 中的android studio 配置文件 在
/Users/apple/Library/Preferences/AndroidStudio2.x


Cmd+1/2/3       可以切换模拟器的显示比例。

Option+Shift     可以在模拟器中调出双指拖动效果。

Option      可以在模拟器中调出双指放大缩小效果。

command+Shift+H       模拟器的Home键。

Cmd+向左箭头/向右箭头       切换横竖屏



下载必备软件
软件列表
Python
地址：https://www.python.org/downloads/
Node.js
地址：https://nodejs.org/en/（下载LTS版本）
Java Development Kit（JDK）
地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
Gradle
地址：https://downloads.gradle.org/distributions/gradle-2.4-all.zip
Android Studio
地址：http://developer.android.com/
以上所有软件百度网盘下载地址：http://pan.baidu.com/s/1nvjkaAP

