---
title: Unit1-层叠，优先级，继承
date: 2021-06-12 20:21:43
tags: css
categories: 笔记
---
从现在开始系统学习Css啦，也就是把css in depth 好好看一遍，这是第一单元的笔记
<!--more-->

# 层叠

## 基本概念
先理解CSS中的C(Cascading)，层叠:
CSS样式在针对同一元素配置同一属性时，依据层叠规则（权重）来处理冲突，选择应用权重高的CSS选择器所指定的属性，一般也被描述为权重高的覆盖权重低的，因此也称作层叠。

而CSS的本质就是声明规则，即在各种条件下产生我们所希望的特定效果。在一份庞大的工作项目中,CSS代码就会变得很复杂，因为在CSS里实现同一种效果会有好几种不同的方式，而当HTML的结构发生改变后，这些不同的CSS实现方式又会产生不同的结果，浏览器在解释这部分样式规则的时候，可能会出现一个元素应用多个规则，这时就会产生**冲突**。

所以层叠大体上就上应对这种冲突情况产生的一种解决规则，层叠有三个条件:

        1.样式表的来源：样式来源，包括自己的CSS文件和浏览器默认(用户代理)样式
        2.选择器优先级：哪些选择器的权重
        3.源码顺序：CSS代码块里面的声明顺序 
        // !important声明权重最高，内联style样式其次

浏览器就会根据以上三条规则按顺序解析CSS从而解决冲突问题。

# 优先级

## 选择器的权重

        #id -- 100
        .class -- 10
        tag -- 1
        // 伪类选择器和属性选择器的权重和类选择器一致，通配符*和组合器(>/+/~)对优先级权重没有影响

## 关于解决冲突的一些方法

有以下代码

```html
 <style>
    #main li {
        background-color: red;
        margin: 20px;
        }
</style>

<body>
    <ul id='main'>
        <li></li>
        <li></li>
        <li class="featured"></li>
    </ul>
</body>
```
想要让最后一个li变成蓝色，有几种方法可以实现呢？

最简单的当然就是用!important 或者 内联的style样式

```html
 <style>
    #main li {
        background-color: red;
        margin: 20px;
        }
    .featured{
        background-color: blue !important;
    }
</style>


<body>
    <ul id='main'>
        <li></li>
        <li></li>
        <li class="featured" style='background-color: blue'></li>
    </ul>
</body>
```
但最简单同样也是最不推荐的，应为这样写的话在一些需求大量的CSS中可能会引起很多未知的Bug(比如太多的important让一切冲突回到原点)

所以我们选择提升最后一个li选择器的优先级。
```html
 <style>
    #main li {
        background-color: red;
        margin: 20px;
        }
    
    #main .featured{
        background-color: blue;
    }
</style>
```
到这一步了，我们来个逆向思维，降低其他li的选择器优先级也是一个方法,我们给ul添加一个class，用class选择器代替id选择器
**这第三种方法也是 Css in depth 最推荐的一种方法**
```html
 <style>
    .class li {
        background-color: red;
        margin: 20px;
        }
    .class .featured{
        background-color: blue;
    }
</style>

<body>
    <ul class='main'>
        <li></li>
        <li></li>
        <li class="featured"></li>
    </ul>
</body>
```

最后一个方法就是按照层叠条件的第三点来了，也就是源码顺序，把两个声明的优先级放到一致，后声明的样式会覆盖前面的样式，这里也不太推荐使用，因为只会覆盖同名的属性，也会造成很多不一致的样式Bug，就不放代码细述了。


## a的伪类选择器
除开几种基础选择器外，还有一些复合选择器，像什么后代选择啊，子选择，并集选择，伪类选择。这里我们就说一下关于a标签伪类选择器的优先级问题。
a标签的伪类选择有四种

        a:link   选择未被访问的
        a:visited 选择已被访问的
        a:hover 选择鼠标悬停的
        a:active 选择鼠标按下未弹起的

关于这四种伪类选择器的优先级，记住 LVHA的顺序 (LV包Hao)即可

## 其他
这里先明白一个概念，层叠值:
**层叠值就是作为层叠结果，应用到一个元素上的特定属性的值**
大白话说就是几个规则都被用于渲染一个元素啊，比武招亲，这叫冲突，然后打架嘛，最后只有一个人获胜，也就是只有一条CSS声明的属性能赢，一夫一妻嘛，这个赢了的声明属性就叫层叠值。
还要注意不要过度的使用id选择器和!important规则。这点之前也提到过，因为这些权重很高，很难覆盖，除了可能会引起一些麻烦外，对于后续的修改优化工作也有很大的阻挠。

# 继承

除了内联，引入Css文件，用户代理等外，这里还有最后一种给HTML元素添加样式的方法，就是**继承**。
首先我们都知道，HTML就是个DOM树。
![01](./CssU1/1 .png)
当我们给body元素添加font-family时，body以下所有的子节点都会继承这个字体，但不是所有属性都能被继承，一般情况下，能被继承的属性有: 文本相关:**color, font, font-family, font-size, font-weight, font-variant,font-style, line-height, letter-spacing, text-align, text-indent, text-transform,white-space, and word-spacing.**  再比如列表属性:**list-style, list-style-type, list-style-position, and list-style-image.** 以及表格的边框属性:**border-collapse and border-spacing,** (这是控制的边框行为)。

## 特殊值

有inherit(继承)和initial(初始)两个特殊值可以赋给任意属性，比如一般情况下我们会给一个网页里面所有的a添加一个字体颜色，但放在页脚的颜色却不能那么显眼，我们就可以

```css
a {
    color: red;
}

.footer {
    color: gray;
}

.footer a{
    color: inherit;
}
```
哎，这里就会问，为什么不直接把页脚的a设置成灰色呢，嗯我也在想，书上说这样继承的话可以直接通过修改.footer里面的值来改变页脚链接的文本颜色，唉可能是我还没有接触到太复杂的页面所以没啥感觉吧呜呜呜。但要注意，**继承的优先级连标签选择器都比不上**
接下来就是initial，也很好理解，就是把属性的值设置为初始值，一般用于重置一个属性，但有以下几点需要注意以下:
1. 不要习惯用auto重置，有些属性的默认值是auto，比如width，但auto对很多属性来说是不合法对值，比如padding等、
2. display的初始值是inline，无论哪种元素的display初始值都是inline，所以小心对display用initial
   
## 简写属性
这里提一下简写属性，就是把很多属性放到同一个属性名下，比如用font来写font-style,font-weight等
`font: italic bold 18px/1.2 "Helvetica", "Arial", sans-serif;`
但这种我不喜欢用，主要是记不住顺序，Emmmm，还是写的少了....估计写多点就能记得住了。(尽管它会包容你的顺序错误TnT)

唯一能记得住的简写方式就是一些表大小或位置的属性，比如border-width,这种后面可以跟四个值，分别代表上右下左(顺时针方向),三个值就是上 左右 下，两个就是上下 左右，一个则设置上下左右。

但要注意，有部分的属性后面的值和上右下左的方向刚好相反，比如background-position, box-shadow,和text-shadow，比如他们后面跟两个值的话，就是先指定左右，再指定上下。书上说相反的原因很简单，按照笛卡尔网格来，按照x,y测量啥的，我就理解为阴影嘛，就跟光影差不多，初中物理就知道呈的是倒立的虚像，所以就反着来呗.

至此第一章就结束来...写的还满多的....

