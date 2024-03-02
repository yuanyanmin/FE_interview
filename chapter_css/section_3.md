# Flex

## Flex 布局是什么

弹性布局，用来为盒装模型提供最大的灵活性

```
.box {
  display: flex;
}
```
## 容器的属性

* flex-direction

  > 决定主轴的方向，即项目的排列方向
  ```
  .box {
    flex-direction：row | row-reverse | column | column-reverse
  }
  ```
  * row： 水平方向，起点在左端
  * row-reverse：水平方向，起点在右端
  * column：垂直方向，起点在上沿
  * column-reverse：垂直方向，起点在下沿

* flex-wrap

  > 若一条轴线排不下如何换行
  ```
  .box {
    flex-wrap：nowrap | wrap | wrap-reverse
  }
  ```
  * nowrap： 不换行
  * wrap：换行，第一行在上方
  * wrap-reverse：换行，第一行在下方

* flex-flow

  > 是 flex-direction 与 flex-wrap 的简写形式，默认为 row nowrap
  ```
  .box {
    flex-flow：<flex-direction> || <flex-wrap>
  }
  ```

* justify-content

  > 主轴上的对齐方式
  ```
  .box {
    justify-content: flex-start | flex-end | center | space-between | space-around
  }
  ```
  * flex-start：左对齐
  * flex-end：右对齐
  * center：居中
  * space-between：两端对齐，项目之间的间隔都相等
  * space-around：每个项目两侧的间隔相等

* align-items

  > 交叉轴上如何对齐
  ```
  .box {
    align-items: flex-start | flex-end | center | baseline | stretch
  }
  ```
  * flex-start：交叉轴的起点对齐
  * flex-end：交叉轴的终点对齐
  * center：交叉轴的中点对齐
  * baseline：项目的第一行文字的基线对齐
  * stretch：若项目未设置高度，则将占满整个容器的高度

* align-content

  > 多根轴线的对齐方式
  ```
  .box {
    align-content：flex-start | flex-end | center | space-between | space-around | stretch
  }
  ```
  * flext-start：与交叉轴的起点对齐
  * flex-end：与交叉轴的终点对齐
  * center：与交叉轴的中点对齐
  * space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
  * space-around：没跟轴线两侧的间隔都相等
  * stretch：轴线占满整个交叉轴
 
## 项目的属性

* order

  > 定义项目的排序顺序，数值越小越靠前，默认为0
  ```
  .item {
    order: <integer>
  }
  ```

* flex-grow

  > 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
  ```
  .item {
    flex-grow：<number>
  }
  ```
  > 如果所有项目的 flex-grow 属性都为1，则他们将等分剩余空间。如果一个项目的 flex-grow 属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍


* flex-shrink

  > 定义项目的缩小比例，默认为1，即如果空间不足，则项目将缩小
  ```
  .item {
    flow-shrink: <number>
  }
  ```
  > 如果所有项目的 flex-shrink 属性都为1，则空间不足时，都将等比例缩小。如果一个项目的 flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小

  > 负值对该属性无效
  

* flex-basis
  
  > 定义了在分配多余的剩余空间之前，项目占据的主轴空间。默认为auto,即项目本来大小
  ```
  .item {
    flex-basis: <length> | auto
  }
  ```
  > 它可以设为跟 width 和 height 属性一样的值，则项目将占据固定空间

* flex

  > 是 flex-grow、flex-shrink 和 flex-basis的简写，默认为 0 1 auto。
  ```
  .item {
    flex: none | [<'flex-grow'> <'flex-shrink'>? || <'flex-basis>]
  }
  ```
  > 该属相有两个快捷键：auto（1 1 auto）和 none （ 0 0 auto）
  > flex: 1 （1 1 0%）

* align-self

  > 允许单个项目有与其他项目不一样的对齐方式，可以覆盖 aligin-items 属性，默认为 auto,表示继承父元素的 align-items 属性。

  ```
  .item {
    align-self：auto | flex-start | flex-end | center | baseline | stretch
  }
  ```

[flex布局——flex简写](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)