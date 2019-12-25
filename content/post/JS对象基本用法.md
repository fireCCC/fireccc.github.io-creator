---
title: "JS对象基本用法"
date: 2019-10-20T19:34:36+08:00
draft: false
---

## 一 声明对象的两种语法

### 对象的定义
<font color="orange">无序的数据集合</font><br>
<font color="orange">键值对的集合</font>

### 对象的写法

`let obj = {'name': 'zwx', 'age': 18, }`
`let obj = new Object({'name': 'zwx'})`

### 细节

<font color="orange">键名是字符串，可以包含任意Unicode字符</font>

<font color="orange">如果书写键名时省略引号，就只能按标识符规则书写</font>

<font color="orange">‘省略引号’也改变不了‘键名是个字符串’这个事实</font>

## 二 如何删除对象的属性

`delete obj.xxx`或者`delete obj['xxx']`
<font color="orange">即可删除obj的xxx属性</font>

<font color="orange">不含属性名</font>
`'xxx' in obj === false`

<font color="orange">含有属性名，但是值为undefined</font>
`'xxx' in obj && obj.xxx === undefined`

## 三 如何查看对象的属性

### 查看所有属性

<font color="orange">查看自身所有属性</font>
`Object.keys(obj)`

<font color="orange">查看自身+共有属性</font>
`console.dir(obj)`

<font color="orange">判断一个属性是自身的还是共有的</font>
`obj.hasOwnProperty('toString')`

### 查看属性

<font color="orange">中括号语法：</font>
`obj['key']`
<font color="orange">点语法：</font>
`obj.key`

<font color="orange">优先使用`obj['key']`</font>

### 重点

<font color="red">obj.name 等价于 obj['name']</font>

### 题目

```js
let my = function(){
let list = ['name', 'age', 'gender']
let person = {
  name: 'zwx',
  age: 18,
  gender: 'male',
}
for (let i = 0; i < list.length; i++) {
  let name = list[i]
  //person.name 等价于 person['name']
  console.log('1.' + person.name)
  console.log('2.' + person[name])
}
}
```
![查看对象属性](/images/查看对象属性.png)


## 四 如何修改或增加对象的属性

### 修改或增加属性

<font color="orange">直接赋值</font><br>

1. `let obj = {name: 'zwx'} // name是字符串`

2. `obj.name = 'zwx' // name是字符串`

3. `obj['name'] = 'zwx'`

4. `obj['na' + 'me'] = 'zwx'`

5. `let key = 'name'; obj[key] = 'zwx'` 

<font color="orange">批量赋值</font><br>

`Object.assign(obj, {age: 18, gender: 'male'})`

### 修改或增加共有属性

<font color="orange">无法通过其中一个对象修改或增加他们的共有属性</font><br>

`let obj = {}, obj2 = {} // 共有原型上的toString`<br>
`obj.toString = 'xxx'` 只会修改自身的属性<br>
`obj2.toString` 还是原型上的方法

<font color="orange">我偏要修改或增加原型上的属性</font><br>

`obj.__proto__.toString = 'xxx' // 不推荐用__proto__`<br>
`Object.prototype.toString = 'xxx'`

<font color="red">一般来说，不要修改原型，会引起很多问题</font>

## 五 'name' in obj和obj.hasOwnProperty('name')的区别


`prop in object` 检查它（或其原型链）是否包含具有指定名称的属性

in右操作数必须是一个对象值。

如果使用 delete 运算符删除了一个属性，则 in 运算符对所删除属性返回 false。

如果你只是将一个属性的值赋值为undefined，而没有删除它，则 in 运算仍然会返回true。

如果一个属性是从原型链上继承来的，in 运算符也会返回 true。

`obj.hasOwnProperty('name')` 方法会返回一个布尔值，指示对象自身属性中（非继承属性）是否具有指定的属性，
如果 object 具有带指定名称的属性，则 hasOwnProperty 方法返回 true，否则返回 false。此方法不会检查对象原型链中的属性；该属性必须是对象本身的一个成员。

```js
let father = {money: 9999}
let son = Object.create(father)
son.name = 'son'
console.log('1.检查它（或其原型链）是否包含具有指定名称的属性')
console.log('money' in son) //true
console.log('2.对象自身属性中（非继承属性）是否具有指定的属性')
console.log(son.hasOwnProperty('money')) //false
```
![方法区别](/images/方法区别.png)