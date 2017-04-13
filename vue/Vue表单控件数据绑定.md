---
title: Vue表单控件数据绑定
date: 2017-04-13 14:55:52
tags: [vue,v-model]
categories: [vue]
---

> v-model指令在表单控件上创建双向数据绑定

v-model 会根据控件类型自动选取正确的方法更新元素，其实也就是监听输入事件来更新数据。

v-model 不关心控件的初始值，只关心Vue实例数据

# 文本
单行文本
```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

多行文本
```
<span>Multiline message is:</span>
<p style="white-space: pre">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

# 复选框

一个勾选框,逻辑值：
```
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
多勾选框，绑定到同一个数组：
```
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```
<!-- more -->
# 单选按钮

```
<div id="example-4" class="demo">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```

# 选择列表

单选列表:
```
<div id="example-5" class="demo">
  <select v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
new Vue({
  el: '#example-5',
  data: {
    selected: null
  }
})
```

多选列表（绑定到一个数组）：
```
<div id="example-6" class="demo">
  <select v-model="selected" multiple style="width: 50px">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```


动态选项，用 v-for 渲染：
```
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

# 绑定value
对于单选按钮，勾选框及选择列表选项， v-model 绑定的 value 通常是静态字符串（对于勾选框是逻辑值）
```
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">
<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">
<!-- 当选中时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```
>如需绑定 value 到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，并且这个属性的值可以不是字符串。

## 复选框
```
<input
  type="checkbox"
  v-model="toggle"
  v-bind:true-value="a"
  v-bind:false-value="b"
>
// 当选中时
vm.toggle === vm.a
// 当没有选中时
vm.toggle === vm.b
```

## 单选按钮
```
<input type="radio" v-model="pick" v-bind:value="a">
// 当选中时
vm.pick === vm.a
```

## 选择列表设置

```
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
// 当选中时
typeof vm.selected // -> 'object'
vm.selected.number // -> 123
```

# v-model的修饰符

## .lazy
在默认情况下， v-model 在 input 事件中同步输入框的值与数据 (除了 上述 IME 部分)，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：
<!-- 在 "change" 而不是 "input" 事件中更新 -->

    <input v-model.lazy="msg" >

## .number
如果想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值：

    <input v-model.number="age" type="number">
这通常很有用，因为在 type="number" 时 HTML 中输入的值也总是会返回字符串类型。

## .trim
如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：
    
    <input v-model.trim="msg">

