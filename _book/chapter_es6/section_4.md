# require 与 import

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