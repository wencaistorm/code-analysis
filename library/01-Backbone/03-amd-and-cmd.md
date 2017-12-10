## 背景代码

代码如下：[源码地址，Line15-Line32](https://github.com/jashkenas/backbone/blob/master/backbone.js#L15)
```js
(function(factory) {

  // Establish the root object, `window` (`self`) in the browser, or `global` on the server.
  // We use `self` instead of `window` for `WebWorker` support.
  var root = (typeof self == 'object' && self.self === self && self) ||
            (typeof global == 'object' && global.global === global && global);

  // Set up Backbone appropriately for the environment. Start with AMD.
  if (typeof define === 'function' && define.amd) {
    define(['underscore', 'jquery', 'exports'], function(_, $, exports) {
      // Export global even in AMD case in case this script is loaded with
      // others that may still expect a global Backbone.
      root.Backbone = factory(root, exports, _, $);
    });

  // Next for Node.js or CommonJS. jQuery may not be needed as a module.
  } else if (typeof exports !== 'undefined') {
    var _ = require('underscore'), $;
    try { $ = require('jquery'); } catch (e) {}
    factory(root, exports, _, $);

  // Finally, as a browser global.
  } else {
    root.Backbone = factory(root, {}, root._, (root.jQuery || root.Zepto || root.ender || root.$));
  }

})(function(root, Backbone, _, $) { 
  // some code here
})
```
关于 `root` 已经在第一篇里说过，这里不管它。

往下看，发现了这些变量：`define`、`define.amd`、`exports`、`require`，它们并不是 JavaScript 内置对象或属性，但也未发现声明它们的语句。不免令人疑惑。

其实这是为了兼容 JavaScript 的两种模块加载规范：[AMD](#AMD) 和 [CMD](#CMD)

## AMD

> AMD是 "`Asynchronous Module Definition`" 的缩写，意思就是 "**异步模块定义**"。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

RequireJS 是遵循 AMD 规范的典型代表，以 RequireJS 为例，简单介绍使用方式：

### （一）加载 require.js 以及主模块（入口模块）

1. `data-main` 用于指定入口模块
2. require.js 默认的文件后缀名是 js，因此省略 main.js 后缀

```js
  <script src="js/require.js" data-main="js/main"></script>
```
### （二）加载模块

1. 通过 `require()` 加载模块
2. `require()` 第一个参数是一个数组，用以指定模块的依赖，如果没有依赖，参数可省略
3. `require()` 第二个参数是一个回调函数，在依赖模块加载完毕后执行

```js
  // main.js
  require(['moduleA', 'moduleB', 'moduleC'], function (moduleA, moduleB, moduleC) {
    // some code here
  })
```
### （三）编写模块

1. 模块通过 `define()` 定义
2. `define()` 第一个参数是一个数组，用以指定模块的依赖，如果没有依赖，参数可省略
3. `define()` 第二个参数是一个回调函数，在依赖模块加载完毕后执行
4. 最后通过 `return` 语句向外暴露模块

```js
// moduleA.js
define(['moduleB], function (moduleB) {
  var moduleA = function (){
    // some code here
  };
  return {
    moduleA: moduleA
  };
});
```

更详细的使用方法，可参考阮老师的教程：

[Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

[Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)

[Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)


## CMD

[Sea.js 手册与文档](http://www.zhangxinxu.com/sp/seajs/)

[CMD 模块定义规范](https://github.com/seajs/seajs/issues/242)



相关文章：
[SeaJS与RequireJS最大的区别](https://www.douban.com/note/283566440/)
[与 RequireJS 的异同 - seajs](https://github.com/seajs/seajs/issues/277)
[AMD 和 CMD 的区别有哪些？](https://www.zhihu.com/question/20351507/answer/14859415)