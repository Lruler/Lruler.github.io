---
title: todolist-hooks
date: 2021-05-09 10:56:45
tags: [react,project]
categories: project
---

用hooks写了份todolist，刚好就借这份project写出一些hooks的使用方法
<!-- more -->
# hooks基本语法介绍

## State Hook 

```
(1). State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作
(2). 语法: const [xxx, setXxx] = React.useState(initValue)  
(3). useState()说明:
        参数: 第一次初始化指定的值在内部作缓存
        返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
(4). setXxx()2种写法:
        setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
        setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值
```



## Effect Hook

        (1). Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
        (2). React中的副作用操作:
                发ajax请求数据获取
                设置订阅 / 启动定时器
                手动更改真实DOM
        (3). 语法和说明: 
                useEffect(() => { 
                // 在此可以执行任何带副作用操作
                return () => { // 在组件卸载前执行
                    // 在此做一些收尾工作, 比如清除定时器/取消订阅等
                }
                }, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
                第二个参数限制了调用这个hooks的范围，则就会只有在statevalue发生改变时调用这个hook
            
        (4). 可以把 useEffect Hook 看做如下三个函数的组合
                componentDidMount()
                componentDidUpdate()
                componentWillUnmount() 

## Ref Hook

        (1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
        (2). 语法: const refContainer = useRef()
        (3). 作用:保存标签对象,功能与React.createRef()一样

## 自定义Hook与其他Hooks API
这部分内容比较多，详情右边搜索关于hooks的详细博客，这里就先介绍最基础的三个。


# Todolist

## 组件化

了解了hooks的基本语法，我们就正式开始写这份todolist的project吧。
先看项目的基本UI
![td](/todolist-hooks/todolist.png)
这算是比较基本的UI了，我们就从上往下拆分组件，可以拆解为
                1. 头部部分的输入框:Input 这可以包括标题的Todo，因为只是基本的样式没有交互效果，可以简单的与Input放在一起，叫他header组件也没什么问题
                2. 容器组件：List 
                        3. List组件中的Item组件，这份todo就是个列表嘛，那必然就可以分类列表的容器组件和负责展示的Item组件
                4.尾部组件：Filter 其实叫他Footer组件也行，但他有三种不同的筛选状态，就叫他Filter吧

组件拆解完毕后，其中的html放在render（）/return（）里， css就作为文件放在对应的文件夹中，结构如图
![cp](/todolist-hooks/components.png)  

**这里的css样式文件我就没有拆了，因为是直接拿来的文件，不太分得清从哪开始拆，也没注释，实际自己写的时候一定要拆的**


# 组件通信方式总结
写完静态页面后就根据需求加交互效果呗，这里就会涉及到react组件之间如何传递数据，下面是一些总结

## 组件间的关系：

- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）

## 几种通信方式：

		1.props：
			(1).children props
			(2).render props
		2.消息订阅-发布：
			pubs-sub、event等等
		3.集中式管理：
			redux、dva等等
		4.conText:
			生产者-消费者模式

## 比较好的搭配方式：
		父子组件：props
		兄弟组件：消息订阅-发布、集中式管理
		祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)

因为todolist算是比较简单的，我们就拿**状态提升**来写。

## 状态提升
就是把一些可以共享state的组件们的state提到他们共同的父组件中去，简单来说，这份数据你也要我也要他也要，但是我们一人写一份就很麻烦，不如把state交给一个老父亲，让他按需求发给我们就好了。
所以我们就在App组件中定义state,todos都知道是个列表嘛，我们就把todos看成一个对象数组，每个todo

                const[todos,setTodos] = useState([])

初始化好后就用props把todos传下去呗，这样每个自组件都能共享这个state了

## Input
接下来开始写第一个功能，按照组件顺序自然就从Input开始。

### 增加一个todo列表

我们给这个事件函数命名为handleAdd(),组件内的函数一般都命名为handleXx
无非就是按下回车后给todo这个空数组添加一个元素todo对象呗
先给输入框绑定键盘事件

                <input onKeyUp={handleAdd}> //onKeyUp就是键盘按下后弹起就会触发
```js
  function handleAdd(e) {
        const { keyCode, target } = e
        if (keyCode !== 13) return //判断是否为回车
        const todoObj = { id: nanoid(), content: target.value, completed: false, flag: true, editing: false } //nanoid用于唯一生成唯一id，安装后引入就行，其他的属性是根据需求来添加的
        props.addTodo(todoObj) //因为state在App中，所以调用App中的方法来进行更新
        target.value = ''  //清空输入框
    }
```

如上行所示，虽然你通过props接受到了state，可想要更新state，还得回App中，因为state在哪儿，操作state的方法就在哪儿 所以就得来个回调嘛
addTodo的具体函数如下
```js
  function addTodo(todoObj) {
        const newTodos = [todoObj, ...todos]// 注明一下为什么用扩展运算符而不用数组的push方法添加元素对象，因为push方法不是纯函数，他产生的新数组和原数组的指针指向不是同一个地址，可能回产生一些不必要的bug，所以就一般不用这些方法。
    }
```
关于纯函数以及函数的副作用等也可以右边搜索相关的详细解析博客啦

### 完成所有todo
这就是Input组件的第二个需求，也很简单，先handleCompleteAll调用App内相对应的方法，再用数组的map方法让每一个todo对象的completed属性变成true就可以了


## List

这个组件没什么好说的，就是一个容器组件，把相应的state和方法用props传给Item就行
不过还是有一个需要注意的点，就是之前说过List其实可以看作是一个数组，Item是里面的元素，所以每一个Item都应该由map方法展示，代码如下
```js
return(
        <div>
        {todos.map(todo => <Item/>)}
        <div>
)
```

## Item
这个需求就比较多啦
编辑 删除 完成
还有随着state的改变样式也跟着等等
不过具体思路还是和上面一样，也就不赘述啦

## Filter
对于需求方法也没什么好说的，还是同上
不过要注意一点，实现排他思想的tab栏切换
思路就是，把id放在state里面，每次点击一个tab栏的时候，就会传入自己的id变成state，这样只有自己的id和state吻合的时候才会实现单个tab选项卡高亮的效果


# 写在最后
上面就是关于这个小项目的总结啦，好说这也是自己写的第一个react项目，也花了一些时间，写一篇博客记录下来，算是加强理解吧

