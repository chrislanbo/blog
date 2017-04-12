---
title: Vue条件渲染详解
date: 2017-04-11 17:46:28
tags: [vue,key]
---

### v-if、v-else、v-else-if
```html
<div v-if="Math.random() > 0.5">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

### template 中 v-if 
template可以当成一个包装元素，一旦这样，便只会渲染标签内的元素


#### 用key管理复用元素
由于vue会尽可能的复用元素
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```
上述代码切换loginType的时候，不清除输入内容。<input>不会被替换，仅仅替换了placeholder。如果想切换的时候清除内容，就需要给<input>添加key值，来声明这两个元素为独立元素，不需复用
```
...
  <input placeholder="Enter your username" key="username-input">
...
  <input placeholder="Enter your email address" key="email-input">
...
```
###v-show
类似v-if，带有 `v-show `的元素始终会被渲染并保留在 DOM 中。`v-show` 是简单地切换元素的 CSS 属性 display 。
注意， v-show 不支持 <template> 语法，也不支持 v-else。


v-if和v-show的区别
v-if
1. 条件为false就不会渲染，true才会渲染
2. 条件内的会被对应销毁和重建

v-show
1. 不论如何都会渲染，只是简单地基于 CSS 进行切换。

> 针对上述总结，条件频繁改变的用v-show，条件不经常改变的用v-if


>当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。


