---
title: js语法细节
date: 2020-11-21 10:49:30
tags: 进阶
categories: js
---

## 箭头函数中的this
它们没有 this。在箭头函数内部访问到的 this 都是从外部获取的。

## 可选链
- 通常我们这样写
```js
let user = {}; // user 没有 address
alert( user && user.address && user.address.street ); // undefined（不报错）
```
依次对整条路径上的属性使用与运算进行判断，以确保所有节点是存在的（如果不存在，则停止计算），但是写起来很麻烦。
- 通过可选链可以这样
```js
let user = null;
alert( user?.address ); // undefined
alert( user?.address.street ); // undefined
```
>不要过度使用可选链

>我们应该只将 ?. 使用在一些东西可以不存在的地方。
>例如，如果根据我们的代码逻辑，user 对象必须存在，但 address 是可选的，那么 user.address?.street 会更好。
>所以，如果 user 恰巧因为失误变为 undefined，我们会看到一个编程错误并修复它。否则，代码中的错误在不恰当的地方被消除了，这会导致调试更加困难。

```?.()```检查左边是否存在，存在就执行,否则运算停止，不报错。

### 总结
可选链 ?. 语法有三种形式：

- obj?.prop —— 如果 obj 存在则返回 obj.prop，否则返回 undefined。
- obj?.[prop] —— 如果 obj 存在则返回 obj[prop]，否则返回 undefined。
- obj.method?.() —— 如果 obj.method 存在则调用 obj.method()，否则返回 undefined。
>正如我们所看到的，这些语法形式用起来都很简单直接。?. 检查左边部分是否为 null/undefined，如果不是则继续运算。
?. 链使我们能够安全地访问嵌套属性。

>但是，我们应该谨慎地使用 ?.，仅在当左边部分不存在也没问题的情况下使用为宜。以保证在代码中有编程上的错误出现时，也不会对我们隐藏。

## Symbol
如果我们要在对象字面量 {...} 中使用 Symbol，则需要使用方括号把它括起来。
就像这样：
```js
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // 而不是 "id"：123
};
```
这是因为我们需要变量 id 的值作为键，而不是字符串 “id”。

>Symbol 属性不参与 for..in 循环。

>Object.assign 会同时复制字符串和 symbol 属性

>参考：[symbol](https://zh.javascript.info/symbol#dui-xiang-zi-mian-liang-zhong-de-symbol)


## 对象原始值的转换
如果没有 Symbol.toPrimitive，那么 JavaScript 将尝试找到它们，并且按照下面的顺序进行尝试：

- 对于 “string” hint，toString -> valueOf。
- 其他情况，valueOf -> toString。

默认情况下，普通对象具有 toString 和 valueOf 方法：

- toString 方法返回一个字符串 "[object Object]"。
- valueOf 方法返回对象自身。
- 如果没有 Symbol.toPrimitive 和 valueOf，toString 将处理所有原始转换。

### 总结
对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。

这里有三种类型（hint）：

- "string"（对于 alert 和其他需要字符串的操作）
- "number"（对于数学运算）
- "default"（少数运算符）
规范明确描述了哪个运算符使用哪个 hint。很少有运算符“不知道期望什么”并使用 "default" hint。通常对于内建对象，"default" hint 的处理方式与 "number" 相同，因此在实践中，最后两个 hint 常常合并在一起。

转换算法是：

1.调用 obj[Symbol.toPrimitive](hint) 如果这个方法存在，

2.否则，如果 hint 是 "string"
- 尝试 obj.toString() 和 obj.valueOf()，无论哪个存在。

3.否则，如果 hint 是 "number" 或者 "default"
- 尝试 obj.valueOf() 和 obj.toString()，无论哪个存在。
在实践中，为了便于进行日志记录或调试，对于所有能够返回一种“可读性好”的对象的表达形式的转换，只实现以 obj.toString() 作为全能转换的方法就够了。

>参考 [Symbol.toPrimitive](https://zh.javascript.info/object-toprimitive#symboltoprimitive)

## 关于数组的length
当我们修改数组的时候，length 属性会自动更新。准确来说，它实际上不是数组里元素的个数，而是最大的数字索引值加一。

例如，一个数组只有一个元素，但是这个元素的索引值很大，那么这个数组的 length 也会很大：
```js
let fruits = [];
fruits[123] = "Apple";

alert( fruits.length ); // 124
```
要知道的是我们通常不会这样使用数组。

length 属性的另一个有意思的点是它是可写的。

如果我们手动增加它，则不会发生任何有趣的事儿。但是如果我们减少它，数组就会被截断。该过程是不可逆的，下面是例子：
```js
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // 截断到只剩 2 个元素
alert( arr ); // [1, 2]

arr.length = 5; // 又把 length 加回来
alert( arr[3] ); // undefined：被截断的那些数值并没有回来
```
>所以，清空数组最简单的方法就是：arr.length = 0;

## 关于JSON的转换
JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 JSON.stringify 跳过。

即：
- 函数属性（方法）。
- Symbol 类型的属性。
- 存储 undefined 的属性。