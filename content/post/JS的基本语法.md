---
title: "JS的基本语法"
date: 2019-10-14T19:34:36+08:00
draft: false
---

## 一 表达式和语句

<font color="orange">JavaScript 程序的执行单位为行（line），也就是一行一行地执行。一般情况下，每一行就是一个语句。</font>

<font color="orange">语句（statement）是为了完成某种任务而进行的操作</font>，比如下面就是一行赋值语句。

`let a = 1 + 3;`

这条语句先用`let`命令，声明了变量`a`，然后将`1 + 3`的运算结果赋值给变量`a`。

`1 + 3`叫做表达式（expression）

表达式指一个为了得到返回值的计算式。

<font color="orange">语句和表达式的区别在于，语句主要为了进行某种操作，一般情况下不需要返回值；
表达式则是为了得到返回值，一定会返回一个值。凡是JavaScript语言中预期为值的地方，都可以使用表达式。
比如，赋值语句的等号右边，预期是一个值，因此可以放置各种表达式。</font>

<font color="orange">语句以分号结尾，一个分号就表示一个语句结束。多个语句可以写在一行内。</font>

`let a = 1 + 3 ; let b = 'abc';`

分号前面可以没有任何内容，JavaScript 引擎将其视为空语句。

`;;;`

上面的代码就表示3个空语句。

表达式不需要分号结尾。一旦在表达式后面添加分号，则 JavaScript 引擎就将表达式视为语句，这样会产生一些没有任何意义的语句。

`1 + 3;`
`'abc';`

上面两行语句只是单纯地产生一个值，并没有任何实际的意义。

## 二 标识符的规则

<font color="orange">标识符（identifier）指的是用来识别各种值的合法名称。</font>最常见的标识符就是变量名，以及后面要提到的函数名。JavaScript 语言的标识符对大小写敏感，所以`a`和`A`是两个不同的标识符。

标识符有一套命名规则，不符合规则的就是非法标识符。JavaScript引擎遇到非法标识符，就会报错。

简单说，标识符命名规则如下。

<font color="orange">第一个字符，可以是任意Unicode字母（包括英文字母和其他语言的字母），以及美元符号（`$`）和下划线（`_`）。
第二个字符及后面的字符，除了Unicode字母、美元符号和下划线，还可以用数字`0`-`9`。</font>
下面这些都是合法的标识符。

`arg0`
`_tmp`
`$elem`
`π`

下面这些则是不合法的标识符。

```javascript
1a  // 第一个字符不能是数字
23  // 同上
***  // 标识符不能包含星号
a+b  // 标识符不能包含加号
-d  // 标识符不能包含减号或连词线
```

中文是合法的标识符，可以用作变量名。

`var 临时变量 = 1;`

## 三 if else 条件语句

if结构先判断一个表达式的布尔值，然后根据布尔值的真伪，执行不同的语句。所谓布尔值，指的是 JavaScript 的两个特殊值，`true`表示真，`false`表示伪

```javascript
if (布尔值)
  语句;

// 或者
if (布尔值) 语句;
```

上面是if结构的基本形式。需要注意的是，“布尔值”往往由一个条件表达式产生的，必须放在圆括号中，表示对表达式求值。如果表达式的求值结果为true，就执行紧跟在后面的语句；如果结果为false，则跳过紧跟在后面的语句。

```javascript
if (m === 3)
  m = m + 1;
```

上面代码表示，只有在`m`等于`3`时，才会将其值加上`1`。

这种写法要求条件表达式后面只能有一个语句。如果想执行多个语句，必须在if的条件判断之后，加上大括号，表示代码块（多个语句合并成一个语句）

```javascript
if (m === 3) {
  m += 1;
}
```

建议总是在if语句中使用大括号，因为这样方便插入语句。

注意，if后面的表达式之中，不要混淆赋值表达式（`=`）、严格相等运算符（`===`）和相等运算符（`==`）。尤其是赋值表达式不具有比较作用。

```javascript
var x = 1;
var y = 2;
if (x = y) {
  console.log(x);
}
// "2"
```

上面代码的原意是，当`x`等于`y`的时候，才执行相关语句。但是，不小心将严格相等运算符写成赋值表达式，结果变成了将`y`赋值给变量`x`，再判断变量`x`的值（等于`2`）的布尔值（结果为`true`）。

<font color="orange">这种错误可以正常生成一个布尔值，因而不会报错。为了避免这种情况，有些开发者习惯将常量写在运算符的左边，这样的话，一旦不小心将相等运算符写成赋值运算符，就会报错，因为常量不能被赋值。</font>

```javascript
if (x = 2) { // 不报错
if (2 = x) { // 报错
```

`if`代码块后面，还可以跟一个`else`代码块，表示不满足条件时，所要执行的代码。

```javascript
if (m === 3) {
  // 满足条件时，执行的语句
} else {
  // 不满足条件时，执行的语句
}
```

上面代码判断变量`m`是否等于`3`，如果等于就执行`if`代码块，否则执行`else`代码块。

