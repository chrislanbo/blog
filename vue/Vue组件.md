---
title: Vue组件
date: 2017-04-13 15:31:48
tags: [vue,data函数]
categories: [vue]
---

>组件可扩展html元素，封装重用代码

# 组件的使用
##注册
要注册一个全局组件，你可以使用 Vue.component(tagName, options)。 例如：
```
Vue.component('my-component', {
  // 选项
})
```

>W3C规则：小写，并且包含一个短杠


组件在注册之后，便可以在父实例的模块中以自定义元素 <my-component></my-component> 的形式使用。要确保在初始化根实例 之前 注册了组件：

```
<div id="example">
  <my-component></my-component>
</div>
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})
// 创建根实例
new Vue({
  el: '#example'
})
```
会渲染成===>

```
<div id="example">
  <div>A custom component!</div>
</div>
```
<!-- more -->
## 局部注册
不必在全局注册每个组件。通过使用组件实例选项注册，可以使组件仅在另一个实例/组件的作用域中可用：

```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```
>这种封装也适用于其它可注册的 Vue 功能，如指令。

## DOM模版

使用 DOM 作为模版时（例如，将 el 选项挂载到一个已存在的元素上）, 你会受到 HTML 的一些限制，因为 Vue 只有在浏览器解析和标准化 HTML 后才能获取模版内容。尤其像这些元素 
`<ul> ， <ol>， <table> ， <select> `限制了能被它包裹的元素，` <option> `只能出现在其它元素内部。
在自定义组件中使用这些受限制的元素时会导致一些问题，例如：

```
<table>
 <my-row>...</my-row>
</table>
```
自定义组件 <my-row> 被认为是无效的内容，因此在渲染的时候会导致错误。变通的方案是使用特殊的 is 属性：

```
<table>
 <tr is="my-row"></tr>
</table>
```

## data必须是函数

Vue构造器传入选项大多数都可以在组件使用。 data 是一个例外，它必须是函数。 错误做法：

```
Vue.component('my-component', {
  template: '<span>{{ message }}</span>',
  data: {
    message: 'hello'
  }
})
```
那么 Vue 会停止，并在控制台发出警告，告诉你在组件中 data 必须是一个函数。理解这种规则的存在意义很有帮助，让我们假设用如下方式来绕开Vue的警告：

```
<div id="example-2">
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
</div>
var data = { counter: 0 }
Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // 技术上 data 的确是一个函数了，因此 Vue 不会警告，
  // 但是我们返回给每个组件的实例的却引用了同一个data对象
  data: function () {
    return data
  }
})
new Vue({
  el: '#example-2'
})
```
由于这三个组件共享了同一个 data ， 因此增加一个 counter 会影响所有组件！这不对。我们可以通过为每个组件返回全新的 data 对象来解决这个问题：

```
data: function () {
  return {
    counter: 0
  }
}
```

## 组件通信
组件 A 在它的模版中使用了组件 B 。它们之间必然需要相互通信：父组件要给子组件传递数据，子组件需要将它内部发生的事情告知给父组件
 Vue.js 中，父子组件的关系可以总结为 props down, events up 。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。
 
