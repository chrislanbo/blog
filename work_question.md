---
title: 工作问题集锦
date: 2017-05-14 11:25:19
tags: [view] 
---
##ViewPager+Fragment第二次进入显示空白
viewPager和Fragment搭配使用比较常见，可能会出现第一次进入正常显示，第二次进入的时候显示空白。
######状况一：
第二次加载页面的时候重复调用onCreateView(),重新实例化一个pageadapter对象导致fragment不显示。
解决方法：在onCreateView()方法加入如下代码,移除view

	if (view != null) {
            ViewGroup parent = (ViewGroup) view.getParent();
            if (parent != null) {
                parent.removeView(view);
            }
            return view;
        }

****