对同一个变量进行多次判断时，多个`if...else`语句可以连写在一起。

```javascript
if (m === 0) {
  // ...
} else if (m === 1) {
  // ...
} else if (m === 2) {
  // ...
} else {
  // ...
}
```

`else`代码块总是与离自己最近的那个`if`语句配对。

```javascript
var m = 1;
var n = 2;

if (m !== 1)
if (n === 2) console.log('hello');
else console.log('world');
```

上面代码不会有任何输出，`else`代码块不会得到执行，因为它跟着的是最近的那个`if`语句，相当于下面这样。

```javascript
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  } else {
    console.log('world');
  }
}
```

如果想让`else`代码块跟随最上面的那个if语句，就要改变大括号的位置

```javascript
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  }
} else {
  console.log('world');
}
// world
```

## 四 while for 循环语句

### While语句

`While`语句包括一个循环条件和一段代码块，只要条件为真，就不断循环执行代码块。

```javascript
while (条件)
  语句;

// 或者
while (条件) 语句;
```

`while`语句的循环条件是一个表达式，必须放在圆括号中。代码块部分，如果只有一条语句，可以省略大括号，否则就必须加上大括号。

```javascript
while (条件) {
  语句;
}
```

下面是`while`语句的一个例子。

```javascript
var i = 0;

while (i < 100) {
  console.log('i 当前为：' + i);
  i = i + 1;
}
```

上面的代码将循环100次，直到i等于100为止。

下面的例子是一个无限循环，因为循环条件总是为真。

```javascript
while (true) {
  console.log('Hello, world');
}
```

### for语句

for语句是循环命令的另一种形式，可以指定循环的起点、终点和终止条件。它的格式如下

```javascript
for (初始化表达式; 条件; 递增表达式)
  语句

// 或者

for (初始化表达式; 条件; 递增表达式) {
  语句
}
```

<font color="orange">for语句后面的括号里面，有三个表达式。</font>

1. <font color="orange">初始化表达式（initialize）</font>：确定循环变量的初始值，只在循环开始时执行一次。
2. <font color="orange">条件表达式（test）</font>：每轮循环开始时，都要执行这个条件表达式，只有值为真，才继续进行循环。
3. <font color="orange">递增表达式（increment）</font>：每轮循环的最后一个操作，通常用来递增循环变量。

下面是一个例子。

```javascript
var x = 3;
for (var i = 0; i < x; i++) {
  console.log(i);
}
// 0
// 1
// 2
```

上面代码中，初始化表达式是`var i = 0`，即初始化一个变量`i`；测试表达式是`i < x`，即只要`i`小于`x`，就会执行循环；递增表达式是`i++`，即每次循环结束后，`i`增大`1`。

所有`for`循环，都可以改写成`while`循环。上面的例子改为`while`循环，代码如下。

```javascript
var x = 3;
var i = 0;

while (i < x) {
  console.log(i);
  i++;
}
```

for语句的三个部分（initialize、test、increment），可以省略任何一个，也可以全部省略。

```javascript
for ( ; ; ){
  console.log('Hello World');
}
```

上面代码省略了for语句表达式的三个部分，结果就导致了一个无限循环。

## 五 break continue 语句

`break`语句和`continue`语句都具有跳转作用，可以让代码不按既有的顺序执行。

`break`语句用于跳出代码块或循环。

```javascript
var i = 0;

while(i < 100) {
  console.log('i 当前为：' + i);
  i++;
  if (i === 10) break;
}
```

上面代码只会执行10次循环，一旦i等于10，就会跳出循环。

`for`循环也可以使用`break`语句跳出循环。

```javascript
for (var i = 0; i < 5; i++) {
  console.log(i);
  if (i === 3)
    break;
}
// 0
// 1
// 2
// 3
```

上面代码执行到`i`等于`3`，就会跳出循环。

`continue`语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环。

```javascript
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}
```

上面代码只有在`i`为奇数时，才会输出`i`的值。如果`i`为偶数，则直接进入下一轮循环。

<font color="orange">如果存在多重循环，不带参数的`break`语句和`continue`语句都只针对最内层循环。</font>

## 六 标签（label）

JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。

```javascript
label:
  语句
```

标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。

标签通常与`break`语句和`continue`语句配合使用，跳出特定的循环。

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```

上面代码为一个双重循环区块，`break`命令后面加上了`top`标签（注意，`top`不用加引号），满足条件时，直接跳出双层循环。如果`break`语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。

标签也可以用于跳出代码块。

```javascript
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

上面代码执行到`break foo`，就会跳出区块。

`continue`语句也可以与标签配合使用。

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```

上面代码中，`continue`命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果`continue`语句后面不使用标签，则只能进入下一轮的内层循环。

<font color="orange">面试相关题：</font>

```javascript
//label
{
    top: x
}
```

## 相关链接
[网道JavaScript 教程](https://wangdoc.com/javascript/index.html)<br>
