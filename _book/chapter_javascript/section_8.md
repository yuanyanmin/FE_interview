# 第 8 题：输入URL发生了什么

### 概述

1、浏览器的地址栏输入URL并按下回车。
2、浏览器查找当前URL是否存在缓存，并比较缓存是否过期。
3、DNS解析URL对应的IP。
4、根据IP建立TCP连接（三次握手）。
5、HTTP发起请求。
6、服务器处理请求，浏览器接收HTTP响应。
7、渲染页面，构建DOM树。
8、关闭TCP连接（四次挥手）


### 构建DOM树

1、浏览器会从上到下解析文档
2、遇见HTML标记，调用HTML解析器解析为对应的token(一个token就是一个标签文本的序列化)并构建DOM树(就是一块内存，保存着tokens，建立他们之间的关系)
3、遇见style/link标记调用相应解析器处理CSS标记，并构建出CSS样式树
4、遇见script标记，调用JavaScript引擎处理script标记，绑定事件，修改DOM树/CSS树等
5、将DOM树与CSS合并成一个渲染树
6、根据渲染树来渲染，以计算每个节点的几何信息(这一过程需要依赖GPU)
7、最终将各个节点绘制在屏幕上


### 重绘回流
1、重绘：重绘是一个元素外观的改变所触发的浏览器行为，例如改变outline、背景色等属性。浏览器会根据元素的新属性重新绘制，使元素呈现新的外观。重绘不会带来重新布局，所以并不一定伴随重排。
2、回流：渲染对象在创建完成并添加到渲染树时，并不包含位置和大小信息。计算这些值的过程称为布局或重排，或回流
3、"重绘"不一定需要"重排"，比如改变某个网页元素的颜色，就只会触发"重绘"，不会触发"重排"，因为布局没有改变。
4、"重排"大多数情况下会导致"重绘"，比如改变一个网页元素的位置，就会同时触发"重排"和"重绘"，因为布局改变了。

### 触发重绘的属性
 color、background、outline-color、border-style、background-image、outline、border-radius、background-position、outline-style、visibility、background-repeat、outline-width、text-decoration、background-size、box-shadow


### 触发回流的属性
 width、top、text-align、height、bottom、overflow-y、padding、left、font-weight、margin、right、overflow、display、position、font-family 、border-width、float、line-height、border、clear、vertival-align、min-height、white-space

### 常见触发重绘回流的行为

1、当你增加、删除、修改 DOM 结点时，会导致 Reflow , Repaint。
2、当你移动 DOM 的位置
3、当你修改 CSS 样式的时候。
4、当你Resize窗口的时候（移动端没有这个问题，因为移动端的缩放没有影响布局视口)
5、当你修改网页的默认字体时。
6、获取DOM的height或者width时，例如clientWidth、clientHeight、clientTop、clientLeft、offsetWidth、offsetHeight、offsetTop、offsetLeft、scrollWidth、scrollHeight、scrollTop、scrollLeft、scrollIntoView()、scrollIntoViewIfNeeded()、getComputedStyle()、getBoundingClientRect()、scrollTo()

### 针对重绘回流的优化方案

1、元素位置移动变换时尽量使用CSS3的transform来代替top，left等操作
2、不要使用table布局
3、将多次改变样式属性的操作合并成一次操作
4、利用文档素碎片（documentFragment），vue使用了该方式提升性能
5、动画实现过程中，启用GPU硬件加速：transform:tranlateZ(0)
6、为动画元素新建图层，提高动画元素的z-index
7、编写动画时，尽量使用requestAnimationFrame

