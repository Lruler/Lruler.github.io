---
title: react2
date: 2021-06-24 18:54:20
tags: react
categories: 笔记
---

看 三天精通react/ React学习手册 / the-road-to-learn-react / 读后感
<!--more-->

```
两个问题
setstate的同步异步问题
setstate的调用生命周期问题
```

《三天精通react》
# useState
先明白什么是组件
这里书上和木犀101都给出了一个类似的概念
**UI=f(x)**
即每一个组件就是一个纯函数，不过这个纯函数的返回值是UI结构，函数里面的x就是用于驱动UI更新的数据，这种通过数据驱动 UI 变化的方式，叫**声明式编程**，而直接对UI操作的叫命令行编程，比如原生DOM，声明式在思维上是更高级的编程范式。

无状态组件就是没state，所有的数据通过props接收，这里有个写法就是css当作变量而不是写在css文件里面

```jsx
import React from "react";

export function Button(
  props: React.PropsWithChildren<{ color?: string; onClick?: () => void }>
) {
  const { color, children, onClick } = props;
  const style = {
    backgroundColor: color,
    border: "none",
    padding: "4px 6px",
    color: "white",
    cursor: "pointer"
  };

  return (
    <button onClick={onClick} style={style}>
      {children}
    </button>
  );
}
```

有状态组件 就是带着state的组件，Hooks里面就用usestate(),具体用法也都知道，但切记不要写在生命周期函数里面，比如ComponentDidUpdate(),这样会导致无限重绘，页面假死。

即setstate()函数用于驱动页面更新，但UI更新总是有原因的
* ⽤⼾触发的交互，如：键盘输⼊、⿏标点击、屏幕滑动等
* 定时器的触发，如： setTimeout 、 requestAnimationFrame 、 setInterval
* IO 事件回调触发，如：AJAX 请求返回的回调

总结就是， setCount 操作必须在某个回调中调⽤，不应该出现在生命周期函数的同步调⽤栈中执⾏。

useState()接收的参数是state的初始值，但在下列情况下需要接收函数作为参数

```js
 //初始状态需要复杂计算
 // bad
 const initialCount = props.data.reduce((acc, cur) => acc + cur, 0);
 const [count, setCount] = useState(initialCount);

 // good
 const [count, setCount] = useState(() => props.data.reduce((acc, cur) => acc + cur, 0));


 //初始状态是复杂对象
 // bad
 const [initialState, setState] = useState({
 attr1: 'xxxx',
 attr2: 'xxxx',
 ...
 attr50: 'xxxx',
 });
 // good
 const [initialState, setState] = useState(() => ({
 attr1: 'xxxx',
 attr2: 'xxxx',
 ...
 attr50: 'xxxx',
 }));
```
传函数的好处是，当依赖 state ⾃⾝最新状态来更新状态时，不需要访问外部变量


# useRef
当需要存放⼀个数据，需要⽆论在哪⾥都取到最新状态时，需要使⽤ useRef。（实际开发少用，影响效率）
