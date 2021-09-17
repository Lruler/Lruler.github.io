---
title: Unit3-盒模型
date: 2021-06-12 20:32:55
tags: css
categories: 笔记
---
Css in depth 第三章学习笔记

<!--more-->

# 盒模型
先了解一下什么是盒模型:
就我目前所学的，所谓盒子就是一个一个小区域，用来装内容的，所有可视内容都要放在盒子里，无论是行内，块级还是行内块元素，都可以是盒子模型。
![1](Unit3-盒模型/1.png)

这是默认的盒子模型，其中盒子的宽高并不影响padding等值，就是可以填充内容区域的高度。
而在我们给元素设置为box-sizing: border-box后会改变盒模型
![2](Unit3-盒模型/2.png)
这会让我们能更好的掌控盒模型，因为我们此时设置的宽高就是该盒子整体的宽高，包括padding和border等值。

所以在写CSS的时候还是选择border-box吧，省事嘛。
可以全局设置修改盒模型所以在写CSS的时候还是选择border-box吧，省事嘛。
可以全局设置修改盒模型
```css
:root{
    box-sizing: border-box;
}

*
::before
::after{
    box-sizing: border-box;
}
```
在使用盒子模型后，对于添加一丢丢内外边距的时候，我们可以不用这么写
```
        div{
            width: 29%;
            magrin-left: 1%;
        }
```
别问，问就是拉，书上给出了一种极为高级的写法
```
        div{
            width: calc(30% - 1.5em);
            magrin-left: 1.5em;
        }
```
这样不仅能使代码更简单易懂(第一种乍一看看到个29%会感觉莫名其妙)，还能实现不同单位的搭配，% + em ，这谁敢想！？ 总之就是好用就得了，好用且高级。

# 元素高度问题
在写项目的时候我们一般不会给盒子限定高度，因为普通的文档流(浏览器默认的布局模型)是为有限的宽度和无限的高度设计的，所以容器的高度应该由其中的内容决定而不能写定。

## 内容溢出的问题
如果盒子的高度写死了，里面的内容又太多就会出现内容溢出，对此可以为其添加overflow属性，该属性有四个值

        visible: 默认值，会看见溢出
        hidden: 隐藏溢出内容
        scroll: 容器出现滚动条 不管有没有溢出
        auto: 只有溢出才会出现滚动条
        一般都用auto而不是scroll

也存在水平溢出的问题，比如一个贼长的url地址，如果让他溢出换行的话会很难看，贼难看，我们就可以用overflow-x单独控制水平溢出(-y单独控制垂直溢出)

## 百分比高度备选方案
之前我们知道width和heigh这些可以设置百分比数值，是相对于他们的父元素解析的，所以想在某一个容器上用百分比数值必须确定该容器的父元素对应的属性有明确的值，不然会造成死循环，最好的选择就是用vh啦，当然也有其他的方法，比如**等高列**

所谓等高列，就是一行内所有的容器都是等高的，无论增加哪个容器的高度，其余容器都会相应变高，有种苟富贵勿相忘的感觉。实现等高列的方法就是让那些容器自己根据自己的内容决定高度，然后拔高较矮的列就行。 有CSS表格和Flexbox两种方法实现。

### CSS表格布局

表格布局自然就是把父容器的display属性的值设置成table，然后把想要实现等高列的容器的display属性设置为table-cell就行。此时有两个注意点:
1. 显示为table的元素宽度不会扩展到100%，所以要写定width: 100%
2. 此时margin值就会失效，等高列之间互相贴合，这时就要用上表格元素的属性 border-spacing来定义单元格之间的间距，该属性接收两个值，分别设置水平间距和垂直间距，但此时直接写的话，由于是水平间距的原因，这就会导致等高列无法跟头部左右两边对齐了，这时就需要在表格容器外面在包一个元素，给他添上一个负的外边距值就可以了，说起来有点复杂，直接看代码

```css
.container{
    margin-left: -1em;
    margin-right: -1em;
}

.table{
    display: table;
    width: 100%;
    border-spacing: 1em 0;
}

.table-cell{
    display: table-cell;
}

```

### Felxbox
还可以用flex布局实现等高列**不考虑兼容性就用 遇事不决flex！！！** ，因为flex布局默认产生等高的元素，只要一句display:flex;就能解决，后续会有更详细的介绍，左侧栏搜索即可啦。

最后 ***一定不要轻易明确元素的高度！！！***

## min-height和max-height 

我们可以用这两个属性限定元素的高度在一个范围内，类似的是min-width 和 max-width


# 垂直居中和水平居中
这部分是个大头，因为能实现的方法太多了，要根据不同的情况给出不同的方法，这里说一些可能会用到的方法，我们先看水平居中，这部分相对于而言比较简单 **因为一般情况下容器的宽度是给定的而高度是未给定的**

## 水平居中
那么有哪些方法可以实现水平居中呢？

### 行内元素水平居中
这个简单，行内元素就是p啊，button啊啥的这写文本内容，就一句text-align: center;就可以了

### 块级元素水平居中

定宽的水平居中: 该元素宽度给定的话，就 margin: 0 auto;**

