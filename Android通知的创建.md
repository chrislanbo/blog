---
title: Android通知的创建
date: 2014-09-28 15:32:38
tags: [notification,android]
---
#Notification

Android 系统创建一条通知需要用到的两个类：`Notification`和`NotificationManager`

Notification类来定义状态通知的属性，比如图标、信息、提示信息，或者提示声音、呼吸灯、震动。
NotificationManager是一个android系统的服务，管理和运行所有的通知

	可通过getSystemService(Context.NOTIFICATION_SERVICE)方法获得句柄。
	调用notify(int, Notification)方法，弹出通知
	调用cancel(int)方法取消通知

