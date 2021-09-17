---
title: react源码初探
date: 2021-09-15 10:45:41
tags: react
categories: 笔记
---
总算到这一步了 看react源码部分的笔记
<!--more-->

# Virtual DOM 
Virtual DOM 是 React 的核心与精髓所在,React就是靠着VDOM和高效的diff算法大量减少了渲染DOM的代价。
VDOM中转换成浏览器中真实的DOM树，即VDOM其实也存在于树这种数据结构中，那他就会有自己的节点，他的节点被称为 ReactNode， 分为3种类型 ReactElement、ReactFragment 和ReactText