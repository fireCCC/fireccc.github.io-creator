---
title: "JS函数的执行时机"
date: 2019-10-26T19:34:36+08:00
draft: false
---

## 一 为什么如下代码会打印 6 个 6

```js
let i = 0
for(i = 0; i < 6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```

当setTimeout()的毫秒数设置为0的时候，是要先执行完函数调用栈中的代码，然后立即调用定时器。

因为定时器都被放在了一个队列中，等待上下文的可执行代码运行完毕后，才开始运行定时器，也就是定时器才刚开始计时。代码中声明一个let全局变量i，然后在for循环中改变i，所以在定时器的方法执行的时候，变量i已经变成了6，所以输出的全部是6。

## 二 让上面代码打印 0、1、2、3、4、5 的方法

```js
for(let i = 0; i < 6; i++){
  setTimeout(() => {
    console.log(i)
  }, 0)
}
```

![setTimeout1](/images/setTimeout1.png)

利用ES6 let命令声明变量块级作用域的概念，和前面for循环使用全局变量i不同，当前的i只在本轮循环有效， 所以每一次循环的i其实都是一个新的变量，所以5个setTimeout回调函数虽然都引用了变量i,但实际上这5个i是独立的，仅在自己的块级作用域内有效

## 三 除了使用 for let 配合，还有什么其他方法可以打印出 0、1、2、3、4、5

```js
let i
for (i = 0; i < 6; i++) {
  // j = i
  !(function(j) { 
    setTimeout(() => {
      console.log(j)
    }, 0);
  })(i);
}
```

![setTimeout2](/images/setTimeout2.png)

这里通过立即执行函数创造了n个不同的函数作用域，给setTimeout传入n个不同的参数，所以就可以打印出0、1、2、3、4、5
此外，我们还可以

```js
let output = function(i) {
  setTimeout(function() {
    console.log(i);
  }, 0);
};
let i
for (i = 0; i < 6; i++) {
  output.call(undefined, i); // 这里传过去的 i 值被复制，而不是引用
}
```

![setTimeout3](/images/setTimeout3.png)

原理和上面相似，也是创造了n个不同的函数作用域，给setTimeout传入了n个不同的参数

此外我们还可以利用setTimeout第三个参数

```js
let i
for (i = 0; i < 6; i++) {
  setTimeout(function(i) {
    console.log(i);
  }, 0, i);
}
```

![setTimeout4](/images/setTimeout4.png)
