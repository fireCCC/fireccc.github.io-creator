---
title: "HTML常用标签"
date: 2019-08-28T13:34:36+08:00
draft: false
---

## 一 a 标签的用法

*  属性 1：href(hyper reference) 超级引用。
  * 取值：网址（https://xx.com, http://xx.com, //xx.com）
  * 路径: /a/b/c 根目录, 也就是服务器的根目录。 a/b 相对路径，等于 ./a/b 
  * 伪协议：`<a href="javascript:alert(1);">xxx</a>`
  * `<a href="javascript:;">xxx</a>`//没有动作
  * `<a href="">xxx</a>`//页面刷新
  * `<a href="#">xxx</a>`//回到页面顶部
  * `<a href="baidu" target="xxx">baidu</a>`//打开一个window.name为xxx的窗口，若没有就新建
  * `<a href="mailto:m.bluth@example.com">Email</a>`//邮件
  * `<a href="tel:+123456789">Phone</a>`//电话

* 属性 2：target（决定跳转的链接在哪里打开）
  * 取值：_self 当前页面加载，即当前的响应到同一HTML 4 frame（或HTML5浏览上下文）。此值是默认的，如果没有指定属性的话。
  * _blank 新窗口打开，即到一个新的未命名的HTML4窗口或HTML5浏览器上下文
  * _top HTML5中：加载响应进入顶层浏览上下文, 如果没有parent框架或者浏览上下文，此选项的行为方式相同_self
  * _parent 加载响应到当前框架的HTML4父框架或当前的HTML5浏览上下文的父浏览上下文。如果没有parent框架或者浏览上下文，此选项的行为方式与 _self 相同。

## 二 img 标签的用法

* 作用是发起 get 请求，展示图片
  * 属性 1：alt(图片加载失败的时的提示文字)
  * 属性 2：src(source)(图片路径)
  * 属性 3：height(高度)
  * 属性 4：width(宽度)

## 三 table 标签的用法

* 常用的标签（table、thead、tbody、tfoot、th、tr、td）
  * 注意的样式 1：table-layout
  * 注意的样式 2：border-collapse（决定是否合并 collapse(合并) | separate（分开））
  * 注意的样式 3：border-spacing（每个 td 之间的间隙）

## 其他

* img max-width: 100%;
* form 发get或者post请求，然后刷新页面
* input和button
  * `<input type="submit" value="xxx" />`
  * 
  ```
  <button type="submit">
    <strong>xxx</strong>
  </button>
  ```
button标签里还可以嵌套其他标签，input不行

* 表单中`<button type="">xxx</button> `默认type="submit"

* 单选和复选框使用相同name属性表示一组

* 选择文件
```
<input type="file" />

<input type="file" multiple /> //选择多个文件
```
* textarea
```
<textarea style="resize: none; width: 50%; height: 300px"></textarea>
```
//禁止拖动大小
