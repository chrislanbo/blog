---
title: viewpager左右无限轮播(手自一体)
date: 2016-10-19 21:40:27
tags: [viewpager]
---

首先创建一个简单的viewpager

instantiateItem(ViewGroup container, int position)
第一个参数为当前加载的item的父控件(viewPager对象)
第二个参数为当前初始化的item的下标
	首次加载 viewpager初始加载.jpg
由于初始加载集合中的第一个view，会缓存第二个view
左滑会缓存第三个，显示第二个
...依次类推

destroyItem(ViewGroup container, int position, Object object)
第一个参数为当前加载的item的父控件(viewPager对象)
第二个参数为当前初始化的item的下标

当当前显示的item为pos[1]对应的item时，左侧和右侧都缓存了一个item，只不过不可见
紧接着左滑会先从container中移除第一个item[pos0],然后加载第四个item[pos3]
	左滑右边缓存一个左边消除一个.jpg
setOffscreenPageLimit(int limit)
设置保留的item页面个数
当该参数设置为3的时候，上述首次加载就会变成
	setOffscreenPageLimit设置3保留了三个item并仍然缓存一个.jpg
并且右滑三次只会缓存item，不会移除item,直到滑到第四个才会移除第一个item
	setOffscreenPageLimit设置3会左右各缓存3个.jpg
也就是setOffscreenPageLimit设置3 会左右各缓存三个