---
title: react
date: 2021-06-09 20:54:35
tags: react
categories: 笔记
---


**写在前面**


**终于可以重新在新电脑(mbp)上写博客啦 话说为什么我的笔记是直接从react开始的...因为是刚好在学react 所以就先写点总结啦，后面会陆续补上html，css,Js的相关笔记博客的。感觉mos系统也要写点笔记，iterm2的命令行写点总结啥的 有点好用的嗦**
正式开始react相关笔记啦

<!-- more -->
# 对React的理解


## 大一


1. 具体的理解有点说不上来，可能还是自己学太浅来。等后续理解深了用的多了再回来补。按照教程官网学长等各方面说法，react就是通过数据驱动虚拟DOM更新，不需要实际去操作DOM，大部分情况下state有更改那么UI就会出现变化，而state的更改又基于用户的操作，总之很方便。 
2. 运用diff算法最小化重绘页面，加上操作虚拟DOM，就是react高效的两个原因
3. 虚拟DOM其实就是一种特殊的JS一般对象，后续会被react渲染成真实DOM
4. 一般运用JSX语法


# React基本概念


概念好像和理解差不多...Emmmmm，那就写点详细的东西


## 关于JSX


1. 全称:  JavaScript XML
2. react定义的一种类似于XML的JS扩展语法: JS + XML本质是React.createElement(component, props, ...children)方法的语法糖
3. 作用: 用来简化创建虚拟DOM 
        1)写法：var ele = <h1>; Hello JSX! </h1>
        2)注意1：它不是字符串, 也不是HTML/XML标签
        3)注意2：它最终产生的就是一个JS对象
4. 语法规则
5. jsx语法规则：
		1).定义虚拟DOM时，不要写引号。
		2).标签中混入JS表达式时要用{}。
		3).样式的类名指定不要用class，要用className。
		4).内联样式，要用style={{key:value}}的形式去写。
		5).只有一个根标签
		6).标签必须闭合
		7).标签首字母
		(1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
		(2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。

6. ReactDOM.render(VDOM,RDOM)
1.语法:  ReactDOM.render(virtualDOM, containerDOM)
2.作用: 将虚拟DOM元素渲染到页面中的真实容器DOM中显示
3.参数说明
1)参数一: 纯js或jsx创建的虚拟dom对象
2)参数二: 用来包含虚拟DOM元素的真实dom元素对象(一般是一个div)

 ## tips:
 {里面放的是表达式}，表达式代表着一个值，而for循环啥的叫语句，不会产生值


 ## 组件:


 **比模块还牛的东西，就对一个页面来说拆就完了，拆到你想不出来这个组件到底能叫什么名字的时候就代表这个组件你已经成功的拆到底了**
 ```
 1. 类式组件
   (1)出hooks之前是主流，基本都用这个，hooks出来后也能用，但最好还是hooks嘛
   (2)一些类的相关知识就详见于JS总结啦
   (3)执行了ReactDOM.render(&lt;MyComponent/>.......之后，发生了什么？
		1.React解析组件标签，找到了MyComponent组件。
		2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
		3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
    (4)组件里面的render()方法是挂在类的原型对象上的，供组件使用，组件实例对象调用这个方法，那么就是render中的this就指向这个组件实例
 2. 函数式组件
   (1)用hooks 贼香 hooks后面有笔记介绍的
   (2)执行了ReactDOM.render(&lt; MyComponent/>.......之后，发生了什么？
		1.React解析组件标签，找到了MyComponent组件。
		2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中
```

## 受控组件与非受控组件
1.表单非受控组件，提交的数据现用现取
就是用ref操作实际DOM，比如在表单中输入结束后，给button添加一个函数提交数据

2.表单受控组件，提交的数据存放在state中
在HTML的表单元素中，它们通常自己维护一套state，并随着用户的输入自己进行UI上的更新，这种行为是不被我们程序所管控的。而如果将React里的state属性和表单元素的值建立依赖关系，再通过onChange事件与setState()结合更新state属性，就能达到控制用户输入过程中表单发生的操作。被React以这种方式控制取值的表单输入元素就叫做受控组件。



## 高阶函数和函数的柯里化
1. 当一个函数接受的参数是函数或者返回值是函数，那么这个函数就是高阶函数
2. 柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式（给注册事件的回调函数传参）
3. 不用柯里化的形式：在内莲函数中写一个回调 （）=> func ,这个回调func就作为事件发生时的回调

# React三大核心属性

## State

1. state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)
2. 组件被称为”状态机”, 通过更新组件的state来更新对应的页面显示(重新渲染组件)
3. 总之就是 UI里面所有所有的可视改变，背后都代表着一种state某个值的改变，也就是说state中的数据驱动UI视图更新
state中的数据不可直接更改，必须利用setState() 方法重新渲染页面，是一个合并的动作而不是替换

## props

***概念：***
1. 每个组件对象都会有props(properties的简写)属性
2. 组件标签的所有属性都保存在props中(props也是个对象)

