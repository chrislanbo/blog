---
title: Vue计算属性
date: 2017-04-12 17:19:38
tags: [vue,computed]
categories: [vue]
---

# 计算属性

模板内不适合放入太多逻辑，不方便维护。复杂逻辑应该使用计算属性
```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
==>

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    //声明一个计算属性 reversedMessage，函数将用作属性 this.reversedMessage 的 getter 。
    reversedMessage: function () {
      // this指向vm
      return this.message.split('').reverse().join('')
    }
  }
})
```
<!-- more -->
# 计算缓存和Methods
可以通过调用表达式中的 method 来达到同样的效果
```
<p>Reversed message: "{{ reversedMessage() }}"</p>
// in component
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
可以将同一函数定义为一个 method 而不是一个计算属性，其实结果都是一样。
区别是，
计算属性是基于它们的依赖进行缓存的,只有在它的相关依赖发生改变时才会重新求值
也就是说，method每次都会调用，而计算属性是有缓存的

# Computed属性和Watched属性
当你有一些数据需要随着其它数据变动而变动时，更好的想法是使用 computed 属性而不是命令式的 watch 回调
但是有的时候，当你想要在数据变化响应时，执行异步操作或开销较大的操作，需要Watched属性

```
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 question 发生改变，这个函数就会运行
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // _.debounce 是一个通过 lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问yesno.wtf/api的频率
    // ajax请求直到用户输入完毕才会发出
    // 学习更多关于 _.debounce function (and its cousin
    // _.throttle), 参考: https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```
在这个示例中，使用 watch 选项允许我们执行异步操作（访问一个 API），限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这是计算属性无法做到的。

# 计算setter

计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter 
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// 
```
现在在运行 this.fullName = 'John Doe' 时， setter 会被调用， this.firstName 和this.lastName 也相应地会被更新。

