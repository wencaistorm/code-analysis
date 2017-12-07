
Backbone.js 将所有的变量和方法赋值给对象 `Backbone`，只向外暴露一个变量 `Backbone` ，这样可以有效避免命名冲突。

但是假如除了 Backbone.js 暴露的 `Backbone` 之外，还存在着另外一个名为 `Backbone` 的变量，该如何处理呢？

Backbone.js 的处理方法如下：
```js
  var previousBackbone = root.Backbone;
  Backbone.noConflict = function() {
    root.Backbone = previousBackbone;
    return this;
  };
```