---
title: Vue列表渲染
date: 2017-04-12 16:02:02
tags: [vue,v-for]
categories: [vue]
---

# 数组迭代v-for
使用v-for指令，可以根据一组数组的选项列表进行渲染。
<语法> item in items  
可以用 of 替代 in 作为分隔符
item 单个元素的自定义别名
items 源数据数组
```html
<ul id="test">
  <li v-for="item in items">
    {{ item.msg }}
  </li>
</ul>
var test1 = new Vue({
  el: '#test',
  data: {
    items: [
      {msg: 'Foo' },
      {msg: 'Bar' }
    ]
  }
})
```

v-for中有父作用域属性的访问权，并且支持另一个可选参数为索引
```
<ul id="test2">
<li v-for="(item,index) in items">
{{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>

new Vue({
  el: '#test2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
<!-- more -->

# Template v-for

如同 v-if 模板，你也可以用带有 v-for 的 <template> 标签来渲染多个元素块。例如：
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider"></li>
  </template>
</ul>
```

# 对象迭代v-for
也可以迭代一个对象的属性
```
<ul id="Object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
new Vue({
  el: '#Object',
  data: {
    object: {
      FirstName: 'John',
      LastName: 'Doe',
      Age: 30
    }
  }
})
```
作为参数，第一个为值，第二个为键(可选)，第三个为索引(可选)
```
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }} : {{ value }}
</div>
```

# 整数迭代

当为整数的时候，相当与重复次数

```
<span v-for="n in 10">{{ n }}</span>
```

# 组件中的v-for
组件中同样可以使用v-for指令
但是因为组件有自己独立的作用域，所以不会自动传递数据到组件。
需要使用props
```
<my-component
  v-for="(item, index) in items"
  :item="item"
  :index="index">
</my-component>
```

> 不自动传递item 到组件，设计目的为了让组件更好的重用


todo-list完整示例

```html
<div id="todo-list-example">
  <input
    v-model="newTodoText"
    v-on:keyup.enter="addNewTodo"//回车的时候调用addNewTodo()方法
    placeholder="添加一个TODO"//hint显示的文字
  >
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:title="todo" // 绑定数据，传递数组中取出的字符
      v-on:remove="todos.splice(index, 1)">
    </li>
  </ul>
</div>
```


```js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">X</button>\
    </li>\
  ',
  props: ['title']// 使用props传递绑定的数据，让template中可以直接调用
})
new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',//声明一个变量
    todos: [        //声明一个数组，添加TODO列表内容
      'Do the dishes',
      'Take out the trash',
      'Mow the lawn'
    ]
  },
  methods: {
    addNewTodo(){
      this.todos.push(this.newTodoText)//向数组中添加一个
      this.newTodoText = ''//清除上次输入
    }
  }
})
```

# key
v-for 默认需要key来识别节点，理想的 key 值是每项都有唯一 id。
```
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```

#  数组的更新

## 变异方法
下面的方法 将会触发视图更新，会改变源数组

    push()
    pop()
    shift()
    unshift()
    splice()
    sort()
    reverse()
    
## 非变异数组
下面这些方法不会改变原始数组，但总是返回一个新数组
栗子
```
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

# 过滤、排序
当需要显示一个过滤、排序后的数组，而不改变原数组

排序数组的计算属性，在计算属性中进行过滤输出
```
<li v-for="n in evenNumbers">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
创建返回过滤,将过滤好的对象返回
```
<li v-for="n in even(numbers)">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

# JavaScript 的限制
Vue 不能检测以下变动的数组

1. 根据索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
2. 修改数组的长度时，例如： vm.items.length = newLength

为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果， 同时也将触发状态更新：
```java
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice`
example1.items.splice(indexOfItem, 1, newValue)

```
为了解决第二类问题，你也同样可以使用 splice：
```java
example1.items.splice(newLength)
```


