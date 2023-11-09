# 第 7 题：路由守卫

### 全局路由钩子

* beforeEach(to, from, next)
> 全局前置守卫，在路由跳转前触发，参数包括 to,from,next 三个

* beforeResolve(to, from, next)
> 全局解析守卫，在路由跳转前触发，参数包括 to,from,next 三个
> 这和 router.beforeEach 类似，因为它在每次导航时都会触发，不同的是，解析守卫刚好会在导航被确认之前、所有组件内守卫和异步路由组件被解析之后调用

* afterEach(to, from)
> 全局后置钩子，参数包括to,from没有了next

```
router.beforeEach((to, from, next) => {
  // to 将要访问的路径
  // from 代表从哪个路径跳转而来
  // next 是一个函数，表示放行
  // next() 放行  next('/login') 强制跳转
})

```

### 路由独享的守卫

* beforeEnter(to, from, next)

> 是指在单个路由配置的时候也可以设置的钩子函数，只在进入路由时触发，不会在 params、query 或 hash 改变时触发

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})

```

### 组件内的守卫

* beforeRouteEnter(to, from, next)

> 路由进入之前调用，参数包括to，from，next。不能 访问 this，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建，不过，你可以通过传一个回调给 next 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数

```
beforeRouteEnter (to, from, next) {
  // 这里还无法访问到组件实例，this === undefined
  next( vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

* beforeRouteUpdate(to, from, next)

> 在当前路由改变时(/foo/:id)，并且该组件被复用时调用，可以通过this访问实例

```
beforeRouteUpdate (to, from) {
  // just use `this`
  this.name = to.params.name
}
```

* beforeRouteLeave(to, from, next)

> 导航离开该组件的对应路由时调用，可以访问组件实例this,通常用来预防用户在还未保存修改前突然离开。该导航可以通过返回 false 来取消。


### 导航守卫回调参数

* to：目标路由对象；
* from：即将要离开的路由对象；
* next：他是最重要的一个参数，他相当于佛珠的线，把一个一个珠子逐个串起来。以下注意点务必牢记：
  * 但凡涉及到有next参数的钩子，必须调用next() 才能继续往下执行下一个钩子，否则路由跳转等会停止。
  * 如果要中断当前的导航要调用next(false)。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到from路由对应的地址。（主要用于登录验证不通过的处理）
  * 当然next可以这样使用，next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。意思是当前的导航被中断，然后进行一个新的导航。可传递的参数与router.push中选项一致。
  * 在beforeRouteEnter钩子中next((vm)=>{})内接收的回调函数参数为当前组件的实例vm，这个回调函数在生命周期mounted之后调用，也就是，他是所有导航守卫和生命周期函数最后执行的那个钩子。
  * next(error): (v2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

### 完整的流程

* 导航被触发
* 在失活的组件里调用 beforeRouteLeave 守卫
* 调用全局的 beforeEach 守卫
* 在重用的组件里调用 beforeRouteUpdate 守卫
* 在路由配置里调用 beforeEnter
* 解析异步路由组件
* 在被激活的组件里调用 beforeRouteEnter
* 调用全局的 beforeResolve 守卫
* 导航被确认
* 调用全局的 afterEach 钩子
* 触发 DOM 更新
* 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入

