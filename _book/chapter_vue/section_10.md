# 第 10 题：虚拟DOM原理

## 虚拟 DOM 介绍

所谓虚拟 DOM 就是用 js 的普通对象来描述DOM的类型、属性、子元素等信息。

```
{
  tag: '',
  children: [],
  text: '',
  el: null
}
```

## Patch 过程

path 函数是我们的主函数，主要是用来进行新旧 DOM 对比，找到差异来更新实际的 DOM，接受两个参数(oldVNode, newVNode)

```
export const path = (oldVNode, newVNode) => {

  // dom 元素
  if (!oldVnode.tag) {
    let el = oldVNode
    el.innerHTML = ''
    oldVNode = h(oldVNode.tagName.toLowerCase())
    oldVNode.el = el
  }

  // 对比新旧 VNode 即可
  pathchVNode(oldNode, newNode)

  return newVNode
}
```

pathchVNode 方法进行新旧 VNode 的比较以及更新 DOM

* 如果类型不同，不用比较，直接使用新的替换旧的
* 如果类型相同，则根据子节点情况判断
  * 新节点为文本节点，则删除旧节点的所有子节点，创建文本子节点
  * 新节点不存在子节点，而旧节点存在，那么移除旧节点的子节点;
  * 新节点不存在子节点，旧节点存在文本节点，那么移除该文本节点;
  * 新节点存在子节点，旧节点存在文本节点，那么移除该文本节点，然后插入新节点;
  * 新旧节点都有子节点的话那么就需要进入到diff阶段;

## diff 算法

diff算法的目的主要是用来尽可能复用旧的节点，以减小DOM操作的开销。

双端diff算法

> 双端 diff 是头尾指针向中间移动的同时，对比头头、尾尾、头尾、尾头是否可以复用，如果可以的话就移动对应的 dom 节点。
如果头尾没找到可复用节点就遍历 vnode 数组来查找，然后移动对应下标的节点到头部。
最后还剩下旧的 vnode 就批量删除，剩下新的 vnode 就批量新增。



