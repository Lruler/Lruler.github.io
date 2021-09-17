---
title: fetch-搜索用户
date: 2021-05-10 19:33:16
tags: [react,project]
categories: project
---

做项目嘛，肯定得与后端合作，这就需要从他们那里拿数据过来，也就是网络请求。在引入网络请求前，我们先明白几个概念。
<!-- more -->
# XMLHttpRequest 对象
大家应该都知道XML是什么东西：
**XML(Extensible Markup Language)可扩展标记语言,跟html差不多，不过html是用来展示、表现数据，而XML则用来传输和储存数据**
那什么又是XMLHttpRequest 对象呢？
官方给的解释是
        **XMLHTTP是一组API函数集，可被JavaScript、JScript、VBScript以及其它web浏览器内嵌的脚本语言调用，通过HTTP在浏览器和web服务器之间收发XML或其它数据**
也就是说他可以用于在后台与服务器交换数据
Emmmm,听上去挺模糊的，但有了XMLhttpRequest 对象，我们就可以：

        在不重新加载页面的情况下更新网页
        在页面已加载后从服务器请求数据
        在页面已加载后从服务器接收数据
        在后台向服务器发送数据

哎呀，具体细节可以前W3C看看，我们接着往下说

# AJAX

AJAX(Asynchronous JavaScript and XML),异步的Js和XML技术，通俗点说就是通过异步编程实现网络请求数据交换等，快速实现动态网页。

## 常用的ajax库
1. jQuery: 比较重, 如果需要另外引入不建议使用
2. axios: 轻量级, 建议使用
    1)封装XmlHttpRequest对象的ajax
    2) promise风格可以用在浏览器端和node服务
   
简单的看看axios的使用
```js
//GET请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });

axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

//POST请求

axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
    .then(function (response) {
        console.log(response);
})
    .catch(function (error) {
        console.log(error);
});
```

但这不是我们今天的重点哈，这方面的内容也可以去自行找点资料去看，我们今天的重点是:


# Fetch
fetch: 原生函数，不再使用XmlHttpRequest对象提交ajax请求 且更符合关注点分离的思想


fetch与ajax的不同
1. 当接收到一个代表错误的 HTTP 状态码时，从 fetch() 返回的 Promise 不会被标记为 reject， 即使响应的 HTTP 状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。
2. fetch 不会发送 cookies

接下来就看看fetch的简单使用呗，

## GET请求
```js
fetch(url).then(function(response) {
    return response.json()
  }).then(function(data) {
    console.log(data)
  }).catch(function(e) {
    console.log(e)
  });
```

## POST请求
```js
  fetch(url, {
    method: "POST",
    body: JSON.stringify(data),
  }).then(function(data) {
    console.log(data)
  }).catch(function(e) {
    console.log(e)
  })
```

# GitHub搜索用户案例

理论知识就到这了，接下来看看实际案例吧
![1](/fetch-搜索用户/1.png)
只有一个需求，就是按下搜索后开始搜索嘛

还是一样的先拆分组件 搜索和展示两个组件而已
![2](/fetch-搜索用户/2.png)

接下来就是写网络请求啦，这个需求只涉及到GET请求，就弄的详细点吧

## 用axios
Search组件

```js
    search = () => {
        //获取用户的输入(连续解构赋值+重命名)
        const { keyWordElement: { value: keyWord } } = this
        //发送请求前通知App更新状态
        this.props.updateAppState({ isFirst: false, isLoading: true })
        //发送网络请求
        axios.get(`https://api.github.com/search/users?q=${keyWord}`).then(
            response => {
                //请求成功后通知App更新状态
                this.props.updateAppState({ isLoading: false, users: response.data.items })
            },
            error => {
                //请求失败后通知App更新状态
                this.props.updateAppState({ isLoading: false, err: error.message })
            }
        )
    }
```

## 用fetch
```js
 search = () => {  
        const{keyWordElement:{value: keyWord}}=this;
        this.props.updateAppState({isFirst:false,isLoding:true});
        fetch(`https://api.github.com/search/users?q=${keyWord}`).then(
            response =>{
                return response.json()//返回一个pending状态的Promise()
            }
        ).then(
            data => {this.props.updateAppState({isLoding:false,users: data.items})}
        ).catch(
            error => {this.props.updateAppState({ isLoading: false, err: error.message })}
        )
    };
```

## 优化后的fetch
```js
    search = async () => {
        const { keyWordElement: { value: keyWord } } = this
        this.props.updateAppState({ isFirst: false, isLoading: true })
        try {
            const response = await fetch(`https://api.github.com/search/users?q=${keyWord}`)
            const data = await response.json()
            console.log(data);
            this.props.updateAppState({ isLoading: false, users: data.items })
        }
        catch (error) {
            this.props.updateAppState({ isLoading: false, err: error.message })
        }
    }
```





























   

