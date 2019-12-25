---
title: "Promise介绍"
date: 2019-12-18T13:34:36+08:00
draft: false
---

### Promise 的用途

Promise是ES6新增的语法，解决了回调地狱的问题

Promise 对象用于表示一个异步操作的最终完成 (或失败), 及其结果值.

Promise 对象是一个代理对象（代理一个值），被代理的值在Promise对象创建时可能是未知的。它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers）。 这让异步方法可以像同步方法那样返回值，但并不是立即返回最终执行结果，而是一个能代表未来出现的结果的promise对象

一个 Promise有以下几种状态:

pending: 初始状态，既不是成功，也不是失败状态。
fulfilled: 意味着操作成功完成。
rejected: 意味着操作失败。
pending 状态的 Promise 对象可能会变为fulfilled 状态并传递一个值给相应的状态处理方法，也可能变为失败状态（rejected）并传递失败信息。当其中任一种情况出现时，Promise 对象的 then 方法绑定的处理方法（handlers ）就会被调用（then方法包含两个参数：onfulfilled 和 onrejected，它们都是 Function 类型。当Promise状态为fulfilled时，调用 then 的 onfulfilled 方法，当Promise状态为rejected时，调用 then 的 onrejected 方法， 所以在异步操作的完成和绑定处理方法之间不存在竞争）。

因为 Promise.prototype.then 和  Promise.prototype.catch 方法返回promise 对象， 所以它们可以被链式调用。

### 创建一个 new Promise
```js
function fn(){
    return new Promise((resolve, reject)=>{
        成功时调用 resolve(数据)
        失败时调用 reject(错误)
    })
}
fn().then(success, fail).then(success2, fail2)
```
### 使用 Promise.prototype.then
```js
var p1 = new Promise((resolve, reject) => {
  resolve('成功！');
  // or
  // reject(new Error("出错了！"));
});

p1.then(value => {
  console.log(value); // 成功！
}, reason => {
  console.error(reason); // 出错了！
});
```
then函数会返回一个Promise实例，并且该返回值是一个新的实例而不是之前的实例
当一个值只是从一个 then 内部返回时，它将等价地返回 Promise.resolve(<由被调用的处理程序返回的值>)。
```js
var p2 = new Promise(function(resolve, reject) {
  resolve(1);
});

p2.then(function(value) {
  console.log(value); // 1
  return value + 1;
}).then(function(value) {
  console.log(value + ' - A synchronous value works');
  //2 - A synchronous value works
});

p2.then(function(value) {
  console.log(value); // 1
});
```
### 使用 Promise.all

Promise.all([promise1, promise2]).then(success1, fail1)<br>
promise1和promise2都成功才会调用success1

```js
var p1 = Promise.resolve(3);
var p2 = 1337;
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
}); 

Promise.all([p1, p2, p3]).then(values => { 
  console.log(values); // [3, 1337, "foo"] 
});
```
### 使用 Promise.race

Promise.race([promise1, promise2]).then(success1, fail1)<br>
方法返回一个 promise，一旦迭代器中的某个promise解决或拒绝，返回的 promise就会解决或拒绝。

```js
var promise1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, 'one');
});

var promise2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"
```

