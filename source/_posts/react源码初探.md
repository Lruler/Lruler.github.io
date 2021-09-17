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
VDOM中转换成浏览器中真实的DOM树，即VDOM其实也存在于树这种数据结构中，那他就会有自己的节点，他的节点被称为 ReactNode， 分为3种类型 ReactElement、ReactFragment 和ReactText，其中，ReactElement 又分为 ReactComponentElement 和 ReactDOMElement。
先看看这些类型源码，涉及到Ts部分、
```ts
type ReactNode = ReactElement | ReactFragment | ReactText;
type ReactElement = ReactComponentElement | ReactDOMElement;
type ReactDOMElement = { type : string,
props : {
children : ReactNodeList, className : string,
etc.
},
key : string | boolean | number | null, ref : string | null
};
type ReactComponentElement<TProps> = { type : ReactClass<TProps>,
props : TProps,
key : string | boolean | number | null, ref : string | null
};
type ReactFragment = Array<ReactNode | ReactEmpty>; type ReactNodeList = ReactNode | ReactEmpty;
type ReactText = string | number;
type ReactEmpty = null | undefined | boolean;
```
然后我们写JSX的时候，react会帮我吗掉createElement方法构造虚拟节点，这是个蛮重要的部分，直接上代码
```ts
// createElement 只是做了简单的参数修正，返回一个 ReactElement 实例对象， // 也就是虚拟元素的实例
ReactElement.createElement = function(type, config, children) {
// 初始化参数
var propName;
var props = {}; 
var key = null; 
var ref = null; 
var self = null; 
var source = null;


// 如果存在 config，则提取里面的内容 if (config != null) {
if (config != null) {
  ref = config.ref === undefined ? null : config.ref;
  key = config.key === undefined ? null : '' + config.key;
  self = config.__self === undefined ? null : config.__self; 
  source = config.__source === undefined ? null : config.__source; 
// 复制 config 里的内容到 props(如 id 和 className 等)
  for (propName in config) {
    if (config.hasOwnProperty(propName) && !RESERVED_PROPS.hasOwnProperty(propName)) {
      props[propName] = config[propName];
    }
  } 
}

// 处理 children，全部挂载到 props 的 children 属性上。如果只有一个参数，直接赋值给 children， // 否则做合并处理
var childrenLength = arguments.length - 2;
if (childrenLength === 1) {
  props.children = children;
} 
else if (childrenLength > 1) {
  var childArray = Array(childrenLength); 
  for (var i = 0; i < childrenLength; i++) {
    childArray[i] = arguments[i + 2]; 
  }
  props.children = childArray; 
}

// 如果某个 prop 为空且存在默认的 prop，则将默认 prop 赋给当前的 prop 
if (type && type.defaultProps) {
  var defaultProps = type.defaultProps;
  for (propName in defaultProps) {
    if (typeof props[propName] === 'undefined') {
       props[propName] = defaultProps[propName];
    } 
  }
}
// 返回一个 ReactElement 实例对象
return ReactElement(type, key, ref, self, source, ReactCurrentOwner.current, props); };

```
## 创建组件
Virtual DOM 模型通过 createElement 创建虚拟元素，那又是如何创建组件的呢?