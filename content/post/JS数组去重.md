---
title: "JS数组去重"
date: 2019-12-17T13:34:36+08:00
draft: false
---

著名面试题：
如何实现数组去重？
假设有数组 array = [1,5,2,3,4,2,3,1,3,4]
你要写一个函数 unique，使得
unique(array) 的值为 [1,5,2,3,4]
也就是把重复的值都去掉，只保留不重复的值。

一个答案不使用 Set 实现
我们可以先使用sort()将数组进行排序
然后比较相邻元素是否相等，从而排除重复项
```js
let arr = [1,5,2,3,4,2,3,1,3,4]
function unique (array) {
    let arr = array.sort()
    let result = [arr[0]]
    for (let i = 1; i < arr.length; i++) {
        arr[i] !== arr[i-1] && result.push(arr[i])
    }
    return result
}
unique(arr)
```
可以使用 Set
```js
let arr = [1,5,2,3,4,2,3,1,3,4]
function unique (array) {
  return Array.from(new Set(arr))
  //或者[...new Set(arr)]
}
unique(arr)
```
上面方案的缺点是不支持对象去重，数组中出现对象的时候会出现问题

我们可以使用 Map / WeakMap 以支持对象去重。
#### Map去重
```js
let arr = [
    {key: 'key', value: 'value'},
    {key: 'key', value: 'value'},
    {value: 'value', key: 'key'},
    {name:'name'}]
function mySort(obj){
    return Object.keys(obj)
    .sort().reduce((a, v) => {
    a[v] = obj[v]
    return a
    }, {})
}

function unique (array) {
  let arr = array
  //console.log(arr)
  let hashMap = new Map()
  let result = new Array()
  for (let i = 0; i < arr.length; i++) {
    let item = arr[i]
    item = mySort(item)
    item = JSON.stringify(item)
    if(hashMap.has(item)) { 
      hashMap.set(item, 2)
    } else {  
      hashMap.set(item, 1)
      result.push(arr[i])
    }
  } 
  //console.log(hashMap)
  return result
}
unique(arr)
```
遇到对象中嵌套对象会出现问题
<hr>

#### WeakMap去重

```js
let ob1 = {key: 'key', value: 'value'}
let ob2 = {value: 'value', key: 'key'}
let arr = [ob1,ob1,ob2]
function unique(array) {
  let wm = new WeakMap()
  for(let i = 0; i < array.length; i++){
    if(!wm.has(array[i])){
      wm.set(array[i], i)
    }
  }
  return wm
}
unique(arr)
```
结果如下
```js
WeakMap {{…} => 0, {…} => 2}
[[Entries]]
0: {Object => 0}
key: {key: "key", value: "value"}
value: 0
1: {Object => 2}
key: {value: "value", key: "key"}
value: 2
__proto__: WeakMap
```

遇到let arr = [<br>
    {key: 'key', value: 'value'},<br>
    {key: 'key', value: 'value'},<br>
    {value: 'value', key: 'key'},<br>
    {name:'name'}]<br>
会出现问题
