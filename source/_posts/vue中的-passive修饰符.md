---
title: vue中的.passive修饰符
date: 2020-11-07 14:45:16
tags: vue
categories: 进阶
---

## vue中的 .passive 修饰符
Vue提供了事件绑定的语法糖，我们在标签中可直接使用形如v-on:click，@click，@focus的形式绑定事件监听器，并且可以使用修饰符对事件进行option设置。本次解释下prevent 与passive的修饰符。prevent 是拦截默认事件，passive是不拦截默认事件。

### prevent
某些标签拥有自身的默认事件，如a[href="#"]，button[type="submit"] 这种标签在冒泡结束后会开始执行默认事件。注意默认事件虽然是冒泡后开始，但不会因为stop阻止事件传递而停止。


### passive
passive这个修饰符会执行默认方法。你们可能会问，明明默认执行为什么会设置这样一个修饰符。这就要说一下这个修饰符的本意了。

【浏览器只有等内核线程执行到事件监听器对应的JavaScript代码时，才能知道内部是否会调用preventDefault函数来阻止事件的默认行为，所以浏览器本身是没有办法对这种场景进行优化的。这种场景下，用户的手势事件无法快速产生，会导致页面无法快速执行滑动逻辑，从而让用户感觉到页面卡顿。】

通俗点说就是每次事件产生，浏览器都会去查询一下是否有preventDefault阻止该次事件的默认动作。我们加上passive就是为了告诉浏览器，不用查询了，我们没用preventDefault阻止默认动作。

<font color="#1ABC9C">这里一般用在滚动监听，@scoll，@touchmove 。因为滚动监听过程中，移动每个像素都会产生一次事件，每次都使用内核线程查询prevent会使滑动卡顿。我们通过passive将内核线程查询跳过，可以大大提升滑动的流畅度。</font>

注：passive和prevent冲突，不能同时绑定在一个监听器上。


参考：[vue中的 .passive 修饰符](https://www.jianshu.com/p/b12d0d3ad4c1)