***作用：***
1. 通过标签属性从组件外向组件内传递变化的数据
2. 注意，组件内部不要修改react，广泛用于父子组件传递数据
   
## refs与事件处理
1. 理解： 一组kv对象,key是refs，value是当前使用ref的一个节点，用于标识节点（可代替id)
就是获取当前用ref标识的节点 有点类似于操作真实的DOM了，所以实际开发中最好不要使用，不然容易引起效率问题。

2. 使用

```
(1). 字符串形式 //已经废弃了
<input ref="input1"/>
(2). 回调形式
<input ref={(c)=>{this.input1 = c}}/>
(3). createRef
myRef = React.createRef()  <input ref={this.myRef}/>

事件处理
三种形式，和原生一样，但最好还是用内联的方式，因为其他两种在操作真实的DOM
a. React使用的是自定义(合成)事件, 而不是使用的原生DOM事件 —————— 为了更好的兼容性
b. React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ————————为了的高效
```

# 生命周期
新钩子
![1](react/1.png)
旧钩子
![2](react/2.png)
区别
```
 1).废弃了三个钩子
 1.componentWillMount
 2.componentWillReceiveProps
 3.componentWillUpdate
 2).重要的钩子
 1.render：初始化渲染或更新渲染调用
 2.componentDidMount：开启监听, 发送ajax请求，订阅与发布，开启定时器等等
 3.componentWillUnmount：做一些收尾工作, 如: 清理定时器
 ```



# 脚手架
`npx create-react-app xx`
即用于快速创建一个基于react模版的项目


## 1.项目结构
```
public ---- 静态资源文件夹
		favicon.icon ------ 网站页签图标
		index.html -------- 主页面
		logo192.png ------- logo图
		logo512.png ------- logo图
		manifest.json ----- 应用加壳的配置文件
		robots.txt -------- 爬虫协议文件
src ---- 源码文件夹
		App.css -------- App组件的样式
		App.js --------- App组件
		App.test.js ---- 用于给App做测试
		index.css ------ 样式
		index.js ------- 入口文件
		logo.svg ------- logo图
		reportWebVitals.js
			--- 页面性能分析文件(需要web-vitals库的支持)
		setupTests.js
			---- 组件单元测试的文件(需要jest-dom库的支持)
```
 
## 2.编码流程
1. 拆分组件: 拆分界面,抽取组件
2. 实现静态组件: 使用组件实现静态页面效果
3. 实现动态组件
   
	3.1 动态显示初始化数据
		3.1.1 数据类型
		3.1.2 数据名称
		3.1.2 保存在哪个组件?
	3.2 交互(从绑定事件监听开始)
	


## 3.具体项目: 详情可见todolist分享


# React AJAX

## 说明

1.React本身只关注于界面, 并不包含发送ajax请求的代码
2.前端应用需要通过ajax请求与后台进行交互(json数据)
3.react应用中需要集成第三方ajax库(或自己封装)

## 常用AJAX库
1.jQuery: 比较重, 如果需要另外引入不建议使用
2.axios: 轻量级, 建议使用
1)封装XmlHttpRequest对象的ajax
2) promise风格
3)可以用在浏览器端和node服务器端

**关于AJAX/axios和fetch的区别与运用，有一篇专门的博客 去<——搜一下就好啦（可能还放在了GitHub搜索用户里面**




# React 路由
 
 
## 1.SPA的理解
1.单页Web应用（single page web application，SPA）。
2.整个应用只有一个完整的页面。
3.点击页面中的链接不会刷新页面，只会做页面的局部更新。
4.数据都需要通过ajax请求获取, 并在前端异步展现。


## 2.路由的理解
1.什么是路由?
1.一个路由就是一个映射关系(key:value)
2.key为路径, value可能是function或component
2.路由分类
1.后端路由：
1)理解： value是function, 用来处理客户端提交的请求。
2)注册路由： router.get(path, function(req, res))
3)工作过程：当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据
2.前端路由：
1)浏览器端路由，value是component，用于展示页面内容。
2)注册路由: `<Route path="/test" component={Test}>`
3)工作过程：当浏览器的path变为/test时, 当前路由组件就会变为Test组件


## 3.react-router-dom的理解
1.react的一个插件库。
2.专门用来实现一个SPA应用。
基于react的项目基本都会用到此库。


## 4.内置组件

        1.<BrowserRouter>
        2.<HashRouter>
        3.<Route>
        4.<Redirect>
        5.<Link>
        6.<NavLink>
        7.<Switch>

        // 其它
        1.history对象
        2.match对象
        3.withRouter函数


# 流行的开源React UI组件库
I. material-ui(国外)
1.官网: http://www.material-ui.com/#/
2.github: https://github.com/callemall/material-ui
II. ant-design(国内蚂蚁金服) 
1.官网: https://ant.design/index-cn
Github: https://github.com/ant-design/ant-design/

