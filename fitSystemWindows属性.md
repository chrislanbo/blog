---
title: fitSystemWindows属性
date: 2017-03-06 09:41:15
tags: [android]
---


1. Android5.0以上：material design风格，半透明(APP 的内容不被上拉到状态) 
2. Android4.4(kitkat)以上至5.0：全透明(APP 的内容不被上拉到状态) 
3. Android4.4(kitkat)以下:不占据status bar

主题： 
使用Theme.AppCompat.Light.NoActionBar(toolbar的兼容主题):既可以适配使用toolbar(由于google已经不再建议使用action bar了，而是推荐使用toolbar，且toolbar的使用更加的灵活，所以toolbar和actionbar的选择也没什么好纠结的)和不使用toolbar的情况(即自定义topBar布局)。

fitSystemWindows属性： 
官方描述: 
Boolean internal attribute to adjust view layout based on system windows such as the status bar. If true, adjusts the padding of this view to leave space for the system windows. Will only take effect if this view is in a non-embedded activity. 
简单描述： 
这个一个boolean值的内部属性，让view可以根据系统窗口(如status bar)来调整自己的布局，如果值为true,就会调整view的paingding属性来给system windows留出空间…. 
实际效果： 
当status bar为透明或半透明时(4.4以上),系统会设置view的paddingTop值为一个适合的值(status bar的高度)让view的内容不被上拉到状态栏，当在不占据status bar的情况下(4.4以下)会设置paddingTop值为0(因为没有占据status bar所以不用留出空间)。
