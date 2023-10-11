# 第 1 题：vue-router 原理

## 前端路由

web 前端单页面中，路由描述的是 URL 与 UI 之间的映射关系，这种关系是单向的，即 URL 变化引起 UI 更新（无须刷新页面）

## hash 实现

hash 是 URL 中 hash (#) 及后面的那部分，常用作锚点在页面内进行导航，最重要的是改变 URL 中的 hash 部分不会引起页面刷新。

我们可以通过 hashchange 事件监听 URL 的变化，改变 URL 的方式只有这几种：
* 通过浏览器前进后退改变 URL 
* 通过标签改变 URL
* 通过 window.location 改变 URL 

## history 实现

HTML5 标准的发布，history 的 api 增加了两个 API。pushState 和 replaceState。通过这两个 API 可以改变 URL 地址且不会发送请求。同时还有 popstate 事件。

用了这种实现方式，单页面路由的 url 就不会多出一个 #，变得更新加美观，但因为没有 # 号，所以当用户刷新页面之类的操作时候，浏览器还是会给服务器端发送请求。为了避免这种情况，所以这个实现需要服务器的支持，需要吧所有路由都重定向到根页面。

* 通过 pushState/replaceState或者标签改变 URL 不会触发页面刷新，也不会触发popstate方法。所以我们可以拦截 pushState/replaceState的调用和标签的点击事件来检测 URL 变化，从而触发 router-view 的视图更新
* 通过浏览器前进后退改变 url,或者通过 js 调用 history 的 back、go、forward方法，都会触发 popstate 事件，所以我们可以监听 popstate来触发 route-view 的视图更新

