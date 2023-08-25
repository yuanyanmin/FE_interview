# 第 2 题：div 水平垂直居中

## 第一种 定宽高

* 定位 + 负 margin

```
.element {
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -250px;
  margin-top: -250px;
  width: 500px;
  height: 500px;
  background-color: red;
}
```

* 定位 + margin: auto [原理](https://www.zhangxinxu.com/wordpress/2013/11/margin-auto-absolute-%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D-%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD/)

```
.element {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  width: 500px;
  height: 500px;
  background-color: red;
}
```

## 第二种 不定宽高

* 定位 + transform

```
.element {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 500px;
  height: 500px;
  background-color: red;
}
```

* flex

```
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 600px;
  height: 600px;
  background-color: greenyellow;
}
.element {
  width: 500px;
  height: 500px;
  background-color: red;
}
```
