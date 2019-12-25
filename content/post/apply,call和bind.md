---
title: "apply,call和bind"
date: 2019-12-07T19:34:36+08:00
draft: false
---

## 一 apply

### 语法
func.apply(thisArg, [argsArray])
#### 参数
* thisArg
可选的。在 func 函数运行时使用的 this 值。
（注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。）
* argsArray
可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。
如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。
(从ECMAScript 5 开始可以使用类数组对象。)
#### 返回值
调用这个指定this值(thisArg)和参数([argsArray])的函数(func)
调用函数的结果即它的返回值

```js
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2

//结果
//> 7
//> 2
```
### 示例
<font color="#00796B">1. 用 apply 将数组添加到另一个数组</font><br>
我们可以使用push将元素追加到数组中。并且，因为push接受可变数量的参数，我们也可以一次推送多个元素。但是，如果我们传递一个数组来推送，它实际上会将该数组作为单个元素添加，而不是单独添加元素，在这种情况下，concat确实具有我们想要的行为，但它实际上并不附加到现有数组，而是创建并返回一个新数组。在这种情况下我们可以使用apply
```js
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

<font color="#00796B">2. 使用apply和内置函数</font><br>
```js
/* 找出数组中最大/小的数字 */
var numbers = [5, 6, 2, 3, 7];

/* 应用(apply) Math.min/Math.max 内置函数完成 */
var max = Math.max.apply(null, numbers); /* 基本等同于 Math.max(numbers[0], ...) 或 Math.max(5, 6, ..) */
var min = Math.min.apply(null, numbers);

/* 代码对比： 用简单循环完成 */
max = -Infinity, min = +Infinity;

for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] > max)
    max = numbers[i];
  if (numbers[i] < min) 
    min = numbers[i];
}
```
但是当心：如果用上面的方式调用apply，会有超出JavaScript引擎的参数长度限制的风险。当你对一个方法传入非常多的参数（比如一万个）时，就非常有可能会导致越界问题, 这个临界值是根据不同的 JavaScript 引擎而定的（JavaScript 核心中已经做了硬编码  参数个数限制在65536），因为这个限制（实际上也是任何用到超大栈空间的行为的自然表现）是未指定的. 有些引擎会抛出异常。更糟糕的是其他引擎会直接限制传入到方法的参数个数，导致参数丢失。举个例子：如果某个引擎限制了方法参数最多为4个（实际真正的参数个数限制当然要高得多了, 这里只是打个比方）, 上面的代码中, 真正通过 apply传到目标方法中的参数为 5, 6, 2, 3 而不是完整的数组。

如果你的参数数组可能非常大，那么推荐使用下面这种策略来处理：将参数数组切块后循环传入目标方法：
```js
function minOfArray(arr) {
  let min = Infinity;
  // let QUANTUM = 32768;
  let QUANTUM = 2;

  for (let i = 0, len = arr.length; i < len; i += QUANTUM) {
    //防止越界
    let rightIndex = Math.min(i + QUANTUM, len);
    let origin = arr.slice(i, rightIndex);
    let submin = Math.min.apply(null, origin);
    min = Math.min(submin, min);
  }

  return min;
}

let min = minOfArray([5, 6, 2, 3, 7]);
```

<font color="#00796B">3. 使用apply来链接构造器</font><br>

你可以使用apply来链接一个对象构造器，类似于Java。在接下来的例子中我们会创建一个全局Function 对象的construct方法 ，来使你能够在构造器中使用一个类数组对象而非参数列表。
```js
Function.prototype.construct = function (aArgs) {
  var oNew = Object.create(this.prototype);
  this.apply(oNew, aArgs);
  return oNew;
};
```

## 二 call

### 语法
function.call(thisArg, arg1, arg2, ...)
#### 参数
* thisArg
可选的。在 func 函数运行时使用的 this 值。
（注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。）
* arg1, arg2, ...
指定的参数列表。
#### 返回值
使用调用者提供的 this 值(thisArg)和参数(arg1, arg2, ...)调用该函数(function)
调用函数的结果即它的返回值。
若该方法没有返回值，则返回 undefined。

```js
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```
结果
> 7
> 2

### 示例
1. 使用 call 方法调用父构造函数<br>
在一个子构造函数中，你可以通过调用父构造函数的 call 方法来实现继承，类似于 Java 中的写法。

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);
```
使用 Food 和 Toy 构造函数创建的对象实例都会拥有在 Product 构造函数中添加的 name 属性和 price 属性,但 category 属性是在各自的构造函数中定义的。

