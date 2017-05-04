---
title: Vue无限滑动的事件冲突
date: 2017-04-18 14:51:15
tags: [vue,无限滑动]
categories: [vue]
---

在研究musi-ui框架的时候，发现一个滑动冲突
```html
<template>
    <div class="demo-refresh-container">
      <!--v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="100"-->
     <mu-refresh-control :refreshing="refreshing" :trigger="trigger" @refresh="refresh"/>
      <!--<div class="list" v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="100">-->
      <mu-list>
        <template v-for="item in list">
          <mu-list-item disableRipple :title="item"/>
          <mu-divider/>
        </template>
      </mu-list>
      <mu-infinite-scroll  class="infinite-scroll" :scroller="scroller" :loading="loading" @load="loadMore"/>
    </div>

</template>
```

如果在如上代码外面添加一对div标签,那么下拉刷新还是可以正常使用，但无限下滑就出现问题。

但是页面除了内容还会有菜单和标题，所以肯定会出现嵌套问题

经过试验发现，只要把菜单和其他项目抽取到其他文件，让滑动标签外没有额外的标签，就不会出现问题


