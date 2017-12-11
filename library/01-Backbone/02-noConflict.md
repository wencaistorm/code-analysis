## 背景
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

## 使用

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

  可以看到，第一个 `Backbone` 的值未发生变化，而引入的 backbone.js 声明的变量 `Backbone` 已由 `newBackbone` 代替。


## 分析

还是这段代码：
```js
 var root = this;
 var previousBackbone = Backbone;
 var Backbone = { version: 'new' };

 Backbone.noConflict = function () {
     root.Backbone = previousBackbone;
     return this;
 }
```
这里的 `previousBackbone` 相当于是个临时变量，临时将原有的 `Backbone` 赋值给 `previousBackbone` ，这样可以避免后续对变量 `Backbone` 的操作影响到原有的 `Backbone` 。

当执行 `var newBackbone = Backbone.noConflict();` 这段代码时，会进行下面两个操作

1. 临时变量 `previousBackbone` 重新赋值给 `Backbone` ，让出变量 `Backbone` 的控制权
2. 将 `this` 返回，并赋值给新声明的变量 `newBackbone` ，那 `this` 又是谁呢？很显然，`this` 是 **调用这个方法时的 `Backbone`**，即 Backbone.js 中声明的那个 `Backbone`

至此，已经完成了变量名冲突的处理，原来的 `Backbone` 仍然叫做 `Backbone` ，之后引入的 Backbone.js 中声明的 `Backbone` 有了新名字：`newBackbone` 。

## 总结

事实上，jQuery、Backbone、underscore 等都使用了这种方式处理冲突。

所以以后我们在封装自己的代码库时，可以向这些优秀的代码学习这种处理冲突的方式，让自己的代码更加健壮。

（本篇完）
