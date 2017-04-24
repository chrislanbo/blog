---
title: Vue样式sass
date: 2017-04-21 17:46:07
tags: [vue,sass,css]
categories:
---

感谢阮老师的文章

sass允许使用变量，变量用$开头

```css
$blue : #1875e7;　
　　div {
　　　color : $blue;
　　}
```
变量需要镶嵌在字符串之中，就必须需要写在#{}之中。

```css
$side : left;
　　.rounded {
　　　　border-#{$side}-radius: 5px;
　　}
```

SASS允许在代码中使用算式：

```css
body {
　　　　margin: (14px/2);
　　　　top: 50px + 100px;
　　　　right: $var * 10%;
　　}
```

SASS允许选择器嵌套

```css
   div h1 {
　　　　color : red;
　　}
　　
　　==>
　　
　　div {
　　　　hi {
　　　　　　color:red;
　　　　}
　　}
```

属性也可以嵌套，比如border-color属性

```css
   p {
　　　　border: {
　　　　　　color: red;
　　　　}
　　}
```
border后面必须加上冒号

嵌套的代码块内，可以使用&引用父元素

```css
 a {
　　　　&:hover { color: #ffb3ff; }
　　}
```

标准的CSS注释 /* comment */ ，会保留到编译后的文件。
单行注释 // comment，只保留在SASS源文件中，编译后被省略。
在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。

## SASS允许一个选择器，继承另一个选择器。

```css
   .class1 {
　　　　border: 1px solid #ddd;
　　}
　　.class2 {
　　　　@extend .class1;
　　　　font-size:120%;
　　}
```

## Mixin

类似宏定义，修饰的可重用的代码块

```css
    @mixin left {
　　　　float: left;
　　　　margin-left: 10px;
　　}
```

使用@include命令，调用这个mixin

```
div {
　　　　@include left;
　　}
```

mixin可指定参数和默认值

```css
@mixin left($value: 10px){
    float: left;
    margin-right: $value;
}
```

使用时，带参
```css
div {
    @include left(20px);
}
```

栗子：

```
@mixin rounded($vert, $horz, $radius: 10px){
       border-#{$vert}-#{$horz}-radius: $radius;
　　　　-moz-border-radius-#{$vert}#{$horz}: $radius;
　　　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
}


#navbar li { @include rounded(top, left); }
#footer { @include rounded(top, left, 5px); }

```

# 颜色函数

SASS 提供了内置颜色函数

```
    lighten(#cc3, 10%) // #d6d65c
　　darken(#cc3, 10%) // #a3a329
　　grayscale(#cc3) // #808080
　　complement(#cc3) // #33c

```


# 插入文件

@import 命令插入外部文件

```
　　@import "path/filename.scss";

```

如果插入的是.css文件，则等同于css的import命令。

```
@import "foo.css";
```

## 条件语句

@if可以用来判断

```
　　p {
　　　　@if 1 + 1 == 2 { border: 1px solid; }
　　　　@if 5 < 3 { border: 2px dotted; }
　　}
```

@else命令

```
　　@if lightness($color) > 30% {
　　　　background-color: #000;
　　} @else {
　　　　background-color: #fff;
　　}
```

## 循环语句

for循环

```
　　@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}
```

while循环

```
　　$i: 6;
　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}
```

each 命令 （类似for）

```
　　@each $member in a, b, c, d {
　　　　.#{$member} {
　　　　　　background-image: url("/image/#{$member}.jpg");
　　　　}
　　}
```


## 自定义函数

```
　　@function double($n) {
　　　　@return $n * 2;
　　}
　　#sidebar {
　　　　width: double(5px);
　　}
```

