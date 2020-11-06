---
title: slice的思考
date: 2020-08-23 09:03:42
tags: js
categories: 进阶
---

## 关于为何在函数中使用Array.prototype.slice.call而不是直接用slice？
经常会看到这种写法，在一个函数中使用，Array.prototype.slice.call(arguments,1); 除了防止传入的arguments有slice同名函数，还有其他什么用处吗？

尤大的回答：
> arguments是每个函数在运行的时候自动获得的一个近似数组的对象（传入函数的参数从0开始按数字排列，而且有length）。比如当你 func('a', 'b', 'c') 的时候，func里面获得的arguments[0] 是 'a'，arguments[1] 是 'b'，依次类推。但问题在于这个arguments对象其实并不是Array，所以没有slice方法。Array.prototype.slice.call( )可以间接对其实现slice的效果，而且返回的结果是真正的Array。

## 箭头函数
写箭头函数时，如果箭头函数后面加了```{}```，里面代表代码块，必须写```return```,如果不写```{}```,就是直接返回，不用写```return```。

## 函数的形参实参
函数的形参几个用 ```fn.length``` 获得,函数的实参用```arguments.length```获得。
## 自执行函数与惰性函数
自执行函数就是惰性函数，自执行函数可以分割函数作用域。
惰性函数表示函数执行的分支只会在函数第一次调用的时候执行。后续我们所使用的就是这个函数执行的结果。