---
title: Vue模板语法
date: 2017-04-12 17:47:12
tags: [vue,数据绑定,缩写,过滤器]
---

# 数据绑定

>绝不要对用户提供的内容插值。

数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值
```
<span>Message: {{ msg }}</span>
```
绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。
通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定：
<span v-once>This will never change: {{ msg }}</span>

```
{{}} 不能在HTML属性中使用，需要使用v-bind指令绑定数据

<div v-bind:id="msg"></div>
```
这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
```
<button v-bind:disabled="someDynamicCondition">Button</button>
```

v-model也可以绑定数据，但是他是用在表单控件上的，用于实现双向数据绑定，所以如果你用在除了表单控件以外的标签是没有任何效果的。

下面单个表达式都是被允许的数据绑定
```
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```
语句和流控制是不会生效的

<!-- more -->
# 纯html
使用v-html 指令
```
<div v-html="rawHtml"></div>
```
被插入的内容都会被当做 HTML —— 数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担任 UI 重用与复合的基本单元。

# 指令
带有 v- 前缀的特殊属性。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。
```
<p v-if="seen">Now you see me</p>

```
这里， v-if 指令将根据表达式 seen 的值的真假来移除/插入 <p> 元素。

v-bind 指令被用来响应地更新 HTML 属性：
```
<a v-bind:href="url"></a>
```
href 是参数，v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。


如下的v-on 指令，它用于监听 DOM 事件：
```
<a v-on:click="doSomething">
```
在这里参数是监听的事件名


## 过滤器
过滤器可以用在两个地方：mustache 插值和 v-bind 表达式
```
<!-- in mustaches -->
{{ message | capitalize }}
<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```
过滤器函数总接受表达式的值作为第一个参数。
```
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```
过滤器可以串联：
```
{{ message | filterA | filterB }}
```
过滤器是 JavaScript 函数，因此可以接受参数：
```
{{ message | filterA('arg1', arg2) }}
```
这里，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。


## v-bind 缩写
```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```
## v-on 缩写
```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