2. 使用 call 方法调用匿名函数
在下例中的 for 循环体内，我们创建了一个匿名函数，然后通过调用该函数的 call 方法，将每个数组元素作为指定的 this 值执行了那个匿名函数。

```js
var animals = [
  { species: 'Lion', name: 'King' },
  { species: 'Whale', name: 'Fail' }
];

for (var i = 0; i < animals.length; i++) {
  (function(i) {
    this.print = function() {
      console.log('#' + i + ' ' + this.species
                  + ': ' + this.name);
    }
    this.print();
  }).call(animals[i], i);
}
```
这个匿名函数的主要目的是给每个数组元素对象添加一个 print 方法，这个 print 方法可以打印出各元素在数组中的正确索引号。当然，这里不是必须得让数组元素作为 this 值传入那个匿名函数（普通参数就可以），目的是为了演示 call 的用法。
3. 使用 call 方法调用函数并且指定上下文的 'this'

```js
function greet() {
  var reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

var obj = {
  animal: 'cats', 
  sleepDuration: '12 and 16 hours'
};

greet.call(obj);  // cats typically sleep between 12 and 16 hours
```
当调用 greet 方法的时候，该方法的this值会绑定到 obj 对象。

4. 使用 call 方法调用函数并且不指定第一个参数（argument）

```js
var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call();  // sData value is Wisen
```

我们调用了 display 方法，但并没有传递它的第一个参数。如果没有传递第一个参数，this 的值将会被绑定为全局对象。

```js
'use strict';

var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call(); // Cannot read the property of 'sData' of undefined
```
在严格模式下，this 的值将会是 undefined

## 三 bind

bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

```js
const module = {
  x: 42,
  getX: function() {
    return this.x;
  }
}

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// expected output: 42

```

### 语法

function.bind(thisArg[, arg1[, arg2[, ...]]])

#### 参数

1. thisArg
调用绑定函数时作为 this 参数传递给目标函数的值。 如果使用new运算符构造绑定函数，则忽略该值。当使用 bind 在 setTimeout 中创建一个函数（作为回调提供）时，作为 thisArg 传递的任何原始值都将转换为 object。如果 bind 函数的参数列表为空，执行作用域的 this 将被视为新函数的 thisArg。
2. arg1, arg2, ...
当目标函数被调用时，被预置入绑定函数的参数列表中的参数。

#### 返回值
返回一个原函数的拷贝，并拥有指定的 this 值和初始参数。

### 示例

<font color="#F5B041">1. 创建绑定函数</font><br>

bind() 最简单的用法是创建一个函数，不论怎么调用，这个函数都有同样的 this 值。JavaScript新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，期望方法中的 this 是原来的对象（比如在回调中传入这个方法）。如果不做特殊处理的话，一般会丢失原来的对象。基于这个函数，用原始的对象创建一个绑定函数，巧妙地解决了这个问题：

```js
this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();   
// 返回 9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
// 新手可能会将全局变量 x 与 module 的属性 x 混淆
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```
<font color="#F5B041">2. 预设初始参数</font><br>


bind() 的另一个最简单的用法是使一个函数拥有预设的初始参数。只要将这些参数（如果有的话）作为 bind() 的参数写在 this 后面。当绑定函数被调用时，这些参数会被插入到目标函数的参数列表的开始位置，传递给绑定函数的参数会跟在它们后面。

```js
function list() {
  return Array.prototype.slice.call(arguments);
}

function addArguments(arg1, arg2) {
    return arg1 + arg2
}

var list1 = list(1, 2, 3); // [1, 2, 3]

var result1 = addArguments(1, 2); // 3

// 创建一个函数，它拥有预设参数列表。
var leadingThirtysevenList = list.bind(null, 37);

// 创建一个函数，它拥有预设的第一个参数
var addThirtySeven = addArguments.bind(null, 37); 

var list2 = leadingThirtysevenList(); 
// [37]

var list3 = leadingThirtysevenList(1, 2, 3); 
// [37, 1, 2, 3]

var result2 = addThirtySeven(5); 
// 37 + 5 = 42 

var result3 = addThirtySeven(5, 10);
// 37 + 5 = 42 ，第二个参数被忽略
```