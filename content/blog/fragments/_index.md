---
linkTitle: 节流与防抖
title: 节流与防抖
type: blog
weight: 7
---

<br />

``` 
节流(throttle),顾名思义就是对流量进行节制;
也就是说，当流量大的时候要对其进行一定节制与控制，一般场景如客户端发起请求的时候，一段时间内只允许发送一条；
服务端接收请求时，一段时间内只接收一条。

防抖(debounce),顾名思义就是防止抖动;
也就是说在一个接连的变化中，不是立马响应变化，而是等待接连变化的抖动结束后再进行相应操作。
一个明显的例子：当要对用户输入的查询内容进行查询响应时，由于用户处于一个连续输入的过程中，
我们以一段时间内没有输入作为用户输入的结束信号，进行查询；
这样避免了在连续输入中连续响应造成的资源浪费和连续响应带来的卡顿或者说抖动。
```

- 节流代码示例

```javascript
function throttle(func, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = new Date().getTime();
    if (now - lastCall >= delay) {
      func(...args);
      lastCall = now;
    }
  };
}

// 用法示例
const throttledFunction = throttle(function () {
  console.log("Throttled function is called");
}, 1000);

// 调用节流函数
throttledFunction();
````

- 防抖代码示例
  
```javascript
function debounce(fn, wait, immediate) {
  let timeout, result;
  return function() { 
    let args = arguments   // 接受event等默认传递的参数
    clearTimeout(timeout)
    if (immediate) {  //如果第一次需要立即执行
      let callNow = !timeout
      timeout = setTimeout(() => { //立即执行后当n秒内触发事件才能再次执行。
        timeout = null
      }, wait)
      if (callNow) result = fn.apply(this, args)
    } else {
      timeout = setTimeout(() => { //箭头函数解决this指向正确
        fn.apply(this, args)
      }, wait)
    }
    return result
  }
}

// 用法示例
const debouncedFunction = debounce(function () {
  console.log("Debounced function is called");
}, 1000);

// 调用防抖函数
debouncedFunction();
```
