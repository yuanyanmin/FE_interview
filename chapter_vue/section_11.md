# 第 11 题：MVVM 理解

## 什么是MVVM

> MVVM 其实表示的是 Model-View-ViewModel

* Model：模型层，负责处理业务逻辑以及服务端进行交互
* View：视图层，负责将数据模型转化为UI展示出来，可以理解为HTML页面
* ViewModel：视图模型层，用来连接 Model 和 View,是 Model 和 View 之间的通信桥梁

在 MVVM 的架构下，View 层和 Model 层并没有直接联系，而是通过ViewModel层进行交互。

ViewModel 层通过 双向数据绑定将 View 层和 Model 层连接了起来，使得View层和Model层的同步工作完全是自动的。

## 实现双向数据绑定步骤

1. 实现一个指令解析器 Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数

2. 实现一个数据监听器 Observer, 能够对数据对象的所有属性进行监听，如有变化可拿到最新值并通知订阅者

3. 实现一个 Watcher，作为连接 Observer 和 Compile 的桥梁，能够订阅并收到每个属性变动的通知，指向指令绑定的响应回调函数，从而更新视图