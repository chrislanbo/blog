---
title: listView嵌套viewPager需求
date: 2016-10-14 16:05:19
tags: [listview ,viewpager,事件冲突]
---

现在有了一个新的需求，要求listView上方加一个viewPager

考虑在listView内嵌套一个头布局
考虑在listView内设置多条目类型
考虑在Scrollview中的listview上方添加一布局

上述三种思路感觉第一个比较好做，先试试这个
listview中addHeaderView(...)方法可以在listview组件上方添加其他view，并且可以多次添加。
假设添加了两次其他类型的view，那么listview中position = 0 时对应的view就是第一次添加的，listview中position = 2 时对应的才是listview原先的第一条数据。（有空给张图）

>填坑第一步：addHeaderView和setAdapter的顺序问题
>>setAdapter需要放在addHeaderView之后，否则报错，因为绑定adapter的时候，回去判断是否有头布局，add之后的布局已经是listview的一部分，所以不能再数据准备好了之后再添加头布局

#难点
多view的嵌套会有事件的冲突，焦点在哪儿就成了一个大问题。
1. listview中若存在button等抢夺焦点的控件时，listview的onItemClick事件就不会被触发。解决办法，咋初始化item的时候屏蔽内部焦点的获取，在自定义item的根布局调用`setDescendantFoucsability(ViewGroup.FOUCS_BLOCK_DESCENDANTS)`这样就能阻止子控件获取焦点，并且button等控件的仍然可正常使用
2. 当listview需要添加【头布局】的时候有两个方法`addHeaderView(headview,null,false)`、`addHeaderView(headview)` 第三个参数为view是否可以被选中，默认是true
3. 由于添加的view 属于listview，所以可以相应onItemClick的事件，不想要的话，可以给添加的view设置点击事件来覆盖onItemClick的事件

