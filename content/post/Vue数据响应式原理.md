---
title: "Vue数据响应式原理"
date: 2020-01-22T13:34:36+08:00
draft: false
comment: true   # 评论
reward: false	 # 打赏
tags: ["Vue数据响应式原理"]  # 标签
categories: ["Vue"]              # 分类
---
### 数据响应式

若一个物体能对外界的刺激做出反应，它就是响应式的。<br>

Vue的data是响应式，即数据响应式<br>
const vm = new Vue({data: {n:0}})<br>
我如果修改vm.n，那么UI中的n就会响应我<br>
Vue2通过Object.defineProperty来实现数据响应式<br>

<font color="#41B883">当把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。
</font>

Object.defineProperty 是 ES5 中一个无法 shim 的特性，这也就是 Vue 不支持 IE8 以及更低版本浏览器的原因。

<font color="#41B883">这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 能够追踪依赖，在属性被访问和修改时通知变更。</font>

(这里需要注意的是不同浏览器在控制台打印数据对象时对 getter/setter 的格式化并不同，所以建议安装 vue-devtools 来获取对检查数据更加友好的用户界面。)

<font color="#41B883">每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据属性记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。</font>

![vue-data-reactive](/images/vue-data-reactive.png)


### 检测变化的注意事项


受现代 JavaScript 的限制 (而且 Object.observe 也已经被废弃)，Vue 无法检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 getter/setter 转化，所以属性必须在 data 对象上存在才能让 Vue 将它转换为响应式的。例如：

```js
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的
```

<font color="#41B883">对于已经创建的实例，Vue 不允许动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, propertyName, value) 方法向嵌套对象添加响应式属性。</font>例如：

`Vue.set(vm.someObject, 'b', 2)`

还可以使用 vm.$set 实例方法，这也是全局 Vue.set 方法的别名：

`this.$set(this.someObject,'b',2)`

<font color="#41B883">由于 Vue 不允许动态添加根级响应式属性，所以你必须在初始化实例前声明所有根级响应式属性，哪怕只是一个空值：</font>
(Vue会给出一个警告，Vue只会检查第一层属性)
```js
var vm = new Vue({
  data: {
    // 声明 message 为一个空值字符串
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// 之后设置 `message`
vm.message = 'Hello!'
```

如果你未在 data 选项中声明 message，Vue 将警告你渲染函数正在试图访问不存在的属性。

Vue.set和this.$set的作用包括

* 新增key
* 自动创建代理和监听（如果没有创建）
* 触发UI更新（但是并不会立刻更新）

<font color="#41B883">Vue 将被侦听的数组的变异方法(mutation method)进行了包裹，所以它们也将会触发视图更新。</font>这些被包裹过的方法包括：

```js
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
```

ES6
```js
class VueArray extends Array{
    push(...args){
        const oldLength = this.length
        super.push(...args)
        for(let i = oldLength; i < this.length; i++){
            Vue.set(this, i, this[i])
        }
    }
}
```
ES5
```js
const vueArrayPrototype = {
    push: function(){
    log('something else')
    return Array.prototype.push.apply(this,arguments)
    }
}
vueArrayPrototype._proto_ = Array.prototype
const array = object.create(vueArrayPrototype)
array.push(1)
```
参考链接：<br>
https://cn.vuejs.org/v2/guide/reactivity.html<br>
https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B
