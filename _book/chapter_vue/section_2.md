# 第 2 题：组件通信

* props/$emit

父组件A通过props的方式向子组件B传递，B to A 通过在 B 组件中 $emit, A 组件中 v-on 的方式实现。


* EventBus($emit/$on)

这种方法通过一个空的Vue实例作为中央事件总线（事件中心），用它来触发事件和监听事件,巧妙而轻量地实现了任何组件间的通信，包括父子、兄弟、跨级

```
var Event=new Vue();
Event.$emit(事件名,数据);
Event.$on(事件名,data => {});

```

* vuex

* $attr/$listeners

* provide/inject

```
// 父
export default {
  provide: {
    name: '浪里行舟'
  }
}
// 子
export default {
  inject: ['name'],
  mounted () {
    console.log(this.name);  // 浪里行舟
  }
}



```

* $parent/$children 与 ref

  * ref：如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例
  * $parent / $children：访问父 / 子实例