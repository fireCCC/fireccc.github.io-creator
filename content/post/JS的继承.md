---
title: "JS的继承"
date: 2019-12-23T13:34:36+08:00
draft: false
---

### 基于原型的继承

```js
function Animal () {
  this.x = 1
}
Animal.prototype.move = function() {
  console.log('Animal-move')
}

function Dog () {
  Animal.apply(this, arguments)
  this.xx = 2
}

let f = function() {}
f.prototype = Animal.prototype
Dog.prototype = new f()

Dog.prototype.constructor = Dog

Dog.prototype.run = function(){
  console.log('Dog-run')
}
```
重点：
```js
let f = function() {}
f.prototype = Animal.prototype
Dog.prototype = new f()

Dog.prototype.constructor = Dog
```

### 基于 class 的继承

```js
class Animal {
  constructor () {
    this.x = 1
  }
  move() {
    console.log('Animal-move')
  }
}
class Dog extends Animal {
  constructor () {
    super()
    this.xx = 2
  }
  run() {
    console.log('Dog-run')
  }
}
```

重点：
```js
class Dog extends Animal {
  constructor () {
    super()
  }
}
```