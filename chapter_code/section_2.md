# 第 2 题：实现防抖节流

## 为什么会用到防抖节流

* 当函数绑定一些持续触发的事件如：resize、scroll、mousemove、键盘输入，多次快速click等等。
* 如果每次触发都要执行一次函数，会带来性能下降，资源请求太频繁的问题

## 防抖

### 介绍

所谓防抖，就是指触发事件后 n 秒后才执行函数，如果在 n 秒内又触发了事件，则会重新计算函数执行时间

### 应用防抖

#### 非立即执行版本

```
 let num = 1;
 const content = document.getElementById('content');
 // 功能函数
 function count() {
  this.innerHTML = num++;
 };
    
    
content.onclick = debounce(count, 1000);

// 防抖函数，非立即执行版本
 function debounce(func, wait) {
  let timeout;
  return function () {
    const context = this;
    const args = [...arguments];
    if (timeout) clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(context, args)
    }, wait);
  }
}

```

#### 立即执行版

触发事件后函数会立即执行，n 秒内触发事件不会执行功能函数下一次调用，n秒后再次触发才会再次执行功能函数

```
// 防抖函数，立即执行版本
function debounce(func, wait) {
  let timeout;
  return function () {
    const context = this;
    const args = [...arguments];
    if (timeout) clearTimeout(timeout);
    const callNow = !timeout;
    timeout = setTimeout(() => {
      timeout = null;
    }, wait);
    if (callNow) func.apply(context, args);
  };
}
```

#### 合并
```
// 防抖函数，合并版本，immediate为true时为立即执行
function debounce(func, wait, immediate) {
  let timeout;
  return function () {
    const context = this;
    const args = [...arguments];
    if (timeout) clearTimeout(timeout);
    if (immediate) {
      const callNow = !timeout;
      timeout = setTimeout(() => {
        timeout = null;
      }, wait)
      if (callNow) func.apply(context, args)
    }
    else {
      timeout = setTimeout(() => {
        func.apply(context, args)
      }, wait);
    }
  }
}

```

## 节流

### 介绍

所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。节流会稀释函数的执行频率

### 应用节流

#### 时间戳

```
function throttle(func, wait) {
    var previous = 0;
    return function() {
        let now = Date.now();
        let context = this;
        let args = arguments;
        if (now - previous > wait) {
            func.apply(context, args);
            previous = now;
        }
    }
}
content.onmousemove = throttle(count,1000);
```

#### 定时器
```
function throttle(func, wait) {
    let timeout;
    return function() {
        let context = this;
        let args = arguments;
        if (!timeout) {
            timeout = setTimeout(() => {
                timeout = null;
                func.apply(context, args)
            }, wait)
        }

    }
}

```

**参考链接**

[防抖和节流](https://juejin.cn/post/6958704784626941982)