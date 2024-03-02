# 第 5 题：axios

* 创建 Axios 实例

```
let instance = axios.create({
	headers:{
		'content-type':'application/x-www-form-urlencoded'
	}
})

```

* 请求拦截

这个拦截器会在你发送请求之前运行，此处请求拦截器的功能是：每次请求前去判断是否有token，如果token存在则在请求头加上这个token,后台会判断这个token是否过期。

```
// http request拦截器
instance.interceptors.request.use(
	config=>{
		const token = localStorage.getItem('token');//此处用这个localStorage 也可以用store，因为一般登录后都会将登录信息存储到vuex中，但是如果不存储到localsStorage的话，当前页面刷新时，vuex中的内容会消失。
		if(token){
			config.headers.authorization = token //请求头加上token
		}
		return config
	},err=>{
		return Promise.reject(err);
	}
)

```

* 响应拦截器

这个拦截器会在拿到后台接口返回的数据后，进行响应处理。比如数据正确返回是code是200，如果code是500,则需要拦截一下。

```
instance.interceptors.response.use(
	response => {
		//拦截响应，做统一处理
		if(response.data.code){
			switch(response.data.code){
				case 1002:
				store.state.isLogin = false;//store中有个isLogin表示登录状态，这个视具体情况而定
				router.replace({
					path:'login',
					query:{
						redirect:router.currentRoute.fullPath	
					}
				})	
			}
		}
		return response
	},
	//接口错误状态处理，也就是说无响应时的处理
	error =>{
		return Promise.reject(error.response.status);//接口返回的错误信息
	}
)

```


* 进入error 情况

```
Axios.interceptors.request.use（config={
    //这里会最先拿到你的请求配置
},err=>{
  // 这里极少情况会进来，暂时没有找到主动触发的方法，估计只有浏览器不兼容时才会触发，欢迎后面同学补充
  // 看了几个GitHub的issue，有人甚至提出了这个方法是不必要的（因为没有触发的场景），不过还是建议大家按照官方的写法，避免不必要的错误
  // 进来之后没法发起请求
}）

Axios.interceptors.response.use（res ={
  //这里会最先拿到你的response
  // 只有返回的状态码是2xx，都会进来这里
},err=>{
  // 目前发现三种情况会进入这里：
  // 1. http状态码非2开头的都会进来这里，如404,500等
  // 2. 取消请求也会进入这里，CancelToken，可以用axios.isCancel(err)来判断是取消的请求
  // 3. 请求运行有异常也会进入这里，如故意将headers写错：axios.defaults.headers = '123',或者在request中有语法或解析错误也会进入这里
  // 进入这里意味着请求失败，axios会进入catch分支
}）
```