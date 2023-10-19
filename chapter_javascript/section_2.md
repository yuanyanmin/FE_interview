# 第 2 题：深拷贝


#### Object.assign
Object.assign 默认是对对象进行深拷贝，它只对最外层的进行深拷贝，也就是当对象内嵌套有对象的时候，被嵌套的对象进行的还是浅拷贝；

```
function cloneDeepAssign(obj){
  return Object.assign({},obj)
  //Object.assign({},obj)
}

```

#### JSON实现深拷贝

```
function cloneDeepJson(obj){
  return JSON.parse(JSON.stringify(obj))
}

```

* 会忽略 undefined和symbol；
* 不可以对Function进行拷贝，因为JSON格式字符串不支持Function，在序列化的时候会自动删除；
* 诸如 Map, Set, RegExp, Date, ArrayBuffer 和其他内置类型在进行序列化时会丢失；
* 不支持循环引用对象的拷贝;（循环引用的可以大概地理解为一个对象里面的某一个属性的值是它自己）


#### 递归实现

```
function cloneDeepDi(obj){
  const newObj = {};
  let keys = Object.keys(obj);
  let key = null;
  let data = null;
  for(let i = 0; i<keys.length;i++){
    key = keys[i];
    data = obj[key];
    if(data && typeof data === 'object'){
      newObj[key] = cloneDeepDi(data)
    }else{
      newObj[key] = data;
    }
  }
  return newObj
}

```

#### StructuredClone

structuredClone是结构化拷贝算法的实现，能够实现几乎对所有数据类型的深拷贝。目前最新版本的浏览器都已经原生支持。

```
structuredClone(value)
structuredClone(value, { transfer })

```

```
// Create an object with a value and a circular reference to itself.
const original = { name: "MDN" };
original.itself = original;

// Clone it
const clone = structuredClone(original);

console.assert(clone !== original); // the objects are not the same (not same identity)
console.assert(clone.name === "MDN"); // they do have the same values
console.assert(clone.itself === clone); // and the circular reference is preserved

```