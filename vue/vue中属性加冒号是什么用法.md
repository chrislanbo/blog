---
title: vue中属性前加冒号、vue中:class的意义
date: 2017-04-11 15:12:11
tags: [vue]
---

vue中的属性前一个冒号，类似:class 或者 :style 其实都是省略了v-bind,应该是v-bind:class和v-bind:style

vue会自动给单独的一个`:`加上前缀`v-bind`

