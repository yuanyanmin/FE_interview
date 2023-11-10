# 第 2 题：Promise

Promise 对象表示异步操作最终的完成（或失败）以及其结果值。

### 描述

一个 Promise 必然处于以下几种状态之一：

* 待定（pending）：初始状态，既没有被兑现，也没有被拒绝。
* 已兑现（fulfilled）：意味着操作成功完成。
* 已拒绝（rejected）：意味着操作失败。

一个待定的 Promise 最终状态可以是已兑现并返回一个值，或者是已拒绝并返回一个原因（错误）。当其中任意一种情况发生时，通过 Promise 的 then 方法串联的处理程序将被调用。如果绑定相应处理程序时 Promise 已经兑现或拒绝，这处理程序将被立即调用，因此在异步操作完成和绑定处理程序之间不存在竞态条件。

### 链式调用

* .then

> 接受两个参数，第一个是 Promise 兑现时的回调函数，第二个是 Promise 拒绝时候的回调函数。每个 .then 返回一个新生成的 Promise 兑现，这个对象也被用于链式调用

**语法**

```
then(onFulfilled)
then(onFulfilled, onRejected)
```

**参数**

```
onFulfilled：一个在此 Promise 对象被兑现时异步执行的函数。它的返回值将成为 then() 返回的 Promise 对象的兑现值。

onRejected：一个在此 Promise 对象被拒绝时异步执行的函数。它的返回值将成为 catch() 返回的 Promise 对象的兑现值。
```

**返回值**

立即返回一个新的 Promise 对象，该对象始终处于待定状态，无论当前 Promise 对象的状态如何。


```
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});

myPromise
  .then(handleFulfilledA, handleRejectedA)
  .then(handleFulfilledB, handleRejectedB)
  .then(handleFulfilledC, handleRejectedC);
```

* .catch

> Promise 实例的 catch() 方法用于注册一个在 promise 被拒绝时调用的函数。它会立即返回一个等效的 Promise 对象，这可以允许你链式调用其他 promise 的方法。此方法是 Promise.prototype.then(undefined, onRejected) 的一种简写形式

**语法**

```
catch(onRejected)
```

**返回值**

返回一个新的 Promise，无论当前的 promise 状态如何，这个新的 promise 在返回时总是处于待定状态。如果 onRejected 方法抛出了一个错误或者返回了一个被拒绝的 promise，那么这个新的 promise 也会被拒绝；否则它最终会被兑现

```
myPromise
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);

```

* .finally

> Promise 实例的 finally() 方法用于注册一个在 promise 敲定（兑现或拒绝）时调用的函数。它会立即返回一个等效的 Promise 对象，这可以允许你链式调用其他 promise 方法

**语法**

```
finally(onFinally)
```

**参数**

```
onFinally: 一个当 promise 敲定时异步执行的函数。它的返回值将被忽略，除非返回一个被拒绝的 promise。调用该函数时不带任何参数
```

* .all

> Promise.all() 静态方法接受一个 Promise 可迭代对象作为输入，并返回一个 Promise。当所有输入的 Promise 都被兑现时，返回的 Promise 也将被兑现（即使传入的是一个空的可迭代对象），并返回一个包含所有兑现值的数组。如果输入的任何 Promise 被拒绝，则返回的 Promise 将被拒绝，并带有第一个被拒绝的原因


* .allSettled

> Promise.allSettled() 静态方法将一个 Promise 可迭代对象作为输入，并返回一个单独的 Promise。当所有输入的 Promise 都已敲定时（包括传入空的可迭代对象时），返回的 Promise 将被兑现，并带有描述每个 Promise 结果的对象数组。


**all 与 allSettled 对比**

> Promise.all() 方法会在任何一个输入的 Promise 被拒绝时立即拒绝。相比之下，Promise.allSettled() 方法返回的 Promise 会等待所有输入的 Promise 完成，不管其中是否有 Promise 被拒绝。如果你需要获取输入可迭代对象中每个 Promise 的最终结果，则应使用 allSettled() 方法。



### 面试题

* 输出

```
new Promise((resolve, reject) => {
    reject(1)
    console.log(2)
    resolve(3)
}).then(res => console.log(res))

// 2
// Uncaught (in promise) 1
```

* promise 实现并发 每次5个

> 核心思路：控制并发数量，关键点就是利用promise,当一个请求完成后,去发起下一个请求。

coding思路：

* 由于要完成一个继续下一个，保证同时有limit个在同时执行。所以我们需要一个递归的执行函数run；
* 首先在函数执行时，需要将执行中的任务按顺序填满，数量为限制并发数
* 执行函数中，需要在执行完成后，判断是否有下一个待执行任务；于是我们声明变量i,用来计数，与最终需要执行的总数比较判断是否需要递归执行下一个任务


```
const fn = url => {
    // 实际场景这里用axios等请求库 发请求即可 也不用设置延时
    return new Promise(resolve => {
        setTimeout(() => {
            console.log('完成一个任务', url, new Date());
            resolve({ url, date: new Date() });
        }, 1000);
    })
};

function limitQueue(urls, limit) {
    // 完成任务数
    let i = 0;
    // 填充满执行队列
    for (let excuteCount = 0; excuteCount < limit; excuteCount++) {
        run();
    }
    // 执行一个任务
    function run() {
        // 构造待执行任务 当该任务完成后 如果还有待完成的任务 继续执行任务
        new Promise((resolve, reject) => {
            const url = urls[i];
            i++;
            resolve(fn(url))
        }).then(() => {
            if (i < urls.length) run()
        })
    }
};



const urls = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15];

(async _ => {
    await limitQueue(urls, 4);
})()

```



