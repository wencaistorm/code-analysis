
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

那么，这段代码究竟是如何工作的呢？

我们先来模拟一下使用场景：

1. 假设在引入 backbone.js 之前已经有了一个全局变量 Backbone
```js
    // 全局作用域下
    var Backbone = { version: 'old' };
```

2. 假设引入的 backbone.js 代码
```js
    var root = this;
    var previousBackbone = Backbone;
    var Backbone = { version: 'new' };

    Backbone.noConflict = function () {
        root.Backbone = previousBackbone;
        return this;
    }
```
3. 进行冲突 (`conflict`) 处理
```js
    var newBackbone = Backbone.noConflict();
    console.log('Backbone: ', Backbone);
    console.log('newBackbone: ', newBackbone);
```
4. 打印结果截图如下（可自行复制上面代码在 chrome 浏览器的 console 面板执行）：

  ![](assets/01/02-1512710451000.png)

  可以看到，第一个 `Backbone` 的值未发生变化，而引入的 backbone.js 暴露的变量 `Backbone` 已由 `newBackbone` 代替。