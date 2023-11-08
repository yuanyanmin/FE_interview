# 第 6 题：跨域

### 什么是跨域

同源指的是两个 URL 的协议、域名、端口一致，反之，则是跨域。

出现跨域的根本原因：浏览器的同源策略不允许非同源的 URL 之间进行资源的交互

### JSONP

##### 原理

script标签是允许跨域请求的。利用这个特性，客户端使用script代替XHR（ XMLHttpRequest）发起跨域请求。

事先定义一个用于获取跨域响应数据的回调函数，并通过没有同源策略限制的script标签发起一个请求（将回调函数的名称放到这个请求的query参数里），然后服务端返回这个回调函数的执行，并将需要响应的数据放到回调函数的参数里，前端的script标签请求到这个执行的回调函数后会立马执行，于是就拿到了执行的响应数据

##### 实现方案

JSONP实现跨域请求的原理就是动态创建script标签，利用“src”不受同源策略约束的性质来实现跨域获取数据。
菜鸟教程提供了一个链接，我们可以用它练练手~
链接：https://www.runoob.com/try/ajax/jsonp.php?jsoncallback=callbackFunction

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>JSONP 实例</title>
</head>
<body>
<script>
  function callbackFunction(data){
    console.log("i got it")；
    console.log(data); // ['customername1', 'customername2']
  }
</script>
<script>
  window.onload=function(){
    var script=document.createElement("script");
    script.src="https://www.runoob.com/try/ajax/jsonp.php?jsoncallback=callbackFunction";
    document.head.append(script)；
  }
</script>
</body>
</html>

```

##### 总结
* ajax和jsonp本质上是不同的东西。ajax的核心是通过XmlHttpRequest获取非本页内容，而jsonp的核心则是动态添加script标签来调用服务器提供的js脚本。
* JSONP方案属于客户端直接请求，不存在二次请求的问题；但是，不足是script标签只能发起get请求


### 反向代理

修改 nginx 配置

### CORS

```
// 这就全是服务端的工作了，主要的三个参数
Access-Control-Allow-Origin： 服务器可接受的请求来源
Access-Control-Request-Method： 服务器实际请求所使用的 HTTP 方法
Access-Control-Request-Headers： 服务器实际请求所携带的自定义首部字段。
```