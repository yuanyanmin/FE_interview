# JS的数据类型

## 数据类型

  * 基本数据类型：String、Number、Boolean、Null、Undefined、Symbol(ES6新增)、BigInt(ES2020新增)。
  * 引用数据类型：Object(Array、Function也属于对象的一种)。

## 基本数据类型与引用数据类型区别

* 存储方式不同
  1. 基本数据类型：保存在栈内存中，直接存取值。
  2. 引用数据类型：保存在堆内存中，变量其实是保存在栈内容的一个指针，这个指针指向堆内存。需要通过指针来存取值。
* 赋值行为不同
  1. 基本数据类型：值传递，复制一个变量时，会创建一个新的值。
  2. 引用数据类型：引用传递，复制一个变量时，只是复制了地址，两个变量指向同一个对象。
* 比较方式不同
  1. 基本数据类型：值比较，即比较的是实际值。
  2. 引用数据类型：引用比较，即比较他们是否指向同一个对象。
* 可变性不同：
  1. 基本数据类型：一旦创建就不能被修改。
  2. 引用数据类型：可以修改其属性或内容。

## 如何区分基本数据类型与引用数据类型

* typeof 操作符： 适用于大多数基本数据类型(除null外)。
```
typeof 123             // "number"
typeof "hello"         // "string"
typeof true            // "boolean"
typeof undefined       // "undefined"
typeof Symbol("id")    // "symbol"
typeof 123n            // "bigint"
typeof function(){}    // "function"
typeof {}              // "object"
typeof []              // "object"
typeof null            // ⚠️ "object"（这是 JS 的历史遗留 bug）
```
* instanceof 操作符：判断引用类型是否属于某个构造函数，不能用于基本类型。
```
[] instanceof Array          // true
{} instanceof Object         // true
new Date() instanceof Date   // true
function(){} instanceof Function // true
```
* Object.prototype.toString.call() 方法：适用于所有数据类型。
```
Object.prototype.toString.call("hello")   // "[object String]"
Object.prototype.toString.call(123)       // "[object Number]"
Object.prototype.toString.call(null)      // "[object Null]"
Object.prototype.toString.call([])        // "[object Array]"
Object.prototype.toString.call({})        // "[object Object]"
Object.prototype.toString.call(() => {})  // "[object Function]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(new Date())// "[object Date]"
```

* Array.isArray() 方法：判断是否为数组。
```
Array.isArray([])       // true
Array.isArray({})       // false
```

## 深拷贝与浅拷贝区别

* 浅拷贝：拷贝对象时，只复制第一层属性，如果属性是引用类型，则复制的是引用地址，不拷贝对象本身，因此修改拷贝后的对象会影响原对象。
  * Object.assign()
  * {...obj}
  * Array.prototype.slice()
* 深拷贝：拷贝对象时，拷贝对象本身，因此修改拷贝后的对象不会影响原对象。
  * JSON.parse(JSON.stringify(obj))
    * 无法处理函数、undefined、symbol类型
  * lodash.cloneDeep()
  * 递归拷贝

## 变量赋值与浅拷贝区别
| 对比点         | 变量赋值                            | 浅拷贝                                  |
|----------------|-------------------------------------|------------------------------------------|
| 拷贝的是？     | 直接复制引用地址                    | 复制一层新对象 + 内层引用（还是同一个） |
| 是否新建对象？ | ❌ 没有，新变量只是个别名           | ✅ 会创建一个新对象（但属性可能共享）     |
| 修改新变量是否影响原变量？ | ✅ 会（引用相同） | ⚠️ 一层不会影响，内层可能会（引用共享） |


### 1. 变量赋值：完全共享引用

```javascript
const a = { name: "Alice" };
const b = a;

b.name = "Bob";

console.log(a.name); // "Bob"（a 也被修改了）
```

### 2. 浅拷贝：新对象但内层共享
```
const a = {
  name: "Alice",
  info: { age: 25 }
};

const b = { ...a }; // 或者 Object.assign({}, a)
b.name = "Bob";         // ✅ 不影响 a.name
b.info.age = 30;        // ❗️ 修改会影响 a.info.age

console.log(a.name);       // "Alice"
console.log(a.info.age);   // 30（原对象也被改了）
```