![](https://cn.vuejs.org/images/props-events.png)

# Prop

## 使用props传递数据
组件实例相互独立，不能在子控件直接引用父控件数据，需要通过子组件的props选项
子组件要显式地用 props 选项声明它期待获得的数据：

```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
```
然后我们可以这样向它传入一个普通字符串：

```
<child message="hello!"></child>
```
## 驼峰式和短横线隔开式
HTML 特性是不区分大小写的。所以，当使用的不是字符串模版，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名：

```
Vue.component('child', {
  // camelCase in JavaScript
  props: ['myMessage'],
  template: '<span>{{ myMessage }}</span>'
})
<!-- kebab-case in HTML -->
<child my-message="hello!"></child>
```
如果你使用字符串模版，则没有这些限制。尽量尊重w3c标准写代码

## 动态props
在模板中，要动态地绑定父组件的数据到子模板的props，与绑定到任何普通的HTML特性相类似，就是用 v-bind。每当父组件的数据变化时，该变化也会传导给子组件：

```
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
  //使用 v-bind 的缩写语法 :<child :my-message="parentMsg"></child>
</div>

```

## 字面量语法 和 动态语法
初学者常犯的一个错误是使用字面量语法传递数值：

```
<!-- 传递了一个字符串"1" -->
<comp some-prop="1"></comp>
```
因为它是一个字面 prop ，它的值是字符串 "1" 而不是number。如果想传递一个实际的number，需要使用 v-bind ，从而让它的值被当作 JavaScript 表达式计算：

```
<!-- 传递实际的mumber -->
<comp v-bind:some-prop="1"></comp>
```

## props是单向数据流
prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态

>注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop 。如果你这么做了，Vue 会在控制台给出警告。

prop作为初始值传入后，单独定义一个局部变量,而不是直接操作prop

```
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```
prop作为初始值传入后，需要处理数据并返回，需要定义一个计算属性，处理prop

```
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```
## prop验证

props提供了验证规格，传入数据不符合规格，就会警告

要指定验证规格，需要用对象的形式，而不能用字符串数组
type 可以是下面原生构造器：

    String
    Number
    Boolean
    Function
    Object
    Array
  
type 也可以是一个自定义构造器函数，使用 instanceof 检测。
  
```
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```



# 自定义事件
父组件是使用 props 传递数据给子组件，子组件要把数据传递回去，就需要自定义事件

## 使用v-on 绑定自定义事件
每个 Vue 实例都实现了事件接口(Events interface)，即：

* 使用 $on(eventName) 监听事件
* 使用 $emit(eventName) 触发事件

父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。
不能用$on侦听子组件抛出的事件，而必须在模板里直接用v-on绑定

```
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>
Vue.component('button-counter', {
  template: '<button v-on:click="increment">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    increment: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```

## 给组件绑定原生事件

有时候，你可能想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on 。例如：

```
<my-component v-on:click.native="doTheThing"></my-component>
```

## 使用自定义事件的表单输入组件
自定义事件可以用来创建自定义的表单输入组件，使用 v-model 来进行数据双向绑定：

```
<input v-model="something">
```

这不过是以下示例的语法糖（效果等效）：

```
<input v-bind:value="something" v-on:input="something = $event.target.value">
```
所以在组件中使用时，它相当于下面的简写：

```
<custom-input v-bind:value="something" v-on:input="something = arguments[0]"></custom-input>
```

组件的v-model生效条件：
接受一个 value 属性
在有新的 value 时触发 input 事件


```
<currency-input v-model="price"></currency-input>

//...

Vue.component('currency-input', {
  template: '\
    <span>\
      $\
      <input\
        ref="input"\
        v-bind:value="value"\
        v-on:input="updateValue($event.target.value)"\
      >\
    </span>\
  ',
  props: ['value'],
  methods: {
    // 不是直接更新值，而是使用此方法来对输入值进行格式化和位数限制
    updateValue: function (value) {
      var formattedValue = value
        // 删除两侧的空格符
        .trim()
        // 保留 2 小数位
        .slice(0, value.indexOf('.') + 3)
      // 如果值不统一，手动覆盖以保持一致
      if (formattedValue !== value) {
        this.$refs.input.value = formattedValue
      }
      // 通过 input 事件发出数值
      this.$emit('input', Number(formattedValue))
    }
  }
})
```

## 非父子组件通信
可以使用一个空的 Vue 实例作为中央事件总线：

```
var bus = new Vue()
// 触发组件 A 中的事件
bus.$emit('id-selected', 1)
// 在组件 B 创建的钩子中监听事件
bus.$on('id-selected', function (id) {
  // ...
})
```
有现成的状态管理模式


# 使用slot分发内容
通常情况下会有如下组合

```
<app>
  <app-header></app-header>
  <app-footer></app-footer>
</app>
```
注意两点：
app 组件不知道它的挂载点会有什么内容。挂载点的内容是由<app>的父组件决定的。
app 组件很可能有它自己的模版。

为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为 内容分发 。Vue.js 实现了一个内容分发 API ，参照了当前 Web 组件规范草案，使用特殊的 <slot> 元素作为原始内容的插槽。

## 编译作用域
父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译
假定模板为：

```
<child-component>
  {{ message }}
</child-component>
```
message 是绑定到父组件的数据。

一个常见错误是：在父组件模板内将一个指令绑定到子组件的属性/方法：

```
<!-- 无效 -->
<child-component v-show="someChildProperty"></child-component>
```
假定 someChildProperty 是子组件的属性，上例不会如预期那样工作。父组件模板不知道子组件的状态。

如果要绑定作用域内的指令到一个组件的根节点，你应当在组件自己的模板上做：

```
Vue.component('child-component', {
  // 有效，因为是在正确的作用域内
  template: '<div v-show="someChildProperty">Child</div>',
  data: function () {
    return {
      someChildProperty: true
    }
  }
})
```

## 单个slot
除非子组件模板包含至少一个 <slot> 插口，否则父组件的内容将会被丢弃。当子组件模板只有一个没有属性的 slot 时，父组件整个内容片段将插入到 slot 所在的 DOM 位置，并替换掉 slot 标签本身。
最初在 <slot> 标签中的任何内容都被视为备用内容。备用内容在子组件的作用域内编译，并且只有在宿主元素为空，且没有要插入的内容时才显示备用内容。
假定 my-component 组件有下面模板：

```
<div>
  <h2>我是子组件的标题</h2>
  <slot>
    只有在没有要分发的内容时才会显示。
  </slot>
</div>
```
父组件模版：

```
<div>
  <h1>我是父组件的标题</h1>
  <my-component>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </my-component>
</div>
```
渲染结果：

```
<div>
  <h1>我是父组件的标题</h1>
  <div>
    <h2>我是子组件的标题</h2>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </div>
</div>
```

## 具名slot

<slot> 元素可以用一个特殊的属性 name 来配置如何分发内容。多个 slot 可以有不同的名字。具名 slot 将匹配内容片段中有对应 slot 特性的元素。
仍然可以有一个匿名 slot ，它是默认 slot ，作为找不到匹配的内容片段的备用插槽。如果没有默认的 slot ，这些找不到匹配的内容片段将被抛弃。
例如，假定我们有一个 app-layout 组件，它的模板为：

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

父组件模版：

```
<app-layout>
  <h1 slot="header">这里可能是一个页面标题</h1>
  <p>主要内容的一个段落。</p>
  <p>另一个主要段落。</p>
  <p slot="footer">这里有一些联系信息</p>
</app-layout>
```

渲染结果为：

```
<div class="container">
  <header>
    <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
  </main>
  <footer>
    <p>这里有一些联系信息</p>
  </footer>
</div>
```
在组合组件时，内容分发 API 是非常有用的机制。

## 作用域slot
作用域插槽是一种特殊类型的插槽，用作使用一个（能够传递数据到）可重用模板替换已渲染元素。
在子组件中，只需将数据传递到插槽，就像你将 prop 传递给组件一样：

```
<div class="child">
  <slot text="hello from child"></slot>
</div>
```
在父级中，具有特殊属性 scope 的 <template> 元素，表示它是作用域插槽的模板。scope 的值对应一个临时变量名，此变量接收从子组件中传递的 prop 对象：

```
<div class="parent">
  <child>
    <template scope="props">
      <span>hello from parent</span>
      <span>{{ props.text }}</span>
    </template>
  </child>
</div>
```
如果我们渲染以上结果，得到的输出会是：

```
<div class="parent">
  <div class="child">
    <span>hello from parent</span>
    <span>hello from child</span>
  </div>
</div>
```
作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：

```
<my-awesome-list :items="items">
  <!-- 作用域插槽也可以是具名的 -->
  <template slot="item" scope="props">
    <li class="my-fancy-item">{{ props.text }}</li>
  </template>
</my-awesome-list>

```
列表组件的模板：

```
<ul>
  <slot name="item"
    v-for="item in items"
    :text="item.text">
    <!-- 这里写入备用内容 -->
  </slot>
</ul>
```

# 动态组件
通过使用保留的 <component> 元素，动态地绑定到它的 is 特性，我们让多个组件可以使用同一个挂载点，并动态切换：

```
var vm = new Vue({
  el: '#example',
  data: {
    currentView: 'home'
  },
  components: {
    home: { /* ... */ },
    posts: { /* ... */ },
    archive: { /* ... */ }
  }
})
<component v-bind:is="currentView">
  <!-- 组件在 vm.currentview 变化时改变！ -->
</component>
```
也可以直接绑定到组件对象上：

```
var Home = {
  template: '<p>Welcome home!</p>'
}
var vm = new Vue({
  el: '#example',
  data: {
    currentView: Home
  }
})
```
## keep-alive指令
如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 keep-alive 指令参数：
<keep-alive>
  <component :is="currentView">
    <!-- 非活动组件将被缓存！ -->
  </component>
</keep-alive>

# 编写可复用组件
在编写组件时，记住是否要复用组件有好处。一次性组件跟其它组件紧密耦合没关系，但是可复用组件应当定义一个清晰的公开接口。
Vue 组件的 API 来自三部分 - props, events 和 slots ：
Props 允许外部环境传递数据给组件
Events 允许组件触发外部环境的副作用
Slots 允许外部环境将额外的内容组合在组件中。
使用 v-bind 和 v-on 的简写语法，模板的缩进清楚且简洁：

```
<my-component
  :foo="baz"
  :bar="qux"
  @event-a="doThis"
  @event-b="doThat"
>
  <img slot="icon" src="...">
  <p slot="main-text">Hello!</p>
</my-component>
```
# 子组件索引

尽管有 props 和 events ，但是有时仍然需要在 JavaScript 中直接访问子组件。为此可以使用 ref 为子组件指定一个索引 ID 。例如：

```
<div id="parent">
  <user-profile ref="profile"></user-profile>
</div>
var parent = new Vue({ el: '#parent' })
// 访问子组件
var child = parent.$refs.profile
```
当 ref 和 v-for 一起使用时， ref 是一个数组或对象，包含相应的子组件。
$refs 只在组件渲染完成后才填充，并且它是非响应式的。它仅仅作为一个直接访问子组件的应急方案——应当避免在模版或计算属性中使用 $refs 。

# 异步组件

在大型应用中，我们可能需要将应用拆分为多个小模块，按需从服务器下载。为了让事情更简单， Vue.js 允许将组件定义为一个工厂函数，动态地解析组件的定义。Vue.js 只在组件需要渲染时触发工厂函数，并且把结果缓存起来，用于后面的再次渲染。例如：
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // Pass the component definition to the resolve callback
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
工厂函数接收一个 resolve 回调，在收到从服务器下载的组件定义时调用。也可以调用 reject(reason) 指示加载失败。这里 setTimeout 只是为了演示。怎么获取组件完全由你决定。推荐配合使用 ：Webpack 的代码分割功能：

```
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 require 语法告诉 webpack
  // 自动将编译后的代码分割成不同的块，
  // 这些块将通过 Ajax 请求自动下载。
  require(['./my-async-component'], resolve)
})
```
你可以使用 Webpack 2 + ES2015 的语法返回一个 Promise resolve 函数：

```
Vue.component(
  'async-webpack-example',
  () => System.import('./my-async-component')
)
```
如果你是 Browserify 用户,可能就无法使用异步组件了,它的作者已经表明 Browserify 是不支持异步加载的。Browserify 社区发现 一些解决方法，可能有助于已存在的复杂应用。对于其他场景，我们推荐简单实用 Webpack 构建，一流的异步支持

# 组件命名约定

当注册组件（或者 props）时，可以使用 kebab-case ，camelCase ，或 TitleCase 。Vue 不关心这个。

```
// 在组件定义中
components: {
  // 使用 kebab-case 形式注册
  'kebab-cased-component': { /* ... */ },
  // register using camelCase
  'camelCasedComponent': { /* ... */ },
  // register using TitleCase
  'TitleCasedComponent': { /* ... */ }
}
```
在 HTML 模版中，请使用 kebab-case 形式：

```
<!-- 在HTML模版中始终使用 kebab-case -->
<kebab-cased-component></kebab-cased-component>
<camel-cased-component></camel-cased-component>
<title-cased-component></title-cased-component>

```
当使用字符串模式时，可以不受 HTML 的 case-insensitive 限制。这意味实际上在模版中，你可以使用 camelCase 、 TitleCase 或者 kebab-case 来引用：


```
<!-- 在字符串模版中可以用任何你喜欢的方式! -->
<my-component></my-component>
<myComponent></myComponent>
<MyComponent></MyComponent>
```
如果组件未经 slot 元素传递内容，你甚至可以在组件名后使用 / 使其自闭合：
```
<my-component/>
```
当然，这只在字符串模版中有效。因为自闭的自定义元素是无效的 HTML ，浏览器原生的解析器也无法识别它。
# 递归组件

组件在它的模板内可以递归地调用自己，不过，只有当它有 name 选项时才可以：
    
    name: 'unique-name-of-my-component'
当你利用Vue.component全局注册了一个组件, 全局的ID作为组件的 name 选项，被自动设置.

```
Vue.component('unique-name-of-my-component', {
  // ...
})
```
如果你不谨慎, 递归组件可能导致死循环:
```
name: 'stack-overflow',
template: '<div><stack-overflow></stack-overflow></div>'
```
上面组件会导致一个错误 “max stack size exceeded” ，所以要确保递归调用有终止条件 (比如递归调用时使用 v-if 并让他最终返回 false )。
# 组件间的循环引用

假设你正在构建一个文件目录树，像在Finder或文件资源管理器中。你可能有一个 tree-folder组件:
```
<p>
  <span>{{ folder.name }}</span>
  <tree-folder-contents :children="folder.children"/>
</p>

```
然后 一个tree-folder-contents组件：
```
<ul>
  <li v-for="child in children">
    <tree-folder v-if="child.children" :folder="child"/>
    <span v-else>{{ child.name }}</span>
  </li>
</ul>
```
When you look closely, you’ll see that these components will actually be each other’s descendent and ancestor in the render tree - a paradox! When registering components globally with Vue.component, this paradox is resolved for you automatically. If that’s you, you can stop reading here.
当你仔细看时，会发现在渲染树上这两个组件同时为对方的父节点和子节点–这点是矛盾的。当使用Vue.component将这两个组件注册为全局组件的时候，框架会自动为你解决这个矛盾，如果你是这样做的，就不用继续往下看了。
然而，如果你使用诸如Webpack或者Browserify之类的模块化管理工具来requiring/importing组件的话，就会报错了：
Failed to mount component: template or render function not defined.
为了解释为什么会报错，简单的将上面两个组件称为 A 和 B ，模块系统看到它需要 A ，但是首先 A 需要 B ，但是 B 需要 A， 而 A 需要 B，陷入了一个无限循环，因此不知道到底应该先解决哪个。要解决这个问题，我们需要在其中一个组件中（比如 A ）告诉模块化管理系统，“A 虽然需要 B ，但是不需要优先导入 B”
在我们的例子中，我们选择在tree-folder 组件中来告诉模块化管理系统循环引用的组件间的处理优先级，我们知道引起矛盾的子组件是tree-folder-contents，所以我们在beforeCreate 生命周期钩子中去注册它：
beforeCreate: function () {
  this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue')
}
问题解决了。

# 内联模版

如果子组件有 inline-template 特性，组件将把它的内容当作它的模板，而不是把它当作分发内容。这让模板更灵活。
```
<my-component inline-template>
  <div>
    <p>These are compiled as the component's own template.</p>
    <p>Not parent's transclusion content.</p>
  </div>
</my-component>
```
但是 inline-template 让模板的作用域难以理解。最佳实践是使用 template 选项在组件内定义模板或者在 .vue 文件中使用 template 元素。
# X-Templates

另一种定义模版的方式是在 JavaScript 标签里使用 text/x-template 类型，并且指定一个id。例如：
```
<script type="text/x-template" id="hello-world-template">
  <p>Hello hello hello</p>
</script>
Vue.component('hello-world', {
  template: '#hello-world-template'
})
```
这在有很多模版或者小的应用中有用，否则应该避免使用，因为它将模版和组件的其他定义隔离了。
# 对低开销的静态组件使用 v-once

尽管在 Vue 中渲染 HTML 很快，不过当组件中包含大量静态内容时，可以考虑使用 v-once 将渲染结果缓存起来，就像这样：
```
Vue.component('terms-of-service', {
  template: '\
    <div v-once>\
      <h1>Terms of Service</h1>\
      ... a lot of static content ...\
    </div>\
  '
})
```

