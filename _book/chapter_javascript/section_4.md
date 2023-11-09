# 第 4 题：数组遍历方式

* for 循环

  可以使用 break 和 continue ，且能改变原数组的值

* while循环

  功能和for一样

* forEach

  只能遍历，不支持 break 和 continue, 函数返回值为 undefined，return 也终止不了循环

* every

  判断数组内所有元素是否都能通过指定函数的测试

* some

  和every()做对比，该方法只要数组中存在某一个符合条件的就返回true

* filter

  过滤数组，返回新数组

* map

  对每一个数组元素应用函数，并返回一个新的数组

* reduce

  遍历数组全部元素，将函数处返回的数字累加到一起，并返回这个累加结果，可初始化一个值

  ```
  arr.reduce((prev,cur,index,arr)=>{

  },init)

  ```
  * arr: 表示将要原数组
  * prev:表示上一次调用回调时的返回值，或者初始值init
  * cur:表示当前正在处理的数组元素
  * index:表示正在处理的数组元素的索引，若提供init值，则索引为0，否则索引为1
  * init: 表示初始值

* for...in

  迭代对象，由于数组是特殊的对象，也可以进行迭代，但不推荐

  以任意顺序迭代一个对象的除Symbol以外的可枚举属性，包括继承的可枚举属性

  ```
  for (let prop in obj) {
    console.log("obj." + prop + " = " + obj[prop]);
  }
  ```

* for...of

  可遍历多种可迭代对象，支持 break 和 return

  在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

  ```
  for (let value of iterable) {
    //statements
  }
  ```

