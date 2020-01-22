---
title: "浅析MVC"
date: 2020-01-03T13:34:36+08:00
draft: false
---

### MVC三类对象

MVC是一种架构设计模式，它通过关注点分离鼓励改进应用程序组织。

MVC包括三类对象，将他们分离以提高灵活性和复用性。

模型model用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法，会有一个或多个视图监听此模型。一旦模型的数据发生变化，模型将通知有关的视图。

视图view是它在屏幕上的表示，描绘的是model的当前状态。当模型的数据发生变化，视图相应地得到刷新自己的机会。

控制器controller定义用户界面对用户输入的响应方式，起到不同层面间的组织作用，用于控制应用程序的流程，它处理用户的行为和数据model上的改变。

mvc伪代码
```js
//数据层，关于数据的操作放在这里
const m = {
    //初始化
    data:{
    },
    //更新数据
    update(data) {
        
    },
    //删除数据
    delete(data) {
    },
    //获得数据
    get(data) {

    },
}
//视图层，关于视图的操作放在这里
const v = {
    el:'挂载点',
    html:'需要插入元素内的HTML内容',
    init(){

    },
    //渲染页面
    render(data){

    },
}
//控制层，其他相关的都放在这里
const c = {
    init() {

    },
    //调用数据层方法更改数据
    changeData() {

    },
    //调用视图层方法渲染页面
    changeUI() {

    },
    //监听数据改变
    monitorData() {

    },
}
```


#### MVC for JAVASCRIPT

![MVC for JAVASCRIPT](/images/javascriptMVC.png)

如图所示，view承接了部分controller的功能，负责处理用户输入，但是不必了解下一步做什么。它依赖于一个controller为她做决定或处理用户事件。事实上，前端的view已经具备了独立处理用户事件的能力，如果每个事件都要流经controller，势必增加复杂性。同时，view也可以委托controller处理model的更改。model数据变化后通知view进行更新，显示给用户。这个过程是一个圆，一个循环的过程。

### EventBus

EventBus API包括

绑定
eventBus.on(eventName, function[, obj]);

触发
eventBus.emit(eventName[, sender][, data]);
eventBus.fire(eventName[, sender][, data]);
eventBus.trigger(eventName[, sender][, data]);


解绑
eventBus.off(eventName[, function]);

伪代码
```js

class EventBus{
    constructor() {
        this._eventBus = $(window)
    }
    //绑定事件
    on(eventName, fn) {
        return this._eventBus.on(eventName, fn)
    }
    //触发事件
    trigger(eventName, data) {
        return this._eventBus.trigger(eventName, data)
    }
    //解绑事件
    off(eventName, fn) {
        return this._eventBus.off(eventName, fn)
    }
}


```

应用

```js
var myEventBus = new eventBus();
```

```js
eventBus.on("data_completed", function (sender, data, obj) {
}, { sign: "F6243749AFF04C0581E1DD178A0B737A" });

eventBus.emit("Data_Completed", window, [1,2,3,4,5,6]);

eventBus.off("Data_Completed", f2);
```

<hr>

### 表驱动编程

表格驱动的意义在于：逻辑和数据分离。

在程序中，添加数据和逻辑的方式是不一样的，成本也是不一样的。简单的说，数据的添加是非常简单的；而逻辑的添加是更复杂的。

比如说，国家简写转换，给一个国家全名，转换成国家简写，用 if－else 法就写成：

```js
function country_initial($country){
    if ($country === "China" ) {
       return "CHN"
    } else if ($country === "America") {
       return "USA"
    } else if ($country === "Japna") {
      return "JPN"
    } else {
       return "OTHER"
    }
}
```

如果我要增加一个国家，那么我要多加一个 else if 语句，那么我就是增加了一条逻辑

如果改成表驱动法就是：

```js
function country_initial($country){
  $countryList=[
      "China"=> "CHN",
      "America"=> "USA",
      "Japna"=> "JPN",
    ];
    if(in_array($country, array_keys($countryList))) {
        return $countryList[$country];
    }
    return "Other";
}
```
如果我需要增加一个国家，我只需要在数组里面加个数据
如此一来，我就可以剥离这个数据与逻辑的关系了。

逻辑和数据区分一目了然
关系表可以更换，比如国家表格可以是多语言的，中文版表格，英文版表格等，日语版表格，以及单元测试中，可以注入测试表格。
在单元测试中，逻辑必须测试，而数据无需测试。

总结一下就是：

逻辑与数据分离
逻辑修改成本巨大，数据修改成本极小
数据来源灵活，数据改变灵活

<hr>

### 如何理解模块化

模块化就是将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起
块的内部数据与实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信

模块化规范<br>
CommonJS

Node 应用由模块组成，采用 CommonJS 模块规范。每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。在服务器端，模块的加载是运行时同步加载的；在浏览器端，模块需要提前编译打包处理。

AMD<br>
CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范。此外AMD规范比CommonJS规范在浏览器端实现要来着早。

CMD<br>
CMD规范专门用于浏览器端，模块的加载是异步的，模块使用时才会加载执行。CMD规范整合了CommonJS和AMD规范的特点。在 Sea.js 中，所有 JavaScript 模块都遵循 CMD模块定义规范。

<hr>
参考链接<br>
https://efe.baidu.com/blog/mvc-deformation/
<br>
https://www.jianshu.com/p/1996ad3e09f9
<br>
https://learnku.com/laravel/t/2712/the-meaning-of-the-if-else-code-style-into-the-table-driven-method
<br>
https://juejin.im/post/5c17ad756fb9a049ff4e0a62