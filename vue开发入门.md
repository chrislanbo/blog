---
title: vue开发入门
date: 2017-03-15 16:16:13
tags: [js]
---

>前戏：
1. node.js安装
2. vue-cli 构建工具
3. 使用淘宝镜像（包管理器代替npm）[淘宝镜像](https://npm.taobao.org/ "淘宝镜像")、




1. node.js安装，官网下载安装包一路确定
2. `npm install -g cnpm --registry=http://registry.npm.taobao.org`
 `npm config set registry http://registry.npm.taobao.org/`
发布自己的包的时候却需要将源修改回官方的
`npm config set registry https://registry.npmjs.org/`
3. `npm install -g vue-cli`  

下载的一些东西会在这里纠缠` C:\Users\Administrator\AppData\Roaming\npm\node_modules\vue-cli\`

4. 进入自己制定的目录执行`vue init webpack vueDemo`创建第一个vue项目，然后要依次确认几个问题

        project name (***) //直接回车
        project description // 项目详情，不写就直接回车
        Author //回车
        install vue-router yes //安装路由
        use an eslint preset standard //js代码标准检查
        剩下两个都no
    
    
5. cd 到 vueDemo 执行`npm install` 下载项目需要的依赖包到`node_modules`,之前用`cnpm install`下载总是不全（其实是在windows上默认有很多前缀，导致无法识别）
6. `npm run dev`运行项目，对应package.json中的js文件
7. 在浏览器中输入`localhost:8080`,查看网页
8. 

To use this template, you must update following to modules:

  npm: 2.15.9 should be >= 3.0.0
单独升级 npm
npm -g install npm@3.10.8



    src/
        assets/                         ---> 静态资源文件
        components/                     ---> 组件文件
        config/                         ---> 项目配置文件
        filters/                        ---> 过滤器
        router/                         ---> vue-router配置文件
        store/                          
            actions/                    ---> vuex actions
            getters/                    ---> vuex getters
            modules/                    ---> vuex modules
            index.js                    ---> vuex store
            plugins.js                  ---> vuex plugins
        utils/                          ---> 共用函数
        vendor/                         ---> ThirdParty lib
        views/                          ---> 展示视图库
        main.js                         ---> 入口文件
    .babelrc                            ---> babel 配置文件es6 ->es5
    .eslintrc                           ---> eslint 配置文件
    .gitignore                          ---> gitignore
    index.html                          ---> index
    package.json                        ---> package
    README.md                           ---> 本文件
    webpack.config.common.js            ---> webpack通用配置
    webpack.config.dev.js               ---> webpack开发配置

打开 工程目录下的 App.vue

template 写 html，script写 js，style写样式

一个组件下只能有一个并列的 div

数据要return 出去 像 react 一样

	所有组件.vue名 都统一 《短横线》 命名 we-compnent
	css内下划线( _ )开始的为通用类
	js中内下划线( _ )开头的为私有属性
	所有events均使用短横线 命名
	所有组件(.vue)里template标签包含的元素必须是component-xx 开头
	所有state统一下划线 命名
	所有action统一下划线命名


vue中两个参数用|隔开是什么意思
```
{{a|b}}

```
其实这是vueJs中的过滤器,一个为定义的id,去根据逻辑操作另外一个数据
```
Vue.filter(id,[definition])
- {string} id
- {Function} [definition]
```
usage:
注册或获取全局过滤器。
```html
// 注册
Vue.filter('my-filter', function (value) {
  // 返回处理后的值
})
// getter，返回已注册的过滤器
var myFilter = Vue.filter('my-filter')
```
------

`=>`是es6语法中的`arrow function`
```html
举例：
(x) => x + 6

相当于

function(x){
    return x + 6;
}
```
```xml
发现了<style>标签中有rel="stylesheet/scss"和type="text/css"时能正确识别sass语言。如：

`<style scoped lang="sass" rel="stylesheet/scss" type="text/css">`
```

```
npm install --save node-sass --registry=https://registry.npm.taobao.org  --sass-binary-site=http://npm.taobao.org/mirrors/node-sass


--disturl=https://npm.taobao.org/dist

```


vue-resource 和后端做ajax通信
webpack 构建工具，将代码编译成浏览器可识别的语句

vue 特点 数据驱动dom改变

vue-cli vue基础代码





