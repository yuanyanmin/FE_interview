# 第 3 题：前端性能优化

### 浏览器

* 减少http请求

* 使用http2.0

* 设置浏览器缓存策略

* 白屏时间做加载动画

### 资源

* 静态资源cdn

* 静态资源单独域名

* gzip 压缩

* 做服务端渲染（SSR）

* 将css放在头部，JavaScript放在底部

### 图片

* 字体图标代替图标

* 精灵图

* 图片懒加载

* 图片预加载

* 使用png格式的图片

* 小于10k的可以打包为 base64格式

### 代码

* 慎用全局变量

* 缓存全局变量

* 减少重绘回流

* 节流、防抖

* 少用闭包，减少内存泄漏

* 减少数据读取次数

### 项目

* 长列表优化

* web worker

* 避免 iframe 嵌套网页

### webpack

* 减少代码体积

* 按需加载

* 提取第三库代码

* webpack dll 优化

### vue

* 路由懒加载

* v-for 路由 加 key

* 第三方插件按需加载

* 使用 computed 和 watch

* v-for 的同时时候 避免使用 v-if

* destroy 时销毁事件

