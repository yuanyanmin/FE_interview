# 第6题： less等预处理器和style对比

less 是CSS预处理器，它使用变量、混合、操作符、函数等功能，可以轻松地编写可维护的CSS。

## 变量

```
@width: 100px;
@height: 100px;

.box {
  width: @width;
  height: @height;
}
```

## 混合

```
.rounded-box {
  border-radius: 5px;
}

.box {
 .rounded-box();
  background-color: #f00;
}
```

## 运算

```
@width: 100px;
@height: 100px;
  
.box {
  width: @width + 10px;
  height: @height / 2;
}
```

## 嵌套

```
.box {
  .header {
    width: 100%;
    height: 50px;
  }
}
```



