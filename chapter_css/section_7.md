# 移动端、PC适配


## 了解viewport

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">

```
将页面的宽度设置为设置的视窗宽度后，即可进行移动端适配工作了。

## 适配方案
### 媒体查询
```
@media screen and (min-width: 600px) and (max-width: 900px) {
  body {
    color: blue;
  }
}

```
### rem布局

rem作用于非根元素时，相对于根元素字体大小；rem作用于根元素字体大小时，相对于其出初始字体大小

### 开发PC端和移动端两套代码
### flex布局
### 百分比
### vw、vh单位
### 两套代码的切换

