---
title: react-native学习笔记
date: 2017-04-13 10:09:27
tags: [react-native]
---


管理本地rn
1. 查看本地版本
react-native —version
2. 更新本地rn版本
npm update -g react-native-cli
3. 查询rn的npm包中的版本
npm info react-native 
4. 升级或者降级到指定版本
npm install —save react-native@0.18
5. 更新项目templates文件（可选）
react-native upgrade
6.webStorm代码提示
将下载的ReactNative.xml 复制到webStorm/templates/目录中后重启

摇一摇出不来设置页面直接输入 
adb shell input keyevent 82

<!-- more -->

TextInput的常见属性和使用
TextInput继承自UIView，所以View的属性、样式TextInput也同样拥有。
onChangeText函数监听输入 例子：onChangeText={(text) =>{this.setState({input:text})}}
属性
value (String) 文本输入默认值
keyboardType 键盘类型
multiline(boolean) 多行输入
password(boolean) 密码文本
placeholder(String) 占位符（hint）
placeholderTextColor


Cannot read property ‘scrollResponderScrollTo’ of undefined
明明存在的方法却说未定义，看看是否导入，不行就重新打包

justifyContent:’flex-start(default)|flex-end|center|space-between|space-around’
该属性定义了伸缩项目在主轴的对齐方式
居左
居右
居中
两端对齐
平均分布，并两端保留一半间隔空间

alignItems:’flex-start|flex-end|center|stretch(default)’
侧轴的对齐方式
居上
局下
居中
基线对齐

