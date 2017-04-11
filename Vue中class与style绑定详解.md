---
title: Vue中class与style绑定详解
date: 2017-04-11 15:31:00
tags: [vue,绑定]
---

vue中用v-bind处理元素的class列表和它内联样式(Style)之间的数据绑定。因为两者都是属性，所以可以表达式计算出最终字符串，或者手动拼接。针对class和style，表达式结果可以是"",[],{}。

>ps： `v-bind:`可以缩写成`:`

##绑定HTML Class
###对象
* 传给v-bind:class一个对象，动态切换class，
```html
<div v-bind:class="{ active: isActive }"></div>
```
上面的语法表示 class<active> 的更新将取决于数据属性 isActive 是否为真值 。

* 对象中传入更多属性用来动态切换多个 class，v-bind:class 指令可以与 class 属性共存
```html
<div class="static"
     :class="{ active: isActive, 'text-danger': hasError }">
</div>
```
当data如下：
```
data: {
  isActive: true,
  hasError: false
}
```
原来标签就渲染成：
```
<div class="static active"></div>
```

如果hasError变成true时候，class列表也会重新渲染
```
<div class="static active text-danger"></div>
```

等效直接绑定对象<classObject>,对象里面有对象属性
```html
<div v-bind:class="classObject"></div>
...

data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
或者绑定返回对象的计算属性
```
data:{
    isActive: true,
    error: null
},
computed:{
    classObject(){
        return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal',
        }
    }
}

```

###数组
* 传给v-bind:class一个数组，以应用class列表

```html
<div v-bind:class="[activeClass, errorClass]">
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染成==>
```html
<div class="active text-danger"></div>
```
数组中可使用三元表达式切换class列表
```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
```
始终添加 errorClass，仅仅当isActive为true是添加activeClass
可以在数组中使用对象语法达到同样效果
```
<div v-bind:class="[{ active: isActive }, errorClass]">
```

###组件上
当组件用到class属性时候，这些类会添加到根元素上，并且已经存在的类不会被覆盖
```html
//声明
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
然后使用的时候添加一些class
```html
<my-component class="baz boo" v-bind:class="{ active: isActive }"></my-component>
```
当isActive为true时，html最终将被渲染成：
```html
<p class="foo bar baz boo active">Hi</p>
```

##绑定内联样式
>CSS 属性名可以用驼峰式 camelCase 或短横分隔命名 kebab-case

###对象
v-bind:style 的对象语法与css类似，本质为JavaScript对象。
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}
```
直接绑定样式更清晰
```html
<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
//同样的，对象语法常常结合返回对象的计算属性使用
```

###数组语法

v-bind:style 的数组语法可以将多个样式对象应用到一个元素上：
```html
<div v-bind:style="[baseStyles, overridingStyles]">
```
>自动添加前缀
当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀。



