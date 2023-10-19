# 第 4 题：双飞翼布局与圣杯布局

header 和 footer ，高度固定
中间的是一个三栏布局，两侧宽度固定不变，中间部分自动填充整个区域

## 圣杯布局

```
<div id="header"></div>
<div id="container">
  <div id="center" class="column"></div>
  <div id="left" class="column"></div>
  <div id="right" class="column"></div>
</div>
<div id="footer"></div>

```


```
body {
  min-width: 550px;
}

#container {
  padding-left: 200px; 
  padding-right: 150px;
}

#container .column {
  float: left;
}

#center {
  width: 100%;
}

#left {
  width: 200px; 
  margin-left: -100%;
  position: relative;
  right: 200px;
}

#right {
  width: 150px; 
  margin-right: -150px; 
}

#footer {
  clear: both;
}

```

## 双飞翼布局

```
<body>
  <div id="header"></div>
  <div id="container" class="column">
    <div id="center"></div>
  </div>
  <div id="left" class="column"></div>
  <div id="right" class="column"></div>
  <div id="footer"></div>
<body>

```

```
body {
  min-width: 500px;
}

#container {
  width: 100%;
}

.column {
  float: left;
}
        
#center {
  margin-left: 200px;
  margin-right: 150px;
}
        
#left {
  width: 200px; 
  margin-left: -100%;
}
        
#right {
  width: 150px; 
  margin-left: -150px;
}
        
#footer {
  clear: both;
}

```

## 总结

#### 相同之处

* 布局类似，都实现三栏布局
* 使用 float 浮动向左脱离文档流，让左中右三列浮动，通过外边距行成三列布局

#### 不同之处

* 圣杯布局是通过 float 搭建布局 + margin 使三列布局到同一行上 + relative 相对定位调整位置
* 双飞翼布局是通过 float + margin

* 圣杯布局是通过给外部容器加 padding，通过相对定位把两边定位出来
* 双飞翼布局是靠在中间这层外面套一层 div 加 padding 将内容挤出来

## 参考链接

[链接](https://www.cnblogs.com/seanxushuo/p/11415316.html)