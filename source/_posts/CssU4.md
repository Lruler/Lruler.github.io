---
title: Unit4-理解浮动
date: 2021-06-12 20:46:16
tags: css
categories: 笔记
---
Css in depth 第四章笔记
<!--more-->


# 什么是浮动
就飘起来浮上去呗，在普通文档流中一个块级元素就是占据一行的，添加浮动后下面的元素就都跑到一行来了，满了塞不下了就换行，这就是浮动(我这解释有点破哈)，
并且浮动的元素具有行内块元素的特征，并不会保留原先的位置。
块级元素的宽=宽度默认情况下和父元素一致，而添加浮动后它的尺寸就根据内容决定了。
而设计浮动的初衷其实是为了实现这种效果

![1](/CssU4/1.png)

实现文字包围图片，跟报纸那样的。虽然现在浮动用的不多，很多东西都能代替浮动，比如flex布局，但还是很有必要学习浮动的，因为想要实现把图片移动到浏览器的一侧，或者文字围绕图片，浮动仍然是唯一的选择

# 清除浮动

## 为什么要清除浮动
父元素容器很多情况下不方便给定高度，子元素添加浮动后又释放了原本的位置，那么父容器就成了一个高度为0且没有内容的容器，会影响下面普通文档流的其他元素，所以就需要清除该浮动带来的副作用。

如果父元素本身有高度则不需要清除浮动，同样，清除浮动后父元素会根据子元素的内容来改变自身的高度，就不会影响后面的标准流(普通文档流)的元素来

## 清除浮动的方法

1. 额外标签法(w3c推荐做法)
在浮动的元素后面添加一个 `<div class='clear'>`,并写上

        .clear{
            clear:both;
        }

2. 给父元素容器添加overflow属性(不是默认值就行)
3. 伪元素法 类似于额外标签法，不过是用伪元素实现，假设父元素容器class='clearfix'

        .clearfix:after{
            content: '';
            display: block;
            height: 0;
            clear: both;
            visibility: hidden;
        }
   
或者用双伪元素法 //用于清除外边距折叠的时候用 因为有了BFC

        .clearfix:before,clearfix:after{
            content: '';
            display: table;
        }

        .clearfix:after{
            clear: both;
        }

# BFC 
哎 上一章刚说的BFC，这一章就提到了，那就简单聊聊吧
BFC(块级格式化上下文 (Block Fromatting Context)) 书上的和网上搜的说的都有点怪，我的理解就是在网页中划分一个一个区域，或大或小，每个区域有着自己的内容，不会干涉到其他区域。以下方式会创建一个BFC

         float: left or float: right—anything but none
         overflow: hidden, auto, or scroll—anything but visible
         display: inline-block, table-cell, table-caption, flex, inline-flex,grid, or inline-grid—these are called block containers.
         position: absolute or position: fixed
    
**Css也有框架，比如bootstrap啥的，也可以学学，**

# 网格系统
[网格系统](https://www.w3school.com.cn/css/css_grid.asp)
