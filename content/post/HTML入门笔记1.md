---
title: "html 入门笔记 1"
date: 2019-08-27T13:34:36+08:00
draft: false
---

## 一 HTML 是谁发明的

蒂姆·伯纳斯-李(简称李爵士) 大佬毕竟是大佬：）

## 二 HTML 起手应该写什么

利用编辑器 emmet 插件使用 ！+ tab 自动生成

```
<!DOCTYPE html> //声明文档类型为html,而不是svg/xml
<html lang="zh-CN"> //声明语言类型，翻译参考
  <head>
    <meta charset="UTF-8" /> //文件的字符编码
    <meta name="viewport" content="width=device-width, initial-scale=1.0" /> //兼容手机
    <meta http-equiv="X-UA-Compatible" content="ie=edge" /> //告诉IE使用最新内核
    <title>Document</title> //网页标题
  </head>
  <body></body>
</html>
```

## 三 常用的表示章节的标签

- header(头部)
  定义文章的介绍信息：标题，logo，slogan；包裹目录部分，搜索框，一个 nav 或者任何相关的 logo；
  一个页面中`<header>`的个数没有限制，可以为每个内容块添加一个 header；

- h1-h6(数字越小字体越大)

- section(章、节)
  与 article 的差别在于，它是整体的一部分，或者是文章的一节，一般来说 section 也会带有标题；

- article(文章)
  定义文档中可以脱离其他部分的，一份独立的内容，通常带有标题，当 article 内嵌 article 时，里外层的内容应该是相关的，比如一篇微博和它的评论；

- main(主要的)
  定义文章的主要内容
  main 标签在一份文档中是唯一的，其后代元素常常包括`<article>`；

- aside(旁边的)
  侧边栏（与 article 并列存在）或者嵌入内容（在 article 内），通常认为是独立拆分出来而不受整体影响的一部分，作为主要内容的附属信息，如索引，词条列表，或者页面及站点的附属信息，如广告，作者资料介绍等；

- p(段落)

* footer(脚部)
  页脚，通常包含作者、版权信息或者相关链接等；

* div(划分 块级元素)

## 四 全局属性有哪些

- class (引用 css 以.开头的样式, 尽量能用就用)
- contenteditable(在浏览器可以编辑)
- hidden(隐藏)
- style(内联样式)
- tabindex(增加用户体验多元化) 可以取 1 2 0 -1 ，tab、 shift tab 切换, -1 永远不访问
- title(增加可读性)
- id

不到万不得已不要用 id，
html 和 css 中出现多个相同 id 的标签同时生效

id 不能和 window 属性重名，
因为 xxx.style.border 失效，
document.getElementById 有效

## 五 常用的内容标签有哪些，分别是什么意思

- a(anchor 超链接)：用作跳转其他页面
- ol+li(ordered list 有序列表)：带有数字的列表
- ul+li(unordered list 无序列表)：以·开头的列表
- dl+dt+dd(description list 描述列表)
- pre(输出默认格式) html 回车缩成一个空格 tab 缩成一个空格 多个空格缩成一个空格
  保留用 pre 标签包裹
- hr(水平线)
- br(回车)
- em(强调语气,以斜体展示)
  表达对文本内容的强调；
  其强调位置的不同往往带来语义的变化（可以理解为英语口语中针对一句话中不同位置的重度，影响听话人的理解）；
  在视觉效果上也是斜体的效果；
- strong(重要强调,以加粗展示)
  表示强调带有着重意味，意在传达内容的重要性（需要尽快被看到）、严重性或者紧急性，；
- code(代码标签)
- quote(引用)
- blockquote(独自占一行块引用)

1. `<em>`适用于在一段文本中突出重点，强调位置的不同往往会影响语义，而如果仅仅在语态或者语气上为了突出某个文本，应该使用`<i>`；
2. 在使用`<i>`时，W3C 鼓励开发者最好检查下是否有更合适的标签可替代，例如，上述的`<em>`，来突出重点，或是`<dfn>`，用来定义一个术语；
3. 如果为了突出文本的重要性，紧急性，严重性应该使用`<strong>`；
4. W3C 明确说明，`<b>` 元素应当是在其他标签都不合适的情况下最后一个考虑使用的标签，言外之意，官方并不推荐使用 b 标签

相关信息引用：https://zhuanlan.zhihu.com/p/32570423
