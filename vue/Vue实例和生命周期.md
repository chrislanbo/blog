---
title: Vue实例和生命周期
date: 2017-04-13 09:57:13
tags: [vue]
categories: [vue]
---

# Vue 构造器

每个Vue应用都是通过构造函数Vue创建一个实例启动的
```
var vm = new Vue({
  //...
})
```
而组件其实就是被扩展的Vue实例
```
var MyComponent = Vue.extend({
  // 扩展选项
})
// 所有的 `MyComponent` 实例都将以预定义的扩展选项被创建
var myComponentInstance = new MyComponent()
```

# 属性和方法
每个Vue实例都会代理 data对象里的所有属性

```
var vm = new Vue({
    data:{
        a:1
    }
})

vm.a === data.a //true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // -> 2
// ... 反之亦然
data.a = 3
vm.a // -> 3
```

>如果在实例创建之后添加新的属性到实例上，它不会触发视图更新

### 除了 data 属性，也暴露了很多实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。

```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
  // 这个回调将在 `vm.a`  改变后调用
})
```

>注意，不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。


# Vue 生命周期
created
mounted
updated
destroyed
![](https://cn.vuejs.org/images/lifecycle.png)


