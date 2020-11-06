---
title: vue中v-if
date: 2020-09-24 09:47:05
tags: vue
categories: 进阶
---

## v-if
当在input框中使用v-if时，如果输入了字符后，改变v-if的显示时，input中的内容不会变化，这是因为vue会尽量复用页面的内容，增加渲染效率，这时可以为每个input增加一个key值，这样vue发现key值不同便不会复用了。

## v-for
使用v-for时，如果不想在外层添加一个div标签，可以使用template作为模版占位符，这个不会在页面中渲染。

## $set
向对象增加属性值时，一种方法是整个改变对象的引用，另一种是通过vue实例.$set(对象，新属性名，新属性值)，这样才能实时更新数据。Vue.set()或vue实例.$set()。
向数组改变数据时，一：通过vue提供的7个变异方法，二：通过改变数组引用，三：通过vue实例.$set(数组，愿属性的index，新属性)。

## click事件修饰符
.prevent阻止默认行为
.stop阻止向上冒泡
.self只有点击自身才触发时间，冒泡的不行，click事件只有e.target = e.currentTaget时才会执行
.once 只绑定一次，只能触发一次
.capture 事件捕获规则，从外部向内部触发，不是事件冒泡了

## 按键修饰符
.enter ...
## 系统修饰符
.ctrl ...
## 鼠标修饰符
.right 右键

## transition
过渡的元素只能是以下之一：
- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点

自定义过渡动画的，记得在css中修改以下的类名：
- 动画名-enter 进入前效果
- 动画名-enter-active 进入的过渡时间和函数
- 动画名-enter-to 进入后效果
- 动画名-leave 离开前效果
- 动画名-leave-active 离开的过渡时间和函数
- 动画名-leave-to 离开后效果