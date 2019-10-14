---
title: "CSS知识总结"
date: 2019-09-15T13:34:36+08:00
draft: false
---

## 一 浏览器渲染过程

#### 概览步骤
* 根据HTML构建HTML树（DOM）
* 根据CSS构建CSS树（CSSOM）
* 将两棵树合并成一颗渲染树（render tree）
* Layout布局（文档流、盒模型、计算大小和位置）
* Paint绘制（把边框颜色、文字颜色、阴影等画出来）
* Composite合成（根据层叠关系展示画面）

#### 三颗树图示
![三棵树](/images/三颗树.png)

#### 三种更新方式
1. 第一种，全部步骤<br>
div.remove()会触发当前元素消失，其他元素re-layout<br>
2. 第二种，跳过layout<br>
改变背景颜色，直接repaint+composite<br>
3. 第三种，跳过layout和paint<br>
改变transform，只需composite<br>
注意！必须全屏查看效果，在iframe里看有问题<br>
图示如下：

![三种更新方式](/images/三种更新方式.png)

## 二 CSS属性之transition-过渡

#### transition作用--补充中间帧

#### transition语法如下

<font color=#0099ff>transition: 属性名 时长 过渡方式 延迟</font><br>
transition: left 200ms linear<br>
可以用逗号分隔两个不同属性<br>
transtion: left 200ms, top 400ms<br>
可以用all代表所有属性<br>
transition: all 200ms<br>
过渡方式有: linear|ease|ease-in|ease-out|ease-in-out<br>
cubic-bezier|step-start|step-end|steps<br>
具体细节参考数学相关<br>

<font color=#FF7F50>注意！并不是所有属性都能过渡</font><br>
display:none => block没办法过渡<br>
一般改为visibility: hidden => visible<br>

<font color=#0099ff>过渡必须要有起始</font><br>
一般只有一次动画，或者两次<br>
比如hover和非hover状态的过渡<br>

## 三 CSS属性之animation-动画

#### 使用animation步骤

1. 声明关键帧@keyframes
2. 添加动画animation

#### @keyframes语法

1. from to<br>
```CSS
@keyframes slidein {
    from {
        transform: translateX(0%);
    }
    to {
        transform: translateX(100%);
    }
}
```
2. 百分数定义<br>
```CSS
@keyframes identifier {
  0% { top: 0; left: 0; }
  30% { top: 50px; }
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}
```
#### animation语法
<font color=#0099ff>animation: 时长 | 过渡方式 | 延迟 | 次数 | 方向 | 填充模式 | 是否暂停 | 动画名</font><br>
时长：1s或者1000ms<br>
过渡方式：跟transition取值一样，如linear<br>
次数：3或者2.4或者infinite<br>
方向：reverse|alternate|alternate-reverse<br>
填充模式：none|forwards|backwards|both<br>
是否暂停：paused|running<br>
以上所有属性都有对应的单独属性<br>


## 相关链接
[渲染树构建、布局及绘制](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction)<br>
[渲染性能](https://developers.google.com/web/fundamentals/performance/rendering/)<br>
[坚持仅合成器的属性和管理层计数](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)<br>
[CSS Triggers](https://csstriggers.com/)<br>

