# 第 14 题：防抖与节流

## 防抖

> 在触发事件后的 n 秒后才执行函数，若在 n 秒内又触发了事件，则会重新计算函数执行时间

```
// 不需要传参 demo

const btn = document.getElementById("btn");
btn.addEventListener("click", debounce(send, 1000))

function send(){
  console.log('执行', this);
}
```

```
// 需要绑定 this，与传参的 demo

// 假设有一个全局的计数器
let globalCount = 0;

// 原始函数，接收参数并依赖this
function increment(value, by) {
  console.log('Current count:', this.globalCount);
  this.globalCount += by;
  console.log('Incremented by', value, ', new count:', this.globalCount);
}

// 创建一个闭包环境用于存储this上下文
const counter = {
  globalCount,
  debouncedIncrement: null
};

// 初始化防抖函数，并通过bind绑定this
counter.debouncedIncrement = debounce(increment.bind(counter), 1000);

// 触发事件时调用防抖后的函数
document.getElementById('incrementBtn').addEventListener('click', () => {
  counter.debouncedIncrement(1, 1); // 假设每次点击增加1
});

```

```
// 非立即执行版本

function debounce(fn, wait) {
  let timer = null
  return function(...args) {
    let _this = this
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(function() {
      fn.apply(_this, args)
    }, wait)
  }
}
```

```
// 立即执行版本
function debounce(fn, wait, immediate = false) {
  let timer = null

  // 设置变量用来记录是否立即执行
  let isImmediateInvoke = false

  return function(...args) {
    let _this = this
    if (timer) {
      clearTimeout(timer)
    }

    if(!isImmediateInvoke && immediate) {
      fn.apply(_this, args)
      isImmediateInvoke = true
    }
    timer = setTimeout(function() {
      fn.apply(_this, args)
      isImmediateInvoke = false
    }, wait)
  }
}
```

## 节流

> 指连续触发事件但是在 n 秒只执行一次函数。会稀释函数的执行频率

```
// 节流-setTimeout
function throttle(fn, delay) {
  let timer = null
  return function(...args) {
    let _this = this

    if (timer) {
      return
    }

    timer = setTimeout(function() {
      fn.apply(_this, args)
      clearTimeout(timer)
      timer = null
    }, delay)
  }
}
```

```
// 节流-时间戳对比
function throttle(fn, delay) {
  let start = 0
  return function(...args) {
    let now = Date.now()

    if (now - start > delay) {
      start = now
      fn.apply(this, args)
    }
  }
}
```