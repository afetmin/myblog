---
title: createObjectURL
date: 2021-09-08 09:08:43
tags: js
---

## createObjectURL 是什么

> URL.createObjectURL() 静态方法会创建一个 DOMString，其中包含一个表示参数中给出的对象的 URL。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的 URL 对象表示指定的 File 对象或 Blob 对象。 — MDN

## createObjectURL 可以用来做什么

比如显示上传的预览图

```html
<body>
  <input
    type="file"
    multiple
    id="fileInput"
    onchange="previewFiles(this.files)"
  />
  <ul id="preview"></ul>
  <script>
    const inputEl = document.getElementById('fileInput') const ulEle =
    document.getElementById('preview') function previewFiles(files){" "}
    {Array.from(files).forEach((file) => {
      const li = document.createElement("li")
      ulEle.appendChild(li)
      const img = document.createElement("img")
      img.src = URL.createObjectURL(file)
      img.height = 100
      img.width = 100
      img.onload = function () {
        // 当图片加载完成之后对象URL就不再需要了
        URL.revokeObjectURL(this.src)
      }
      li.appendChild(img)
    })}
  </script>
</body>
```

来看看效果：

![效果](/img/createObjectURL/1.png)

> 在每次调用  `createObjectURL()`  方法时，都会创建一个新的 URL 对象，即使你已经用相同的对象作为参数创建过。当不再需要这些 URL 对象时，每个对象必须通过调用  [URL.revokeObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL) 方法来释放。

浏览器在 document 卸载的时候，会自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。

## FileReader 的 readAsDataURL

> readAsDataURL 方法会读取指定的 Blob 或 File 对象。读取操作完成的时候，readyState 会变成已完成 DONE，并触发 loadend 事件，同时 result 属性将包含一个 data:URL 格式的字符串（base64 编码）以表示所读取文件的内容。

还是上一个例子

```js
function useFileReaderPreivewFiles(files) {
  Array.from(files).forEach((file) => {
    const reader = new FileReader()
    const li = document.createElement("li")
    ulEle.appendChild(li)
    reader.onload = function (e) {
      const img = new Image()
      img.height = 100
      img.width = 100
      img.src = e.target.result
      li.appendChild(img)
    }
    reader.readAsDataURL(file)
  })
}
```

同样的效果:

![效果图](/img/createObjectURL/2.png)

来一张终极图：

![data_url.jpg](/img/createObjectURL/3.jpg)
