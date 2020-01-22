---
title: "Vue 两个版本"
date: 2020-01-15T13:34:36+08:00
draft: false
---
### 两个版本对应的文件名
完整版：vue.js<br>
只包含运行时版(非完整版)：vue.runtime.js<br>

完整版：同时包含编译器和运行时的版本。

编译器：用来将模板字符串编译成为 JavaScript 渲染函数的代码。

运行时：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的代码。基本上就是除去编译器的其它一切。

### template 和 render 怎么用

```js
// 需要编译器
new Vue({
  template: '<div>{{ hi }}</div>'
})
// 不需要编译器
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

yarn build 当使用 vue-loader 或 vueify 的时候，*.vue 文件内部的模板会在构建时预编译成 JavaScript。你在最终打好的包里实际上是不需要编译器的，所以只用运行时版本即可。

因为运行时版本相比完整版体积要小大约 30%，所以应该尽可能使用这个版本。

### 两种版本区别

![vue-version](/images/vue-version.png)

最佳实践：总是使用非完整版，然后配合vue-loader和vue文件

思路：<br>
1. 保证用户体验，用户下载的JS文件体积更小，但是只支持h函数<br>
2. 保证开发体验，开发者可以直接在vue文件里写html标签，而不写h函数<br>
3. 其他活让vue-loader去做<br>



### 如何用 codesandbox.io 写 Vue 代码

不进行注册登录 直接create a sandbox

然后选择Explore Templates

![codesandbox](/images/codesandbox.png)

![code-sand-box-vue](/images/code-sand-box-vue.png)

