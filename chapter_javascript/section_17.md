# 继承

js有几种经典的继承方式。比如原型链继承、构造函数继承、组合继承、寄生组合继承、ES6继承

## 原型链继承
> 通过修改子类的原型为父类的实例，从而实现子类可以访问到父类构造函数以及原型上的属性或方法。

```
function Parent() {
  this.name = 'andy';
}

Parent.prototype.getName = function() {
  return this.name;
}

function Child() {}

Child.prototype = new Parent();

var child1 = new Child();
child1.getName(); // 'andy'
```

* 缺点：父类构造函数中的引用类型，会被所有子类实例共享。其中一个子类实例进行修改，会导致所有的其他子类实例这个值都会改变。

## 构造函数继承

> 通过修改父类构造函数this实现的继承。我们在子类构造函数中执行父类构造函数，同时修改父类构造函数的this为子类的this

```
function Parent() {
  this.name = ['fedaily']
}

function Child() {
  Parent.call(this)
}

var child = new Child()
child.name.push('fe')

var child2 = new Child() // child2.name === ['fedaily']
```
* 缺点：所有方法都定义在构造函数中，每次都需要重新创建（对比原型链继承的方式，方法直接写在原型上，子类创建时不需要重新创建方法）

## 组合继承

> 同时结合原型链继承、构造函数继承就是组合继承了。

```
function Parent() {
  this.name = 'fedaily'
}

Parent.prototype.getName = function() {
  return this.name
}

function Child() {
  Parent.call(this)
  this.topic = 'fe'
}

Child.prototype = new Parent()
// 需要重新设置子类的constructor，Child.prototype = new Parent()相当于子类的原型对象完全被覆盖了
Child.prototype.constructor = Child
```

* 缺点：父类构造函数被调用了两次。同时子类实例以及子类原型对象上都会存在name属性。

## 寄生组合继承

> 寄生组合继承其实就是在组合继承的基础上，解决了父类构造函数调用两次的问题。

```
function Parent() {
  this.name = 'fedaily'
}

Parent.prototype.getName = function() {
  return this.name
}

function Child() {
  Parent.call(this)
  this.topic = 'fe'
}

// 仔细看这个函数的实现
inherit(Child, Parent)
```
```
function inherit(child, parent) {
  var prototype = object(parent.prototype)
  prototype.constructor = child
  child.prototype = prototype
}

// 这个函数的作用可以理解为复制了一份父类的原型对象
// 如果直接将子类的原型对象赋值为父类原型对象
// 那么修改子类原型对象其实就相当于修改了父类的原型对象
function object(o) {
  function F() {}
  F.prototype = o;
  return new F();
}
```

* 优点：解决了组合继承中的构造函数调用两次，构造函数引用类型共享，以及原型对象上存在多余属性的问题。是推荐的最合理实现方式。

## ES6继承

> ES6提供了class语法糖，同时提供了extends用于实现类的继承

```
class Parent {
  constructor() {
    this.name = 'fedaily'
  }

  getName() {
    return this.name
  }
}

class Child extends Parent {
  constructor() {
    // 这里很重要，如果在this.topic = 'fe'后面调用，会导致this为undefined
    super()
    this.topic = 'fe'
  }
}

const child = new Child()
child.getName() // fedaily
```