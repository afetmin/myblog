---
title: 关于vue的mixin
date: 2020-10-30 10:47:27
tags: vue
categories: 进阶
---

## vue的mixin
当有多个组件写了多个同样的语句时，可以使用vue的mixin机制
创建一个utils/mixin.js文件，内容如下：
```js
import {
  mapGetters,
  mapActions
} from 'vuex'

export const bookMixin = {
  computed: {
    ...mapGetters(['bookName', 'bookCover']),
  },
  methods: {
    ...mapActions(['setBookName', 'setBookCover'])
  }
}
```
接下来就可以在组件内这样写：
```js
import { bookMixin } from 'utils/mixin'
  mixins:[bookMixin]
  methods: {}
```
这样可以将vuex的内容在一处增加，多处复用。

mapActions是写到methods里的，这样就可以替换这种写法
```js
// 原来写法
this.$store.dispatch('setBookName',this.bookName)
// 优雅写法
this.setBookName(this.bookName)
```
## 结束