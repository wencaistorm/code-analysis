# JavaScript 的全局对象

Backbone 开头有这么几行代码：（[源码地址](https://github.com/jashkenas/backbone/blob/master/backbone.js#L10)）

```js
  // Establish the root object, `window` (`self`) in the browser, or `global` on the server.
  // We use `self` instead of `window` for `WebWorker` support.
  var root = (typeof self == 'object' && self.self === self && self) ||
            (typeof global == 'object' && global.global === global && global);
```

相信对 JavaScript 有所了解的同学都知道：

> JavaScript 中有一个特殊的对象，称为全局对象（Global Object），它及其所有属性都可以在程序的任何地方访问，即全局变量。

> 在浏览器 JavaScript 中，通常 `window` 是全局对象， 而 Node.js 中的全局对象是 `global`。

因此：这段代码的作用就是为了兼容浏览器端和服务端（nodejs），将不同环境下的全局对象都赋值给 `root` 变量


