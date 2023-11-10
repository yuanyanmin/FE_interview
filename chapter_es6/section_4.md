# 第 4 题 require 与 import

CommonJS 模块化方案 require/exports 是为服务器端开发设计的。服务器模块系统同步读取模块文件内容，编译执行后得到的模块接口。


* 原生浏览器不支持 require/exports，可使用支持 CommonJS 模块规范的 Browsersify、webpack 等打包工具，它们会将 require/exports 转换成能在浏览器使用的代码。
* import/export 在浏览器中无法直接使用，我们需要在引入模块的 `<script>` 元素上添加type="module" 属性。

#### 用法

* require/exports
```
const fs = require('fs')

exports.fs = fs

moudule.exports = fs
```

```
exports = module.exports = {};
```

* import/export

```
import fs from 'fs'
import {readFile} from 'fs' //从 fs 导入 readFile 模块
import {default as fs} from 'fs' //从 fs 中导入使用 export default 导出的模块
import * as fileSystem from 'fs' //从 fs 导入所有模块，引用对象名为 fileSystem
import {readFile as read} from 'fs' //从 fs 导入 readFile 模块，引用对象名为 read

export default fs
export const fs
export function readFile
export {readFile, read}
export * from 'fs'
```

### 模块化

最初我们都使用script标签来引入js，但当一个页面引入的js文件越来越多时，就产生了几个难以避免的问题：

* 全局变量污染
* 变量重名
* js之间的依赖关系复杂，无法保证顺序


模块化规范的出现就是为了解决以上问题，同时提高代码的效率和复用性。主流的模块化规范包括CommonJS、AMD、CMD、UMD和ESModule这几种，其中CommonJS主要用于服务端NodeJS项目；AMD和CMD提供相似的异步加载模块方式，但对依赖的处理稍有不同；ESM是依赖于ES静态模块结构的一种更为高效的模块规范；UMD则是对以上几种规范的兼容

#### CommonJS

非浏览器环境的JavaScript模块化规范，最常见的场景是用于NodeJS。

```
 // 引入 
const doSomething = require('./doSomething.js'); 

// 导出
module.exports = function doSomething(n) { 
// do something 

```

**特点**

* 模块在运行时加载和执行，并且只在首次加载时运行一次，然后将运行结果缓存，以备后续多次加载；
* 模块加载方式为同步加载，多个模块依次按顺序加载；
* require 语句可以放在块级作用域或条件语句中

#### AMD

即异步模块化定义规范，主要用于浏览器端，其中最具代表性的是require.js。

```
 // 引入
require(["./amd.js"], function (m) { 
    console.log(m); 
});

// 导出
define(['dep1', 'dep2'], function (dep1, dep2) { 
    // Define the module value by returning a value. 
    return function () {}; 
});

```

**特点**

* 异步加载模块
* 依赖前置，即模块加载前会先加载依赖，加载完成后会执行回调


#### CMD

CMD 和 AMD 一样，都是用于浏览器端的异步模块定义规范，最常见的实践是sea.js。它和AMD的最大区别是对依赖的处理时机，AMD要求先加载依赖，再执行当前模块逻辑，CMD则是执行到相应依赖时再加载依赖

```
 // 引入 
seajs.use(["./cmd2.js"], function () { 
    ...
});

// 导出
define(function (require, exports, module) { 
    ...
    const num = require("./cmd3.js");
    ...
});

```

**特点**

* 和AMD规范大体一致

* 区别是依赖后置，即需要的时候才通过require来加载依赖

#### ESM

ESM(ES Module)，基于ES的模块标准，也是当前最常使用的模块化标准。

```
 // 引入
import {foo, bar} from './myLib'; 

// 导出
export default function() { 
    // your Function 
}; 
export const function1() {...}; 
export const function2() {...}

```

**特点**

* 支持大多数现代浏览器，但也存在部分浏览器兼容问题 （jakearchibald.com/2017/es-mod…）
* 它既拥有像CJS一样简洁的语法，同时又支持AMD的异步加载
* 得益于ES6 的静态模块结构（static module structure），ESM规范可以支持打包时的tree-shaking，支持移除不必要的引用
* 编译时加载
* 可以在html中引用

```
 <script type="module"> 
    import {func1} from 'my-lib'; 
    func1(); 
</script>

```

#### UMD

UMD(Universal Module Definition),通用模块定义，顾名思义是对以上几种标准的统一，使每个版本都能兼容运行。

```
 (function (root, factory) { 
    if (typeof define === "function" && define.amd) { 
    // 支持 AMD 规范
        define(["jquery", "underscore"], factory);
    } else if (typeof define === 'function' && define.cmd){ 
    // 支持 CMD 规范
        define(function(require, exports, module) { 
            module.exports = factory() 
        })
    } else if (typeof exports === "object") { 
    // 支持 CommonJS
        module.exports = factory(require("jquery"), require("underscore")); 
    } else { 
    // 支持全局引用
        root.Requester = factory(root.$, root._); 
    } 
}(this, function ($, _) { 
    // this is where I defined my module implementation 
    var Requester = { // ... }; 
    return Requester; 
}))

```

**特点**

* 兼容浏览器和服务端两种场景
* 兼容 CMD、AMD、CJS 等多种规范，用法也相同

### 编译时加载与运行时加载

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性

**CommonJS**

CommonJS是运行时加载，例如下面的代码，是加载了fs模块整体，再读取fs对象中的属性stat等。

```
// CommonJS模块
let { stat, exists, readfile } = require('fs')

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”


**ESM**

ESM 是编译时加载，例如下面的代码，就只加载了模块中的两个方法而不是全部。因为es模块设计的思想是静态化，显式地通过export输出，使得编译时就可以进行静态分析。

不同于CommonJS引入的模块是一个对象，ESM引入的是模块本身，并不是一个对象。

```
 // a.js
export const a = 1;
export const b = 2;
export const c = 3;

// b.js
import { a, b } from 'a.js'
```

上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。