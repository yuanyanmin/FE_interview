# 原型和原型链

## 原型(prototype)

JavaScript 中的每个对象都有一个原型属性__proto__，这个属性指向另一个对象，这个对象就是该对象的原型。

* 所有对象都有 __proto__ 属性；
* 只有函数有 prototype 属性；


```
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function() {
  console.log(`Hi, I'm ${this.name}`);
};

const alice = new Person("Alice");

alice.sayHi(); // 找不到 sayHi，在原型上找到并执行
```

```
Person --------> Person.prototype --------> Object.prototype --------> null
   |                   ↑
   |                   |
   v                   |
alice.__proto__  === Person.prototype
```
##  原型链(Prototype Chain)

在 JavaScript 中，每一个对象都有一个“隐藏的属性”指向它的原型对象（__proto__）。这个原型对象本身也是一个对象，也可能有它自己的原型，以此类推，形成所谓的原型链.

## 创建对象的方式

* new
* 字面量表示法
* Object.create()

## new 操作符

* 创建一个全新的对象，并将其 __proto__属性指向构造函数的 prototype 属性；
* 构造函数内部的 this 关键字指的是新创建的对象，并执行构造函数中的代码；
* 如果构造函数返回的是一个对象，则返回该对象，否则返回这个新的对象；

```
function operate(func, ...args) {
   let newObj = Object.create(func.prototype);
   let result = func.apply(newObj, args);
   return result instanceof Object ? result : newObj;
}
```
### Object.create()

会创建一个新对象，第一个参数接收一个对象，将会作为新创建对象的原型对象，第二个可选参数是属性描述符

```
function CreateObj(proto) {
   function F() {}
   F.prototype = proto;
   return new F();
}
```
我们平常所说的空对象，其实并不是严格意义上的空对象，它的原型对象指向Object.prototype，还可以继承hasOwnProperty、toString、valueOf等方法。

如果想要生成一个不继承任何属性的对象，可以使用Object.create(null)。

如果想要生成一个平常字面量方法生成的对象，需要将其原型对象指向Object.prototype：

```
let obj = Object.create(Object.prototype)
// 等价于
let obj = {}
```
[参考链接](https://segmentfault.com/a/1190000042725370#item-1)