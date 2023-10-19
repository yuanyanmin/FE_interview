# 第 8 题：computed 与 watch

### 相同点

* 计算属性和监听属性，本质上都是一个 watcher 实例，他们都通过响应式系统预数据，页面建立通信。

### 不同点

* 计算属性存在缓存
* 监听的逻辑有差异， 监听属性是目标值变了,它去执行函数，而计算属性是函数的值变了, 它重新求值
* 页面刷新以后, 计算属性会默认立即执行, 而watch默认第一次加载不做监听，如果需要第一次加载做监听，添加immediate属性，设置为true（immediate:true）


### method 与 computed 对比

我们也可以使用methods实现前面的需求，当有多个地方在使用，对应的methods函数会被调用多次，函数内部的逻辑也需要执行多次；而计算属性因为存在缓存，只要数据未发生变化，则多次访问计算属性对应的函数只会执行一次


### 缓存的理解

计算属性有对应的 watcher，watcher 实例有一个 dirty 属性控制缓存的读取，当 watcher.dirty 为 true 时，调用计算属性 get 方法重新求值；当 watcher.dirty 为 false 时，直接返回 watcher.value 的值






