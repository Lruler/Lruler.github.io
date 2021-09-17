---
title: Router base
date: 2021-06-12 15:02:56
tags: react
categories: 笔记
---                   

一篇关于使用路由的Blog

<!-- more -->
# 路由简介 
在之前的react笔记中，我们已经简单的了解过路由

## 1.SPA的理解
1.单页Web应用（single page web application，SPA）。
2.整个应用只有一个完整的页面。
3.点击页面中的链接不会刷新页面，只会做页面的局部更新。
4.数据都需要通过ajax请求获取, 并在前端异步展现。


## 2.路由的理解

1. 什么是路由?
一个路由就是一个映射关系(key:value)
key为路径, value可能是function或component
2. 路由分类
后端路由：
1)理解： value是function, 用来处理客户端提交的请求。
2)注册路由： router.get(path, function(req, res))
3)工作过程：当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据
前端路由：
1)浏览器端路由，value是component，用于展示页面内容。
2)注册路由: 
`<Route path="/test" component={Test}>`
3)工作过程：当浏览器的path变为/test时, 当前路由组件就会变为Test组件


## 3.react-router-dom的理解
1. react的一个插件库。
2. 专门用来实现一个SPA应用。
基于react的项目基本都会用到此库。


## 4.内置组件
```
        1.<BrowserRouter> //支持H5history，用于包裹顶层组件，监听该组件中URL的变化，管理路由
        2.<HashRouter> //作用与前者相同，但使用URL的hash部分，不支持location.key或location.state
        3.<Route> //负责把一般组件注册为路由组件
        4.<Redirect> //类似与default，所有路径都不被匹配时就用它指定的路由组件
        5.<Link> // 前往路由组件，代替<a>标签
        6.<NavLink> // 与<Link>相同，不过会给点击后的链接默认添加一个active类名，与bootstrap联用实现
        高亮效果，具体样式可以自己封装
        7.<Switch> // 实现单一匹配，

        // 其它
        1.history对象
        2.match对象
        3.withRouter函数
        4.Route组件中exact属性的作用：路径进行精确匹配，不会存在包含关系
```

这里我们可以简略的说一下路由的发展哈
**最早的最早，用户加载网页，是要等整个页面全部加载完才行，很麻烦很耗时间，后来呢，微软就搞了AJAX的基本概念，也就是XMLHttpRequest的前身，有了AJAX后，用户就不需要每次载完了，比如图片GIF什么的加载慢，就先往后靠靠，我看会儿文字，而不是等你图片加载好我才能看到文字。再后来，Google Map展现了AJAX的真正魅力，让其不仅仅局限于简单的数据和页面交互，为后来异步交互体验方式的繁荣发展带来了根基。**


**而异步交互体验的更高级版本就是 SPA—— 单页应用。单页应用不仅仅是在页面交互是无刷新的，连页面跳转都是无刷新的，为了实现单页应用，所以就有了前端路由。**


# 路由的使用
先要从'react-router-dom'中引入所需要的。

## 用&lt;Link &gt; 代替&lt;a &gt;

原生Html中，用a标签实现不同页面的跳转：

`<a herf='/path'>tab</a>`

而在路由中就该用Link实现单页面跳转：

`<Link to='/'>tab</Link>`

to的值应和路由组件的path一致达成匹配

**&lt;NavLink&gt;中有个属性是activeClassName,默认是active，修改它的值就可以做到给点击的链接添加一个修改内容的类名**

## 注册路由
确定好要跳转的组件后，用Route把一般组件注册成路由组件，路由组件因为类似一个单页面，就要把他们放在一个名为pages的文件夹中和一般组件区分开。
然后再用Switch包裹这些路由组件，实现单一匹配。(防止一个路径匹配多个组件出现bug)
最后可以加上Redirect，表示默认

```js
<Switch>
	<Route path="/about" component={About}/>
	<Route path="/home" component={Home}/>
	<Route path="/home" component={Test}/> //这个就不会被显示
    <Redireact to='/about'/>
</Switch>
```

别忘了添加路由器
```js
<BrowserRouter>
	<App/>
</BrowserRouter>,
```

## 路由组件接收到到参数

路由组件会默认接到三个props
```
        history:
            go: f go(n); // n就是跳转到第几个浏览页面，可以是负数
            goBack: f gouBack();
            goForward: f goForWard();
            push: f push(path,state); //state就是指下文的state参数
            repalce: f replace)(path,state);

        location:
            pathname: "/aboplut"
			search: ""
			state: undefined

        match:
            params: {}
            path: "/about"
            url: "/about"
```

## 嵌套路由(多级路由)
1. 注册子路由时要写上父路由的path值
2. 路由的匹配是按照注册路由的顺序进行的

## 解决多级路径刷新页面样式丢失的问题
1. public/index.html 中 引入样式时不写 ./ 写 / （常用）
2. public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）
3. 使用HashRouter
   
## 路由的严格匹配与模糊匹配
1. 默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）
2. 开启严格匹配：&lt;Route exact={true} path="/about" component={About}/ &gt;
3. 严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由



## 向路由组件传递参数
```
        1.params参数
        路由链接(携带参数)：<Link to='/demo/test/tom/18'}>详情</Link>
        注册路由(声明接收)：<Route path="/demo/test/:name/:age" component={Test}/>
        接收参数：this.props.match.params

        2.search参数/query参数
        路由链接(携带参数)：<Link to='/demo/test?name=tom&age=18'}>详情</Link>
        注册路由(无需声明，正常注册即可)：<Route path="/demo/test" component={Test}/>
        接收参数：this.props.location.search
        备注：获取到的search是urlencoded编码字符串，需要借助querystring解析
                        
        3.state参数
        路由链接(携带参数)：<Link to={ { pathname:'/demo/test',state:{ name:'tom',age:18 } } }>详情</Link> 且不在地址栏上显示
        注册路由(无需声明，正常注册即可)：<Route path="/demo/test" component={Test}/>
        接收参数：this.props.location.state
        备注：刷新也可以保留住参数(和浏览器的history有关，如果历史记录被清空则刷新就会丢失参数)
```

## withRouter

就是把一个一般组件加工，让其可以使用一些路由组件的API，比如props.history
用法:
```js
import {withRouter} from 'react-dom-router';

// 某组件Demo

export default withRouter(Demo)
```
这样Demo组件就变成了一个新组件，不同于一般组件和路由组件

## BrowserRouter与HashRouter的区别
```
1.底层原理不一样：

		BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
		HashRouter使用的是URL的哈希值。

2.path表现形式不一样

	    BrowserRouter的路径中没有#,例如：localhost:3000/demo/test
		HashRouter的路径包含#,例如：localhost:3000/#/demo/test

3.刷新后对路由state参数的影响
		(1).BrowserRouter没有任何影响，因为state保存在history对象中。
		(2).HashRouter刷新后会导致路由state参数的丢失！！！
			
4.备注：HashRouter可以用于解决一些路径错误相关的问题。
```