不定宽的水平居: 
1. 该元素宽度不固定，就给要居中的元素设置display:table; margin: 0 auto; 
2. 该元素设置display: inline-block,父元素设置text-align:center;
3. flex布局: 只需把要处理的块状元素的父元素设置display:flex,justify-content:center;
4. position + 负margin；
5. position + margin：auto
6. position + transform；
   
这里方法4、5、6同下面垂直居中一样的道理，只不过需要把top/bottom改为left/right，在垂直居中部分会详细讲述

## 垂直居中

### 文本(行内元素)垂直居中
单行:设置paddingtop=paddingbottom；或 设置line-height=height(容器高度)；

多行:通过设置父元素table，子元素table-cell和vertical-align;vertical-align:middle的意思是把元素放在父元素的中部


### 块级元素垂直居中

1. flex布局:在需要垂直居中的父元素上，设置display:flex和align-items：center **父元素的height值必须设置**
2. 定位子绝父相后，需要居中元素设置top: 50%; margin-top: -(自身高度的一半); 所以这个要设置该垂直居中元素的高度
3. **定位子绝父相后，需要居中元素设置top: 50%; transform: translate(0,-50%) 这个不需要知道该元素自身尺寸**，所以用这个比较好，可能会有兼容性问题

## 水平垂直居中
其实水平居中垂直居中都是为了这个服务的，因为这样最好，中国人嘛，讲究规整嘿嘿，然后有以下几种方法

1. 绝对定位后加 margin: auto, (top/bottom/rigth/left都为0)
2. 绝对定位后 利用上述水平居中/垂直居中的方法4和方法2 
3. 绝对定位后 top: 50%; left: 50%;transform: translate(-50%,-50%),**在未定义尺寸的情况下**
4. flex布局 就是用上述中flex用的水平/垂直居中方法结合就行
5. table-cell 不喜欢用 就不写了


综上 其实还是推荐用flex布局和transform的，但好像都是C3的写法要考虑兼容性，不太清楚，总之看情况来吧。

***这里明确一下负margin，如果给margin的left或者top设置负值，则元素会相应的向左或者上方移动，若是设置right和bottom为负值，则会将相应位置的后续元素拉过来与自身重叠，在有些地方会有妙有，比如上面说的实现居中的时候，但还是减少使用***

# 外边距折叠

## 概念
 在CSS中，两个或多个毗邻的普通流中的盒子（可能是父子元素，也可能是兄弟元素）在垂直方向上的外边距会发生叠加，这种形成的外边距称之为外边距折叠。
  关键字：毗邻、两个或多个、垂直方向和普通流

## 什么时候会发生外边距折叠呢
普通流中的块级元素的margin-bottom永远和它相邻的下一个块级元素的margin-top叠加，除非相邻的兄弟元素clear
普通流中的块级元素（没有border-top、没有padding-top）的margin-top和它的第一个普通流中的子元素（没有clear）发生margin-top叠加
普通流中的块级元素（height为auto、min-height为0、没有border-bottom、没有padding-bottom）和它的最后一个普通流中的子元素（没有自身发生margin叠加或clear）发生margin-bottom叠加
如果一个元素的min-height为0、没有border、没有padding、高度为0或者auto、不包含子元素，那么它自身的外边距会发生叠加

## 外边距折叠的大小
     1.两个相同大小的正数：取某个外边距的值。即30px与30px发生折叠，折叠后的值为30px。
     2.两个不同大小的正数：取较大的外边距的值。即30px与20px发生折叠，折叠后的值为30px。
     3.一正一负：取正数与负数的和。即30px与-20px发生折叠，折叠后的值为10px。
     4.相同大小的负数：同相同大小正数。-30px与-30px折叠，折叠后为-30px。
     5.不同大小负数： 取绝对值较大的负数。-30px与-20px折叠，折叠后为-30px。

## 避免外边距的方法
1. 对容器添加overflow: auto , 防止内部元素的外边距与容器外部外边距折叠，这种方法副作用最小
2. 在发生折叠的外边距之间加上边框或者内边距
3. 如果容器为浮动元素，行内块元素，绝对定位或者固定定位时
4. 使用Flexbox布局或者网格布局
5. 元素显示为, table-cells, table-row等
   
即，创建了BFC的元素不会发生外边距折叠的问题，关于BFC请[点击这里](https://www.cnblogs.com/libin-1/p/7098468.html)

# 猫头鹰选择器 * + *
叫这个名字的原因无它 就是因为长的像猫头鹰而已，其中 +： 相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素。
猫头鹰选择器会选中**页面上有着相同父元素的非第一个子元素**(比如ul里面会选择第二个往后的所有li)
一般用于给他们添加一个统一的margin-top解决边距问题

 ```css
 * + * {
     margin-top: 2em;
 }
 ```

 最后，有:
 1. ***tag + tag 会选择除第一个tag之外的所有tag***
 2. ***因为'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' +'border-right-width' + 'margin-right' = width of containing block ，在我们只设置了margin-left: auto 后为满足上式 ，就有margin-left =  width,故元素会直接到容器的最右边***


 ok 第三章的笔记就到这，书上的基础部分也结束了，接下来就到了关键的布局咯





