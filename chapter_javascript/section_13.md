# 第 13 题：JavaScript 设计模式

## 单例模式

**特点：** 维护一个全局对象

* 引用第三方库（多次引用只会使用一个库引用，例如 JQuery）
* 弹窗（登录框，信息提示框）
* 购物车（一个用户只有一个购物车）
* 全局状态管理（Vuex）

**优缺点：**

* 优点：适用于单一对象，只生成一个对象实例，避免频繁创建和销毁实例，减少内存占用。
* 缺点：不适用于动态扩展对象，或需要创建多个相似对象的场景


## 策略模式

```
// 策略类（开发人员）
var Strategies = {
    "backend": function(task) {
        console.log('进行后端任务：', task);
    },
    "frontend": function(task) {
        console.log('进行前端任务：', task);
    },
    "testend": function(task) {
        console.log('进行测试任务：', task);
    }
};

//  环境类（开发组长）
var Context = function(type, task) {
    typeof Strategies[type] === 'function' && Strategies[type](task);
}

Context('backend', '优化服务器缓存');
Context('frontend', '优化首页加载速度');
Context('testend', '完成系统并发测试');
```

**优缺点**

* 优点：
  * 利用组合、委托、多态的技术和思想，避免多重条件选择语句 if...else/switch...case
  * 复用性更高，算法函数可在系统其它地方使用
  * 支持设计模式 “开发-封闭原则“ ，算法封装在独立的 Strategy 中，易于维护和扩展
  * 策略模式使用 “组合和委托” 来让 Context 拥有执行算法的能力，一种替换对象继承的可行方案

* 缺点：
  * 增加了许多策略类或对象
  * 必须了解各个 Strategy 的不同点，违反“最少知识原则”


## 代理模式

**优缺点**

* 优点：

  * 可拦截和监听外部对本体对象的访问；
  * 复杂运算前可以进行校验或资源管理；
  * 对象职能粒度细分，函数功能复杂度降低，符合 “单一职责原则”；
  * 依托代理，可额外添加扩展功能，而不修改本体对象，符合 “开发-封闭原则”


* 缺点：

  * 额外代理对象的创建，增加部分内存开销；
  * 处理请求速度可能有差别，非直接访问存在开销，但 “虚拟代理” 及 “缓存代理” 均能提升性能


## 适配者模式

适配器模式非常适用于跨浏览器兼容，例如强大的 jQuery 封装了事件处理的适配器，解决跨浏览器兼容性问题，极大简化我们日常编程操作

```
// $('selector').on 的实现
function on(target, event, callback) {
    if (target.addEventListener) {
        // 标准事件监听
        target.addEventListener(event, callback);
    } else if (target.attachEvent) {
        // IE低版本事件监听
        target.attachEvent(event, callback)
    } else {
        // 低版本浏览器事件监听
        target[`on${event}`] = callback
    }
}
```

**适用场景**

* 跨浏览器兼容
* 整合第三方SDK
* 新老接口兼容

**优缺点**

* 优点：兼容性，保证外部可统一接口调用
* 缺点：额外对象的创建，非直接调用，存在一定的开销（且不像代理模式在某些功能点上可实现性能优化）

## 迭代器模式

> 提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。

**特点**

* 为遍历不同数据结构的 “集合” 提供统一的接口；
* 能遍历访问 “集合” 数据中的项，不关心项的数据结构

## 观察者模式(Observer)

> 观察者模式：定义了对象间一种一对多的依赖关系，当目标对象 Subject 的状态发生改变时，所有依赖它的对象 Observer 都会得到通知。

**特征**

* 一个目标者对象 Subject，拥有方法：添加 / 删除 / 通知 Observer；

* 多个观察者对象 Observer，拥有方法：接收 Subject 状态变更通知并处理；

* 目标对象 Subject 状态变更时，通知所有 Observer。

## 发布订阅模式（Publisher && Subscriber）

> 发布订阅模式：基于一个事件（主题）通道，希望接收通知的对象 Subscriber 通过自定义事件订阅主题，被激活事件的对象 Publisher 通过发布主题事件的方式通知各个订阅该主题的 Subscriber 对象。

**应用**

* jQuery 的 on 和 trigger，$.callback();
* Vue 的双向数据绑定;
* Vue 的父子组件通信 $on/$emit