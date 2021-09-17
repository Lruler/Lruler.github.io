---
title: Unit5-Flexbox
date: 2021-06-12 20:49:50
tags: css
categories: 笔记
---

Css in depth 第五章 flexbox 的学习笔记
<!--more-->


# Flexbox 原则

从display属性开始，当我们给一个元素添加display:flex后，该容器就会变成一个弹性容器，它的直接子元素就变成了弹性子元素，弹性子元素默认是在同一行按照从左到右的顺序并排排列。 弹性容器和块级容器一样自动填满可用的宽度，但弹性子元素不一定会填满容器的宽度弹性子元素高度相等，该高度由他们的内容决定。

然后弹性容器里面有两个坐标轴，主轴为水平x(默认，从左到右)和副轴y(从上到下)，子元素就会默认沿主轴排列，并成等高列形式。

# 容器属性

接下来就要看看flex布局的属性了，这部分是核心，了解他们才能熟练掌握flex布局，接下来先看看容器属性，也就是通过在弹性容器中设置属性来操纵弹性子元素
```
        1. flex-direction: 设置主轴方向
        {
            row  //默认
            row-reverse  //从右到左
            column  //从上到下
            column-reverse //从下到上
        }

        2. justify-content: 设置主轴上子元素排列方式
        {
            flex-start  //默认
            flex-end  //从尾部开始
            center  //居中对齐
            space-around //平分剩余空间
            space-between //先两边贴合 再平分剩余空间
        }

        3. flex-warp: 设置子元素是否换行
        {
            nowarp  //默认不换行
            wrap  //换行
            wrap-reverse  //换行，第一行往下
        }

        4. align-items: 设置侧轴子元素排列方式 (单行)
        {
            flex //侧轴默认方向
            flex-end  //默认反向
            center  //居中
            stretch  //拉伸(默认)
            baseline  //取内容第一行文字基线对齐
        }

        5. align-content: 设置侧轴子元素排列方式(多行)
        {
            其他属性和单行相同，多了space-around 和 space-bettwen
        }

        6. flex-flow 同时设置 flex-direction 和 flex-warp 
```

# item 属性
接下来再看看子相属性
```
        order 定义项目排列顺序 数值越小 排列越靠前
        flex-basis 定义了该item(弹性子元素)尺寸的基准值，默认是auto，即项目由本身内容决定的尺寸，该值可以是任意width单位值，
        flex-grow  每个item的增长比例，即用于平分剩余空间时所占的比例
        flex-shrink  每个item的缩小比例 即内容溢出后所占的缩小比例
        flex： flex-gorw，flex-shrink，flex-basis 的合写
        align-self 用于设置单个item的对齐方式，可以覆盖align-items 值与其相等
```

Okey Flex到此为止咯