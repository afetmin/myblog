---
title: betterscroll的refresh问题
date: 2020-11-30 18:38:57
tags: vue
categories: 进阶
---

## betterscroll 在数据更新时问题
由于 ```betterscroll的实例化要先于我动态设置wrapper的高度```
导致betterscroll开始计算时的内高和外容器高度一致，```hasVerticalScroll``` 的值为 ```false```  。需要在页面更新后refresh一下。最后通过在 update 钩子函数中增加 betterScroll 的refresh 方法，成功实现滚动效果。
update 函数官网是这样说的:
>由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

异步获取数据渲染到页面上时，调用该钩子来重新计算 bs 实例，如果不在update 钩子里调用 refresh 方法，hasVerticalScroll 的值是false。导致:最后的高度```看似```计算正确，就是不能滑动。其实是因为 我是动态设置的wrapper的高度，因为不是绝对定位，需要先获得标题栏高度，才能算出 ```scrollwrapper``` 的高度。
