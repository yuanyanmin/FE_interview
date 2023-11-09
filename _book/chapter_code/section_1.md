# 第 1 题：实现数组的 Map 方法

## 介绍 Array.prototyp.map

> map() 方法创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。

```
map(callbackFn)
map(callbackFn, thisArg)
```

* callbackFn
  * 为数组中的每个元素执行的函数。它的返回值作为一个元素被添加为新数组中。该函数被调用时将传入以下参数：
    * element: 数组中当前正在处理的元素
    * index: 正在处理的元素在数组中的索引
    * arr: 调用了 map() 的数组本身
* thisArg
  * 执行 callbackFn 时用作 this 的值。（可选）（callbackFn中绑定的this对象）


```
var new_array = arr.map(function callback(currentValue[, index[, array]]) {
  // Return element for new_array 
}[, thisArg])
```

## 详解 thisArg 参数

这里的意思是这个 thisArg 对象可以在callback函数中访问，这里有一个小坑，如果想要访问这个 thisArg 对象的话，只能使用 es5 函数方式。这是因为，箭头函数没有 不能绑定 this， 会捕获其所在的上下文的this值，作为自己的this值

```
let arr = [1,2,3];
let obj = {
  a:1
}

// 正确示例
arr.map(function(val){
  console.log(this)   // { a:1 }
}, obj)

// 错误示例
arr.map((val) => {
  console.log(this)  // window 对象
}, {a:1})

```

**示例**

```
var obj = { a: 3 };
var array = [1,2,3,4,5];
array.map(function (value) {
    return this.a * value;
}, obj); //  [3, 6, 9, 12, 15]
```

## 实现 map

```
Array.prototype.myMap = function(callback, thisArg) {
  let length = this.length;

  let res = [];
  if (!Array.isArray(this)) {
      throw new Error('this is not an  arrary')
  }

  if (typeof callback !== 'function') {
      throw new Error('callback is no a function')
  }

  if (length === 0) {
      return res
  }

  for (let i = 0; i < length; i++) {
      res[i] = callback.call(thisArg, this[i], i, this)
  }
  return res
}

let arr = [1, 2, 3, 4, 5]

let obj = { a: 3 }

let res = arr.myMap(function (item, index, arr) {
  return item * this.a
}, obj)

console.log('res', res); //  [3, 6, 9, 12, 15]

```