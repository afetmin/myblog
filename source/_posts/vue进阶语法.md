---
title: vue进阶语法
date: 2020-05-27 09:54:50
tags: vue
categories: vue
index_img: /img/vue进阶语法/code.jpg
banner_img: /img/vue进阶语法/code.jpg
---
## vue的部分进阶语法
学习一下，做下总结
### $emit和$on
> 本文内容引自[这里](http://www.youbaobao.xyz/admin-docs/guide/base/vue.html)
```html
<html>
  <head>
    <title>$emit 和 $on</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <button @click="boost">触发事件</button>
    </div>
    <script>
      new Vue({
        el: '#root',
        data() {
          return {
            message: 'hello vue'
          }
        },
        created() {
          this.$on('my_events', this.handleEvents)
        },
        methods: {
          handleEvents(e) {
            console.log(this.message, e)
          },
          boost() {
            this.$emit('my_events', 'my params')
          }
        }
      })
    </script>
  </body>
</html>
```
使用this.$on时既可以一个事件绑定多个处理函数，也可以多个事件绑定一个处理函数
```javascript
this.$on('event1',this.handleEvents1)
this.$on('event1',this.handleEvents2)
```
```javascript
this.$on(['event1','event2'],this.handleEvents)
```
### directive 用法
```html
<html>
  <head>
    <title>directive 用法</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <div v-loading="isLoading">{{data}}</div>
      <button @click="update">更新</button>
    </div>
    <script>
      Vue.directive('loading', {
        update(el, binding, vnode) {
          if (binding.value) {
            const div = document.createElement('div')
            div.innerText = '加载中...'
            div.setAttribute('id', 'loading')
            div.style.position = 'absolute'
            div.style.left = 0
            div.style.top = 0
            div.style.width = '100%'
            div.style.height = '100%'
            div.style.display = 'flex'
            div.style.justifyContent = 'center'
            div.style.alignItems = 'center'
            div.style.color = 'white'
            div.style.background = 'rgba(0, 0, 0, .7)'
            document.body.append(div)
          } else {
            document.body.removeChild(document.getElementById('loading'))
          }
        }
      })
      new Vue({
        el: '#root',
        data() {
          return {
            isLoading: false,
            data: ''
          }
        },
        methods: {
          update() {
            this.isLoading = true
            setTimeout(() => {
              this.data = '用户数据'
              this.isLoading = false
            }, 3000)
          }
        }
      })
    </script>
  </body>
</html>
```
通过directive命令可以创造自定义的指令，通过 v-指令名 进行使用，方便解耦复用。
<p class="note note-primary">TIP</br>官方文档：<a href="https://cn.vuejs.org/v2/api/#Vue-directive">https://cn.vuejs.org/v2/api/#Vue-directive</a></p>

### Vue.component 用法
```html
<html>
  <head>
    <title>Vue.component 用法</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <Test :msg="message"></Test>
    </div>
    <script>
      Vue.component('Test', {
        template: '<div>{{msg}}</div>',
        props: {
          msg: {
            type: String,
            default: 'default message'
          }
        }
      })
      new Vue({
        el: '#root',
        data() {
          return {
            message: "Test Component"
          }
        }
      })
    </script>
  </body>
</html>
```
### Vue.extend 进阶用法
```html
<html>
  <head>
    <title>Vue.extend 用法2</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      #loading-wrapper {
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,.7);
        color: #fff;
      }
    </style>
  </head>
  <body>
    <div id="root">
      <button @click="showLoading">显示Loading</button>
    </div>
    <script>
      function Loading(msg) {
        const LoadingComponent = Vue.extend({
          template: '<div id="loading-wrapper">{{msg}}</div>',
          props: {
            msg: {
              type: String,
              default: msg
            }
          },
          name: 'LoadingComponent'
        })
        const div = document.createElement('div')
        div.setAttribute('id', 'loading-wrapper')
        document.body.append(div)
        new LoadingComponent().$mount('#loading-wrapper')
        return () => {
          document.body.removeChild(document.getElementById('loading-wrapper'))
        }
      }
      Vue.prototype.$loading = Loading
      new Vue({
        el: '#root',
        methods: {
          showLoading() {
            const hide = this.$loading('正在加载，请稍等...')
            setTimeout(() => {
              hide()
            }, 2000)
          }
        }
      })
    </script>
  </body>
</html>
```
将 loading 挂载到 Vue 原型上，通过 this.$loading 使用，比通过 directive 方法更加方便。
### Vue.use 用法
```html
<html>
  <head>
    <title>Vue.use 用法</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      #loading-wrapper {
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100%;
        background: rgba(0,0,0,.7);
        color: #fff;
      }
    </style>
  </head>
  <body>
    <div id="root">
      <button @click="showLoading">显示Loading</button>
    </div>
    <script>
      const loadingPlugin = {
        install: function(vm) {
          const LoadingComponent = vm.extend({
            template: '<div id="loading-wrapper">{{msg}}</div>',
            props: {
              msg: {
                type: String,
                default: 'loading...'
              }
            }
          }, 'LoadingComponent')
          function Loading(msg) {
            const div = document.createElement('div')
            div.setAttribute('id', 'loading-wrapper')
            document.body.append(div)
            new LoadingComponent({
              props: {
                msg: {
                  type: String,
                  default: msg
                }
              }
            }).$mount('#loading-wrapper')
            return () => {
              document.body.removeChild(document.getElementById('loading-wrapper'))
            }
          }
          vm.prototype.$loading = Loading
        }
      }
      Vue.use(loadingPlugin)
      new Vue({
        el: '#root',
        methods: {
          showLoading() {
            const hide = this.$loading('正在加载，请稍等...')
            setTimeout(() => {
              hide()
            }, 2000)
          }
        }
      })
    </script>
  </body>
</html>
```
这种方法彻底把 loading 做成了一个插件，可以把主要插件内容放到别的文件中，然后通过 import 后用 Vue.use 来使用，更加解耦和方便。
