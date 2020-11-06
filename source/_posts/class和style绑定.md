---
title: class和style绑定,vue.observable,slot插槽
date: 2020-07-14 10:33:37
tags: vue
categories: 进阶
---
## class和style绑定
```html
<html>
  <head>
    <title>class 和 style 绑定的高级用法</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <div :class="['active', 'normal']">数组绑定多个class</div>
      <div :class="[{active: isActive}, 'normal']">数组包含对象绑定class</div>
      <div :class="[showWarning(), 'normal']">数组包含方法绑定class</div>
      <div :style="[warning, bold]">数组绑定多个style</div>
      <div :style="[warning, mix()]">数组包含方法绑定style</div>
      <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }">style多重值</div>
    </div>
    <script>
      new Vue({
        el: '#root',
        data() {
          return {
            isActive: true,
            warning: {
              color: 'orange'
            },
            bold: {
              fontWeight: 'bold'
            }
          }
        },
        methods: {
          showWarning() {
            return 'warning'
          },
          mix() {
            return {
              ...this.bold,
              fontSize: 20
            }
          }
        }
      })
    </script>
  </body>
</html>
```
## vue.observable
```html
<html>
  <head>
    <title>Vue.observable</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      {{message}}
      <button @click="change">Change</button>
    </div>
    <script>
      const state = Vue.observable({ message: 'Vue 2.6' })
      const mutation = {
        setMessage(value) {
          state.message = value
        }
      }
      new Vue({
        el: '#root',
        computed: {
          message() {
            return state.message
          }
        },
        methods: {
          change() {
            mutation.setMessage('Vue 3.0')
          }
        }
      })
    </script>
  </body>
</html>
```
## slot插槽
```html
<html>
  <head>
    <title>插槽 slot</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <div>案例1：slot的基本用法</div>
      <Test>
        <template v-slot:header="{user}">
          <div>自定义header({{user.a}})</div>
        </template>
        <template v-slot="{user}">
          <div>自定义body({{user.b}})</div>
        </template>
      </Test>
    </div>
    <div id="root2">
      <div>案例2：Vue2.6新特性 - 动态slot</div>
      <Test>
        <template v-slot:[section]="{section}">
          <div>this is {{section}}</div>
        </template>
      </Test>
      <button @click="change">switch header and body</button>
    </div>
    <script>
      Vue.component('Test', {
        template:
          '<div>' +
            '<slot name="header" :user="obj" :section="\'header\'">' +
              '<div>默认header</div>' +
            '</slot>' +
            '<slot :user="obj" :section="\'body\'">默认body</slot>' +
          '</div>',
        data() {
          return {
            obj: { a: 1, b: 2 }
          }
        }
      })
      new Vue({ el: '#root' })
      new Vue({
        el: '#root2',
        data() {
          return {
            section: 'header'
          }
        },
        methods: {
          change() {
            this.section === 'header' ?
              this.section = 'default' :
              this.section = 'header'
          }
        }
      })
    </script>
  </body>
</html>
```