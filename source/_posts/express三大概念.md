---
title: express三大概念
date: 2020-07-17 10:28:24
tags: express
categories: 后端
---
## Express 三大基础概念
### 中间件
中间件是一个函数，在请求和响应周期中被顺序调用
```javascript
const myLogger = function(req, res, next) {
  console.log('myLogger')
  next()
}

app.use(myLogger)
```
提示：中间件需要在响应结束前被调用

### 路由
应用如何响应请求的一种规则
响应 / 路径的 get 请求：
```javascript
app.get('/', function(req, res) {
  res.send('hello node')
})
```
响应 / 路径的 post 请求：
```javascript
app.post('/', function(req, res) {
  res.send('hello node')
})
```
规则主要分两部分：
- 请求方法：get、post......
- 请求的路径：/、/user、/.*fly$/......

### 异常处理
通过自定义异常处理中间件处理请求中产生的异常
```javascript
app.get('/', function(req, res) {
  throw new Error('something has error...')
})

const errorHandler = function (err, req, res, next) {
  console.log('errorHandler...')
  res.status(500).json({
    error:'Error...'
  })
}

app.use(errorHandler)
```
使用时需要注意两点：

- 第一，参数一个不能少，否则会视为普通的中间件
- 第二，中间件需要在请求之后引用