# 第 11 题：MVVM 理解

## 什么是MVVM

> MVVM 其实表示的是 Model-View-ViewModel

* Model：模型层，负责处理业务逻辑以及服务端进行交互
* View：视图层，负责将数据模型转化为UI展示出来，可以理解为HTML页面
* ViewModel：视图模型层，用来连接 Model 和 View,是 Model 和 View 之间的通信桥梁

在 MVVM 的架构下，View 层和 Model 层并没有直接联系，而是通过ViewModel层进行交互。

ViewModel 层通过 双向数据绑定将 View 层和 Model 层连接了起来，使得View层和Model层的同步工作完全是自动的。

## 实现双向数据绑定步骤

1. 实现一个指令解析器 Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数.
> 主要做的事情是解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

2. 实现一个数据监听器 Observer, 能够对数据对象的所有属性进行监听，如有变化可拿到最新值并通知订阅者

3. 实现一个 订阅者 Watcher，作为连接 Observer 和 Compile 的桥梁，能够订阅并收到每个属性变动的通知，指向指令绑定的响应回调函数，从而更新视图


## 总结

vue.js是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调来渲染视图

Model：数据以及业务逻辑
View：展示效果，UI组件
ModelView：核心，将数据和视图关联起来，视图变化后更新数据以及数据变化后更新视图。

主要包括两个部分组成

1. 监听器Observer：对所有数据的属性进行监听,对数据对象进行递归遍历，包括子属性对象的属性，都加上 getter 和 setter。这样的话，给这个对象的某个值赋值，就会触发setter, 那么就可以监听到了数据变化

2. 解析器Compile：对节点元素的指令进行扫描和解析，根据指令模板替换对应的数据，以及绑定响应的更新函数。解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

3. 订阅者Watcher: 是Observer和Compile之间通信的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的回调函数，从而更新视图，主要做的事情有
1、在自身实例化时王属性订阅器（dep）里面添加自己
2、自身必须有一个update()方法
3、带属性变动 dep.notice() 通知时，能够调用自身的 update() 方法，并触发 compile 中绑定的回调。

MVVM 作为数据绑定的入口，整合 Observe、Compile和Watcher 三者，通过Observe来监听自身的model变化，通过Compile来解析编译模板指令，最终利用Watcher 搭建Observe和Compile之间的通信桥梁，达到数据变化-->视图更新；视图交互变化--->数据model变化的双向绑定效果

[链接1](https://blog.nowcoder.net/n/8517450fe4fd4220b4078f9c61e42ec1)

[链接2](https://vue3js.cn/interview/vue/bind.html#%E4%BA%8C%E3%80%81%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A%E7%9A%84%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)

[链接3](https://juejin.cn/post/7080562890628923423?searchId=20231213172744D10D0A5303513EB6A613)

[链接4](https://blog.csdn.net/weixin_43638968/article/details/123635980)
