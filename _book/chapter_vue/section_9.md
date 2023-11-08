# 第 9 题：路由 hash 模式与 history 模式

### hash 模式

hash 模式是一种把前端路由的路径用井号 # 拼接在真实 URL 后面的模式。当井号 # 后面的路径发生变化时，浏览器并不会重新发起请求，而是会触发 hashchange 事件。

监听地址栏url的 hashchange 事件，url改变时获取新url的哈希值，与VueRouter对象中配置的路由规则比较，找到新url对应的组件，用对应的组件替换`<router-view>`

**特点**

* hash值的变化不会导致浏览器向服务器发送请求，不会引起页面刷新
* hash值变化会触发hashchange事件
* hash值变化会在浏览器的历史中留下记录，使用的浏览器的后退按钮可以回到上一个hash值
* hash永远不会提交到服务器，即使刷新页面也不会

```
window.addEventListener('hashchange', function(){ 
  // 监听hash变化，点击浏览器的前进后退会触发
})
```

**优缺点**

* 优点
  * 兼容性好，支持低版本浏览器
  * 实现前端路由无须服务器端支持
* 缺点
  * url 带有 # 号，有点丑

### history 模式

history API 是 H5 提供的新特性，允许开发者直接更改前端路由，即更新浏览器 URL 地址而不重新发起请求

```
window.history.pushState(state, title, url) 
  // let currentState = history.state; 获取当前state
  // state：需要保存的数据，这个数据在触发popstate事件时，可以在event.state里获取
  // title：标题，基本没用，一般传 null
  // url：设定新的历史记录的 url。新的 url 与当前 url 的 origin 必须是一樣的，否则会抛出错误。url可以是绝对路径，也可以是相对路径。
  //如 当前url是 https://www.baidu.com/a/,执行history.pushState(null, null, './qq/')，则变成 https://www.baidu.com/a/qq/，
  //执行history.pushState(null, null, '/qq/')，则变成 https://www.baidu.com/qq/

window.history.replaceState(state, title, url)
  // 与 pushState 基本相同，但她是修改当前历史记录，而 pushState 是创建新的历史记录

window.addEventListener("popstate", function() {
  // 监听浏览器前进后退事件，pushState 与 replaceState 方法不会触发              
});

```

pushState和replaceState方法可以改变url，但是不会刷新页面，浏览器不会向服务端发送请求，具备了实现前端路由的能力。

**如何实现监听url的变化**

* 对比hash的hashchange方法，history的变化不会触发任何事件，我们可以通过罗列可能触发history变化的情况，对这些情况进行拦截，以此监听history的变化
* 对于单页面的history模式而言，url的改变只能由以下情况引起：
  * 点击浏览器的前进/后退按钮，onpopstate可以监听到
  * 点击a标签
  * 在JS代码中触发history.pushState()或history.replaceState()


### 两种模式的区别

* 外观：hash的url有个#符号，history没有，history外观更好看。
* 刷新：hash刷新会加载到地址栏对应的页面，history刷新浏览器会重新发起请求，如果服务端没有匹配当前的url，就会出现404页面。
* 兼容性：hash能兼容到IE8，history只能兼容到IE10。
* 服务端支持：hash（#及后面的内容）的变化不会导致浏览器向服务器发起请求；history刷新会重新向服务器发起请求，需要在服务端做处理：如果没有匹配到资源，就返回同一个html页面。
* 原理：hash通过监听浏览器的onhashchange()事件，查找对应的路由规则；history利用H5新增的pushState()和replaceState()方法改变url。
* 记录：hash模式只有#后面的内容被修改才会添加新的记录栈；history通过pushState()设置的url于当前url一模一样也会被记录到历史记录栈。