`关于蚂蚁金服的使用请左栏搜索啦`



# setState

## setState更新状态的2种写法

```
(1). setState(stateChange, [callback])------对象式的setState
    1.stateChange为状态改变对象(该对象可以体现出状态的更改)
    2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
					
(2). setState(updater, [callback])------函数式的setState
    1.updater为返回stateChange对象的函数。
    2.updater可以接收到state和props。
    4.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。
总结:
	1.对象式的setState是函数式的setState的简写方式(语法糖)
	2.使用原则：
		(1).如果新状态不依赖于原状态 ===> 使用对象方式
		(2).如果新状态依赖于原状态 ===> 使用函数方式
		(3).如果需要在setState()执行后获取最新的状态数据, 
		要在第二个callback函数中读取
```



# lazyLoad

## 路由组件的lazyLoad

```js
	//1.通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
	const Login = lazy(()=>import('@/pages/Login'))
	
	//2.通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
	<Suspense fallback={<h1>loading.....</h1>}>
        <Switch>
            <Route path="/xxx" component={Xxxx}/>
            <Redirect to="/login"/>
        </Switch>
    </Suspense>
```


# Fragment

## 使用

	<Fragment><Fragment>
	<></>

## 作用

可以不用必须有一个真实的DOM根标签了

<hr/>


# Context

## 理解

 一种组件间通信方式, 常用于【祖组件】与【后代组件】间通信

## 使用

```js
//1) 创建Context容器对象：
	const XxxContext = React.createContext()  
	const {Provider,Consumer} = XxxContext
	
//2) 渲染子组时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据：
	<xxxContext.Provider value={数据}>
		子组件
    </xxxContext.Provider>
    
//3) 后代组件读取数据：

	//第一种方式:仅适用于类组件 
	  static contextType = xxxContext  // 声明接收context
	  const {value} = this.context // 读取context中的value数据
	  
	//第二种方式: 函数组件与类组件都可以
	  <xxxContext.Consumer>
	    {
	      value => ( // value就是context中的value数据
	        要显示的内容
	      )
	    }
	  </xxxContext.Consumer>
```

## 注意

	在应用开发中一般不用context, 一般都它的封装react插件

<hr/>


# 组件优化

## Component的2个问题 

1. 只要执行setState(),即使不改变状态数据, 组件也会重新render()

 2. 只当前组件重新render(), 就会自动重新render子组件 ==> 效率低

## 效率高的做法

 只有当组件的state或props数据发生改变时才重新render()

## 原因

 Component中的shouldComponentUpdate()总是返回true

## 解决

	办法1: 
		重写shouldComponentUpdate()方法
		比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false
	办法2:  
		使用PureComponent 从'react'中引进
		PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true
		注意: 
			只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false  
			不要直接修改state数据, 而是要产生新数据
	项目中一般使用PureComponent来优化

<hr/>


# render props (类似于Vue的插槽技术 )

## 如何向组件内部动态传入带内容的结构(标签)? 

	Vue中: 
		使用slot技术, 也就是通过组件标签体传入结构  <AA><BB/></AA>
	React中:
		使用children props: 通过组件标签体传入结构
		使用render props: 通过组件标签属性传入结构, 一般用render函数属性

## children props

	<A>
	  <B>xxxx</B>
	</A>
	xxx会在B组件中用 this.props.children接收
	问题: 如果B组件需要A组件内的数据, ==> 做不到 

## render props

	<A render={(data) => <C data={data}></C>}></A>
	A组件: {this.props.render(内部state数据)}
	C组件: 读取A组件传入的数据显示 {this.props.data} 



<hr/>

# 错误边界

## 理解：

错误边界：用来捕获后代组件错误，渲染出备用页面

## 特点：

只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误

## 使用方式：

getDerivedStateFromError配合componentDidCatch

```js
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true,
    };
}

componentDidCatch(error, info) {  //是后代组件渲染出错后调用的生命周期函数
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```

# 组件通信方式总结

## 方式：

		props：
			(1).children props
			(2).render props
		消息订阅-发布：
			pubs-sub、event等等
		集中式管理：
			redux、dva等等
		conText:
			生产者-消费者模式

## 组件间的关系

		父子组件：props
		兄弟组件(非嵌套组件)：消息订阅-发布、集中式管理
		祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(用的少)


# 消息订阅与发布机制
1)import PubSub from 'pubsub-js' //引入
2)PubSub.subscribe('msg', function(data){ }); //订阅
3)PubSub.publish('msg', data) //发布消息

例如

```js
// 某个组件中订阅消息
componentDidMount() {
this token = PubSub.subscribe('msg',function(){/*函数体*/ });
}
// 卸载组件后取消订阅
ComponentWillUnmount() {
		PubSub.unsubscribe(this.token)
	}


//某个组件发布消息
PubSub.publish('masg',data)
// 一般发过去的是state对象，然后在订阅消息消息的function里面写setstate()
```