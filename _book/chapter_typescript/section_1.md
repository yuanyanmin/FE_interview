# 泛型

## 介绍

泛型是程序设计语言的一种分格或者范式

泛型允许我们在强类型程序设计语言中编写代码时使用一些以后才指定的类型，在实例化时作为参数指明这些类型在 typescript 中，定义函数，接口或者类的时候，不预先定义好具体的类型，而在使用的时候指定类型的一种特性

## 使用方法

泛型通过 <> 的形式进行表述，可以申明：
* 函数
```
function returnItem<T>(para: T): T {
    return para
}
```
> 可以一次定义 [多个类型参数]，比如泛型T 和 泛型U
```
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}
 
swap([7, 'seven']); // ['seven', 7]
```
* 接口
```
interface ReturnItemFn<T> {
    (para: T): T
}
```
> 当我们想传入一个 number 作为参数的时候，可以这样申明

```
const returnItem: ReturnItemFn<number> = para => para
```

* 类

```
class Stack<T> {
    private arr: T[] = []
 
    public push(item: T) {
        this.arr.push(item)
    }
 
    public pop() {
        this.arr.pop()
    }
}
```
> 使用方式如下：

```
const stack = new Stacn<number>()

```


