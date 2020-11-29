---
title: v-show和v-if的思考
date: 2020-11-29 09:22:30
tags: vue
categiries: 进阶
---

## v-show和v-if
今天在写一个项目时，遇到了一个问题。使用了better-scroll来做页面滚动，首页进入后可以滚动，但是点击搜索框后出现搜索记录却无法滚动。查看scroll的打印发现 ```hasVerticalScroll: false```，并且scrollerHeight和clientHeight都是0，那就是都没有获得高度。最后把v-show改成v-if就可以了。
 - 问题出现原因：
众所周知，v-show渲染了dom，只是把dom的style设置为：display:none ，而使用了 display:none 的元素使用js是无法获得一些属性的值的,你可以通过  ```console.dir(dom)``` 获得dom的所有属性和方法，可以看到：
 - clientHeight: 0
 - clientLeft: 0
 - clientTop: 0
 - clientWidth: 0，
 - offsetHeight: 0
 - offsetLeft: 0
 - offsetTop: 0
 - offsetWidth: 0，
 - scrollHeight: 0
 - scrollLeft: 0
 - scrollTop: 0
 - scrollWidth: 0

而 v-if 是会在条件满足时进行渲染的，所以能够直接获得 dom 属性值，可以计算 wrapper 和 content 的高度， 当 ```wrapperHeight < contentHeight``` 时，就可以滚动了。