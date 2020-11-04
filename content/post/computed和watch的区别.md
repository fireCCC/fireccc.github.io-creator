---
title: "computed和watch的区别"
date: 2020-02-03T13:34:36+08:00
draft: false
comment: true   # 评论
reward: false	 # 打赏
tags: ["Vue"]  # 标签
categories: ["Vue"]              # 分类
---
### computed和watch

computed是计算属性<br>
使用的时候不需要加括号<br>
计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值
即依赖没有发生变化的时候，computed不会重新计算<br>

watch是侦听器<br>
如果某个属性变化了，就会去执行一个函数

除了 watch 选项之外，还可以使用命令式的 vm.$watch<br>
Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。如果需要在某个数据变化时做一些事情，使用watch

示例：
computed
```js
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // 仅读取
    aDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
vm.aPlus   // => 2
vm.aPlus = 3
vm.a       // => 2
vm.aDouble // => 4
```
<hr>

watch
{{< highlight go "linenos=table,hl_lines=,linenostart=1" >}}
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 方法名
    b: 'someMethod',
    // 设置deep: true
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 设置immediate: true
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: 'someMethod',
      immediate: true
    },
    e: [
      'handle1',
      function handle2 (val, oldVal) { /* ... */ },
      {
        handler: function handle3 (val, oldVal) { /* ... */ },
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
{{< / highlight >}}

参考链接：<br>
https://cn.vuejs.org/v2/api/index.html#computed<br>
https://cn.vuejs.org/v2/guide/computed.html<br>
