---
title: "jQuery设计思想"
date: 2019-11-10T19:34:36+08:00
draft: false
---

<font color="0769AD">jQuery的基本设计思想和主要用法，就是选择某个网页元素，然后对其进行某种操作。<br>
这是它区别于其他Javascript库的根本特点。</font>
## 一 jQuery如何获取元素

使用jQuery的第一步，往往就是将一个选择表达式，放进构造函数jQuery()（简写为$），然后得到被选中的元素。
选择表达式可以是CSS选择器：

![css-selector](/images/css-selector.png)

也可以是[jQuery特有的表达式]((https://www.jquery123.com/category/selectors/))：

```js
$('a:first') //选择网页中第一个a元素

$('tr:odd') //选择表格的奇数行

$('#myForm :input') // 选择表单中的input元素

$('div:visible') //选择可见的div元素

$('div:gt(2)') // 选择所有的div元素，除了前三个

$('div:animated') // 选择当前处于动画状态的div元素
```

## 二 jQuery的链式操作是怎样的

<font color="0769AD">jQuery设计思想之三，就是最终选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来。比如：</font>

```js
　$('div').find('h3').eq(2).html('Hello');
```

分解开来，就是下面这样：

```js
$('div') //找到div元素
  .find('h3') //选择其中的h3元素
  .eq(2) //选择第3个h3元素
  .html('Hello'); //将它的内容改为Hello
```

<font color="0769AD">这是jQuery最令人称道、最方便的特点。它的原理在于每一步的jQuery操作，返回的都是一个jQuery对象，
这个jQuery对象中就包括了那些可以对选中元素做一些操作的函数
所以不同操作可以连在一起。</font>

jQuery还提供了.end()方法，使得结果集可以后退一步：

![end](/images/end.png)

## 三 jQuery如何创建元素

创建新元素的方法非常简单，只要把新元素直接传入jQuery的构造函数就行了：

```js
$('<p>Hello</p>');

$('<li class="new">new list item</li>');

$('ul').append('<li>list item</li>');
```

复制元素使用.clone()。

删除元素使用.remove()和.detach()。两者的区别在于，前者不保留被删除元素的事件，后者保留，有利于重新插入文档时使用。

清空元素内容（但是不删除该元素）使用.empty()。

## 四 jQuery如何移动元素

<font color="0769AD">jQuery设计思想之五，就是提供两组方法，来操作元素在网页中的位置移动。一组方法是直接移动该元素，另一组方法是移动其他元素，使得目标元素达到我们想要的位置。</font>

假定我们选中了一个div元素，需要把它移动到p元素后面。

第一种方法是使用.insertAfter()，把div元素移动p元素后面：

```js
$('div').insertAfter($('p'));
```

第二种方法是使用.after()，把p元素加到div元素前面：

```js
$('p').after($('div'));
```

表面上看，这两种方法的效果是一样的，唯一的不同似乎只是操作视角的不同。但是实际上，它们有一个重大差别，那就是返回的元素不一样。第一种方法返回div元素，第二种方法返回p元素。你可以根据需要，选择到底使用哪一种方法。

使用这种模式的操作方法，一共有四对：
![move](/images/move.png)

## 五 jQuery如何修改元素的属性

操作网页元素，最常见的需求是取得它们的值，或者对它们进行赋值。

<font color="0769AD">jQuery设计思想之四，就是使用同一个函数，来完成取值（getter）和赋值（setter），即"取值器"与"赋值器"合一。到底是取值还是赋值，由函数的参数决定。</font>

```js
$('h1').html(); //html()没有参数，表示取出h1的值

$('h1').html('Hello'); //html()有参数Hello，表示对h1进行赋值
```

常见的取值和赋值函数如下：

![change](/images/change.png)

需要注意的是，如果结果集包含多个元素，那么赋值的时候，将对其中所有的元素赋值；取值的时候，则是只取出第一个元素的值（.text()例外，它取出所有元素的text内容）。


参考链接

[jQuery设计思想-阮一峰](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)